# Dockerfile: Build Your Own Images

## Your First Dockerfile

Create a folder `my-first-image` then change directory into it.
```bash
mkdir my-first-image
cd my-first-image
```

Create a `Dockerfile` file.
```bash
# Base Image
FROM ubuntu

# Updates package lists and install `curl`
RUN apt update && apt install curl -y

# Prints the message using default command
CMD ["echo", "Hello from my custom image!"]
```

Build the image.
```bash
docker build -t my-ubuntu:v1 .
```

Run a container from your image
```bash
docker run my-ubuntu:v1
```

Verify: The message prints on `docker run`
```bash
Hello from my custom image!
```

---

## Dockerfile Instructions

- `FROM` — Sets the base image to a lightweight Python environment
- `RUN` — Executes commands during the build process
- `COPY` — Copy files from local host machine into the container's directory
- `WORKDIR` — Set working directory inside the container
- `EXPOSE` — Documents that the container will listen on port 8080
- `CMD` — Default command to run when the container starts

```bash
FROM python:3.11-slim

WORKDIR /app

COPY app.py .

RUN echo "Build finished at $(date)" > build-info.txt

EXPOSE 8080

CMD ["python", "app.py"]
```

Build the image.
```bash
docker build -t my-python-app:v1 .
```

Run the container.
```bash
docker run -p 8080:8080 my-python-app:v1
```

---

## CMD vs ENTRYPOINT

### CMD

Create a file named `Dockerfile.cmd`
```bash
FROM alpine
CMD ["echo", "hello"]
```

Build it.
```bash
docker build -t test-cmd -f Dockerfile.cmd .
```

Run it **normally**.
```bash
docker run test-cmd
```
**Output**: `hello`

**Why**: Docker executes the default command exactly as written in the `CMD` instruction.

Run it with a **custom command**
```bash
docker run test-cmd echo "I changed the container"
```
**Output**: `I changed the container`

**Why**: Any command you type at the end of `docker run` completely replaces the `CMD` instruction in the Dockerfile.

### ENTRYPOINT

Create a file named `Dockerfile.entrypoint`
```bash
FROM alpine
ENTRYPOINT ["echo"]
```

Build it.
```bash
docker build -t test-entrypoint -f Dockerfile.entrypoint .
```

Run it **normally**
```bash
docker run test-entrypoint
```
**Output**: (A blank line)

**Why**: Docker runs exactly what is in the `ENTRYPOINT`. It executes `echo` with no additional arguments, printing an empty line.

Run it with **additional arguments**
```bash
docker run test-entrypoint "hello world"
```
**Output**: `hello world`

**Why**: Arguments passed at the end of `docker run` do not replace the ENTRYPOINT. Instead, they are appended to it. Docker internally ran `echo "hello world"`.

### When would you use CMD vs ENTRYPOINT?

| Instruction | Behavior | When to use it |
| :--- | :--- | :--- |
| `CMD` | Fully replaced by `docker run <args>` | When providing a **default command** that users can easily override. |
| `ENTRYPOINT` | Appends `docker run <args>` to itself | When container is designed to act as a **standalone executable utility**. |

**Bonus**: Use them together
```bash
ENTRYPOINT ["ping"]
CMD ["google.com"]
```
then
```bash
docker run my-ping # pings google.com (uses default `CMD`)
# OR
docker run my-ping localhost # pings localhost (replaces `CMD`)
```

---

## Build a Simple Web App Image

Create a folder `my-website` then change directory into it.
```bash
mkdir my-website
cd my-website
```

Create a small static HTML file (`index.html`) with any content
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Docker Website</title>
</head>
<body>
    <h1>Hello from my Nginx Docker Container!</h1>
    <p>This is a custom static website running on Nginx Alpine.</p>
</body>
</html>
```

Create a Dockerfile
```bash
FROM nginx:alpine

WORKDIR /usr/share/nginx/html

COPY index.html .

EXPOSE 80
```

Build and tag it `my-website:v1`
```bash
docker build -t my-website:v1 .
```

Run it with port mapping
```bash
docker run -p 8080:80 my-website:v1
```

Access it in your browser
```text
http://localhost:8080
```

---

## .dockerignore

Create a `.dockerignore` for `my-first-image` project
```bash
node_modules
.git
*.md
.env
```

Edit your `Dockerfile`
```bash
# Base Image
FROM ubuntu

WORKDIR /app

# Copy EVERYTHING from the current local directory into /app
COPY . .

# When the container runs, list all files (including hidden ones)
CMD ["ls", "-la"]
```

Build the image — verify that ignored files are not included
```bash
docker build -t my-first-image:v1 .
docker run my-first-image:v1
```

---

## Build Optimization

Create a new folder `my-python-app-cache` with `app.py` and `requirements.txt`.
```bash
mkdir my-python-app-cache && cd my-python-app-cache
touch app.py && touch requirements.txt
```

Create a `Dockerfile`.
```bash
FROM python:3.11-alpine
WORKDIR /app

# Inefficient order: Copying everything BEFORE installing dependencies
COPY . .
RUN pip install -r requirements.txt

CMD ["python", "app.py"]
```

Build the image.
```bash
docker build -t my-python-app-cache:v1 .
```
**Output**:
```
[1/4] FROM docker.io/library/python:3.11-alpine@sha25  6.2s
[2/4] WORKDIR /app                                     0.1s
[3/4] COPY . .                                         0.0s
[4/4] RUN pip install -r requirements.txt              2.5s
```

Edit `app.py`
```python
print("Hello World!")
```

Rebuild the image.
```bash
docker build -t my-python-app-cache:v1 .
```
**Output**:
```
[1/4] FROM docker.io/library/python:3.11-alpine@sha25  0.0s
CACHED [2/4] WORKDIR /app                              0.0s
[3/4] COPY . .                                         0.0s
[4/4] RUN pip install -r requirements.txt              3.5s
```

Reorder your `Dockerfile` so that frequently changing lines come **last**
```bash
FROM python:3.11-alpine
WORKDIR /app

# 1. Copy ONLY the dependency file first
COPY requirements.txt .

# 2. Install dependencies
RUN pip install -r requirements.txt

# 3. Copy the rest of the source code (which changes frequently) LAST
COPY . .

CMD ["python", "app.py"]
```

Rebuild the image.
```bash
docker build -t my-python-app-cache:v1 .
```
**Output**:
```
[1/5] FROM docker.io/library/python:3.11-alpine@sha256:685  0.0s
CACHED [2/5] WORKDIR /app                                   0.0s
[3/5] COPY requirements.txt .                               0.0s
[4/5] RUN pip install -r requirements.txt                   3.1s
[5/5] COPY . .                                              0.0s
```

Edit `app.py` again
```python
print("Hello DevOps Engineer!")
```

Rebuild the image.
```bash
docker build -t my-python-app-cache:v1 .
```
**Output**:
```
[1/5] FROM docker.io/library/python:3.11-alpine@sha256:685  0.0s
CACHED [2/5] WORKDIR /app                                   0.0s
CACHED [3/5] COPY requirements.txt .                        0.0s
CACHED [4/5] RUN pip install -r requirements.txt            0.0s
[5/5] COPY . .                                              0.0s
```

Why does layer order matter for build speed?
- Docker builds images in layers, reading your `Dockerfile` from top to bottom.
- Each instruction (`FROM`, `RUN`, `COPY`) creates a new layer on top of the previous one.
- When you rebuild an image, Docker checks each instruction if anything changed after last build. 
  - If **no**, Docker reuses the cached layer (fast).
  - If **yes**, Docker runs the instruction to build a new layer and **instantly invalidates the cache for ALL subsequent layers**.
