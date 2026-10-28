# Day 35 – Multi-Stage Builds & Docker Hub

## Docker Hub Repository
- **Repository URL:** `https://hub.docker.com/r/vishakhach24/go-app`

## Task 1: The Problem with Large Images

1. Write a simple Go, Java, or Node.js app (even a "Hello World" is fine)
`main.go`:
```go
package main

import (
	"fmt"
	"net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello from Day 35 Multi-Stage Build!")
}

func main() {
	http.HandleFunc("/", handler)
	fmt.Println("Server running on port 8080...")
	http.ListenAndServe(":8080", nil)
}
```

2. Create a Dockerfile that builds and runs it in a single stage
`Dockerfile.single`:
```dockerfile
FROM golang:1.22-alpine

WORKDIR /app

COPY main.go .

RUN go build -o server main.go

EXPOSE 8080

CMD ["./server"]
```

3. Build the image and check its size
```bash
# Build the single-stage image
docker build -t go-single:v1 -f Dockerfile.single .
# Check image size
docker images go-single:v1
```
Output:
```text
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
go-single    v1        85e620bc0073   48 minutes ago   303MB
```

---

## Task 2: Multi-Stage Build

1. Rewrite the `Dockerfile` using **multi-stage build**

`Dockerfile`:
```dockerfile
# Stage 1: Build stage
FROM golang:1.22-alpine AS builder

WORKDIR /build

COPY main.go .

# Build statically linked binary without debug information
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o server main.go

# Stage 2: Runtime stage
FROM alpine:3.19

WORKDIR /app

# Create non-root system user and group
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copy compiled binary from builder stage
COPY --from=builder /build/server .

# Change file ownership and switch user
RUN chown appuser:appgroup /app/server
USER appuser

EXPOSE 8080

CMD ["./server"]
```

2. Build the image and check its size again
```bash
docker build -t go-multi:v1 .
```

3. Compare the two sizes
```bash
docker images | grep -E "go-single|go-multi"

# Output
go-multi              v1        e5b575c08012   42 minutes ago   16.9MB  # Contains only the compiled binary and minimal runtime OS
go-single             v1        85e620bc0073   54 minutes ago   303MB   # Contains Go compiler, build tools, package caches
```

### Why is the multi-stage image so much smaller?

In a standard single-stage build, all development tools, intermediate compilation files, and compilers remain in the final layer. Multi-stage builds let you separate the **build environment** from the **execution environment**. Using `COPY --from=builder`, only the final binary artifact is moved into the release layer, shedding hundreds of megabytes of unnecessary build-time packages.

---

## Task 3: Push to Docker Hub

1. Create a free account on [Docker Hub](https://hub.docker.com/)

2. Log in from your terminal
```bash
docker login
```

1. Tag your image properly
```bash
docker tag go-multi:v1 vishakhach24/go-app:1.0.0
docker tag go-multi:v1 vishakhach24/go-app:latest
```

2. Push it to Docker Hub
```bash
docker push vishakhach24/go-app:1.0.0
docker push vishakhach24/go-app:latest
```

3. Pull it on a different machine (or after removing locally) to verify
```bash
# Remove local cached images
docker rmi vishakhach24/go-app:1.0.0

# Pull fresh from Docker Hub and run
docker run -d -p 8080:8080 vishakhach24/go-app:1.0.0
curl http://localhost:8080
```

---

## Best Practices Applied

1. **Minimal Base Image:** Switched from a full Go SDK image down to `alpine:3.19` for runtime.
2. **Non-Root Execution:** Added `addgroup` and `adduser`, enforcing `USER appuser` to mitigate container breakout vulnerabilities.
3. **Optimized Compilation:** Built with `CGO_ENABLED=0` and `-ldflags="-w -s"` to strip debug tables and build a standalone static binary.
4. **Deterministic Versioning:** Avoided `:latest` in `FROM` directives, using explicit version tags (`golang:1.22-alpine`, `alpine:3.19`).
