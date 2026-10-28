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
