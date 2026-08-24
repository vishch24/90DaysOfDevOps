# Docker Images & Container Lifecycle

## Docker Images

1. Pull the `nginx`, `ubuntu`, and `alpine` images from Docker Hub
```bash
docker pull nginx && docker pull ubuntu && docker pull alpine
```

2. List all images on your machine — note the sizes
```bash
docker images
```

3. Compare `ubuntu` vs `alpine` — why is one much smaller?
   - `alpine` is much smaller because, it uses a minimal `musl` C library, a tiny `BusyBox` toolset, and strips out extra packages to stay ultra-lightweight.
   - `ubuntu` includes a full set of GNU core utilities, management tools, and a broad software library.

4. Inspect an image — what information can you see?
```bash
docker inspect <image-id>
```
It shows details like build history, environment variables, exposed network ports, working directories, default commands, and image layers.

5. Remove an image you no longer need
```bash
docker rmi <image-id>
```
Use `-f` to forcefully remove an image.

---

## Image Layers

1. Run `docker image history nginx` — what do you see?
   - `IMAGE`: The cryptographic hash (Layer ID) of each layer. Official pulled images often show `<missing>` for older layers because they were built on a remote BuildKit server.
   - `CREATED`: How long ago that specific layer was created or updated by the maintainer.
   - `CREATED BY`: The exact Dockerfile instruction (like `RUN`, `COPY`, `ENV`, `CMD`) used to compile that layer.
   - `SIZE`: The precise amount of disk space that the layer occupies.COMMENT: Usually empty or containing internal BuildKit build labels.

2. Each line is a **layer**. Note how some layers show sizes and some show 0B
```text
IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
0d4374c710a9   4 days ago    CMD ["nginx" "-g" "daemon off;"]                0B        buildkit.dockerfile.v0
<missing>      4 days ago    STOPSIGNAL SIGQUIT                              0B        buildkit.dockerfile.v0
<missing>      4 days ago    EXPOSE map[80/tcp:{}]                           0B        buildkit.dockerfile.v0
<missing>      4 days ago    ENTRYPOINT ["/docker-entrypoint.sh"]            0B        buildkit.dockerfile.v0
<missing>      4 days ago    COPY 30-tune-worker-processes.sh /docker-ent…   16.4kB    buildkit.dockerfile.v0
<missing>      4 days ago    COPY 20-envsubst-on-templates.sh /docker-ent…   12.3kB    buildkit.dockerfile.v0
<missing>      4 days ago    COPY 15-local-resolvers.envsh /docker-entryp…   12.3kB    buildkit.dockerfile.v0
<missing>      4 days ago    COPY 10-listen-on-ipv6-by-default.sh /docker…   12.3kB    buildkit.dockerfile.v0
<missing>      4 days ago    COPY docker-entrypoint.sh / # buildkit          8.19kB    buildkit.dockerfile.v0
<missing>      4 days ago    RUN /bin/sh -c set -x     && groupadd --syst…   87.6MB    buildkit.dockerfile.v0
<missing>      4 days ago    ENV DYNPKG_RELEASE=1~trixie                     0B        buildkit.dockerfile.v0
<missing>      4 days ago    ENV PKG_RELEASE=1~trixie                        0B        buildkit.dockerfile.v0
<missing>      4 days ago    ENV ACME_VERSION=0.4.1                          0B        buildkit.dockerfile.v0
<missing>      4 days ago    ENV NJS_RELEASE=1~trixie                        0B        buildkit.dockerfile.v0
<missing>      4 days ago    ENV NJS_VERSION=1.0.0                           0B        buildkit.dockerfile.v0
<missing>      4 days ago    ENV NGINX_VERSION=1.31.4                        0B        buildkit.dockerfile.v0
<missing>      4 days ago    LABEL maintainer=NGINX Docker Maintainers <d…   0B        buildkit.dockerfile.v0
<missing>      3 weeks ago   # debian.sh --arch 'amd64' out/ 'trixie' '@1…   87.4MB    debuerreotype 0.17
```

3. What are layers and why does Docker use them?
   - Docker layers are individual, read-only file system changes stacked on top of each other to form a complete container image.
   - Each instruction in a Dockerfile (like `RUN` or `COPY`) creates a new layer.
   - Docker uses them to save storage space, speed up builds through caching, and enable efficient sharing of base files.

---

## Container Lifecycle

1. Create a container (without starting it)
```bash
docker create --name my_app nginx
```
STATUS: `Created`

2. **Start** the container
```bash
docker start my_app
```
STATUS: `Up 2 seconds`

3. **Pause** it and check status
```bash
docker pause my_app
```
STATUS: `Up 15 seconds (paused)`

4. **Unpause** it
```bash
docker unpause my_app
```
STATUS: `Up 25 seconds`

5. **Stop** it
```bash
docker stop my_app
```
STATUS: `Exited (0) 2 seconds ago`

6. **Restart** it
```bash
docker restart my_app
```
STATUS: `Up 2 seconds`

7. **Kill** it
```bash
docker kill my_app
```
STATUS: `Exited (137) 2 seconds ago`

8. **Remove** it
```bash
docker rm my_app
```

---

## Working with Running Containers

1. Run an Nginx container in detached mode
```bash
docker run -d --name my_nginx -p 8080:80 nginx
```

2. View its **logs**
```bash
docker logs my_nginx
```

3. View **real-time logs** (follow mode)
```bash
docker logs -f my_nginx
```

4. **Exec** into the container and look around the filesystem
```bash
docker exec -it my_nginx bash
```

5. Run a single command inside the container without entering it
```bash
docker exec my_nginx ls -l /usr/share/nginx/html
```

6. **Inspect** the container — find its IP address, port mappings, and mounts
```bash
docker inspect my_nginx
# OR
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' my_nginx # Find the IP Address
docker inspect -f '{{json .NetworkSettings.Ports}}' my_nginx # Find Port Mappings
docker inspect -f '{{json .Mounts}}' my_nginx # Find Mounts (Volumes)
```

---

## Cleanup

1. Stop all running containers in one command
```bash
docker stop $(docker ps -q)
```

2. Remove all stopped containers in one command
```bash
docker container prune
```

3. Remove unused images
```bash
docker image prune # To remove only "dangling" images (images with no name/tag).
# OR
docker image prune -a # To remove all images not currently used by a container.
```

4. Check how much disk space Docker is using
```bash
docker system df
```

5. Remove stopped containers, unused networks, dangling images, and build cache
```bash
docker system prune
```
Use `-a` flag to wipe all unused images, and `--volumes` flag to delete unused volumes.
