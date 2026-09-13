# Day 34 – Docker Compose: Real-World Multi-Container Apps

## Task 1: Build Your Own App Stack

A simple **Python** app which stores user's data in **MySQL** database with **Redis** caching.

### Tech Stack
- Python 3.13
- MySQL 8.0
- Redis

`Dockerfile`:
```dockerfile
FROM python:3.11-alpine

WORKDIR /app

RUN --mount=type=cache,target=/root/.cache/pip \
    --mount=type=bind,source=requirements.txt,target=requirements.txt \
    pip install -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "main.py"]
```

`docker-compose.yml`:
```yml
services:
  web:
    build: . # Custom Dockerfile imported in Compose
    container_name: python-app
    restart: always
    ports:
      - "8000:8000"
    env_file:
      - .env
    depends_on: # The app starts AFTER the database
      mysql:
        condition: service_healthy # App waits for the database to be truly ready, not just started
      redis:
        condition: service_healthy
    networks:
      - python-app-network

  mysql:
    image: mysql:8.0
    container_name: mysql
    restart: always
    environment:
      MYSQL_HOST: ${MYSQL_HOST}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
    volumes:
      - python-mysql-data:/var/lib/mysql
      - ./schema.sql:/docker-entrypoint-initdb.d/schema.sql:ro
    healthcheck:
      test: ["CMD-SHELL", "mysqladmin ping -h localhost -u $$MYSQL_USER -p$$MYSQL_ROOT_PASSWORD"] # Healthcheck added
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 20s
    networks:
      - python-app-network

  redis:
    image: redis:alpine
    container_name: redis
    restart: on-failure
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 3
    networks:
      - python-app-network

networks:
  python-app-network:

volumes:
  python-mysql-data:
```

---

## Task 2: `depends_on` & Healthchecks

Add `depends_on` to your compose file so the app starts **after** the database
```yml
services:
  web:
  # Other code
  depends_on: # The app starts AFTER the database
      mysql:
```

Add a **healthcheck** on the database service
```yml
services:
  mysql:
    healthcheck:
      test: ["CMD-SHELL", "mysqladmin ping -h localhost -u $$MYSQL_USER -p$$MYSQL_ROOT_PASSWORD"] # Healthcheck added
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 20s
```
- `start_period` is important. MySQL takes time to initialize on first boot.
- Without it, the healthcheck fires before MySQL is ready and marks it unhealthy immediately.

Use `depends_on` with `condition: service_healthy` so the app waits for the database to be truly ready, not just started
```yml
services:
  web:
  # Other code
  depends_on: # The app starts AFTER the database
      mysql:
        condition: service_healthy # App waits for the database to be truly ready, not just started
      redis:
        condition: service_healthy
```
- The `web` container will not start until both `mysql` and `redis` pass their healthchecks.
- `depends_on` without `condition` only waits for the container to start, not for the service inside to be ready.

Bring everything down and up — does the app wait for the DB? *Yes*.

---

## Task 3: Restart Policies

Add `restart: always` to your database service
```yml
services:
  mysql:
    image: mysql:8.0
    container_name: mysql
    restart: always
```

Manually kill the database container — does it come back?
```bash
docker kill <service-name>
# OR
sudo kill -9 <PID>
```

`docker kill` vs `sudo kill -9`

| Aspect | `docker kill` | `sudo kill -9` |
| :--- | :--- | :--- |
| Who sends the signal | Docker daemon | Linux kernel (directly) |
| Path | Docker API → daemon → container process | `kill` syscall → PID |
| `hasBeenManuallyStopped` | Set to `true` | Never touched, stays `false` |
| Restart policy triggered | No | Yes |
| Docker's awareness | Before the process dies | After the process dies |

Why `OOMKilled=false` despite exit code **137**?   
- Exit code **137 = 128 + 9**. It means the process received SIGKILL.

| Exit Code | Cause | `OOMKilled` flag |
| :--- | :--- | :--- |
| 137 | **SIGKILL** sent by operator (`docker kill` or `sudo kill -9`) | `false` |
| 137 | **SIGKILL** sent by Linux OOM killer (container ran out of memory) | `true` |

Try `restart: on-failure` — how is it different?
```yml
services:
  redis:
    image: redis:alpine
    container_name: redis
    restart: on-failure
```

When would you use each restart policy?

| Aspect | `always` | `on-failure` | `unless-stopped` | `no` |
| :--- | :--- | :--- | :--- | :--- |
| Restarts on crash (non-zero exit) | Yes | Yes | Yes | No |
| Restarts on clean exit (exit 0) | Yes | No | Yes | No |
| Restarts after `docker kill` / `docker stop` | No | No | No | No |
| Restarts after Docker daemon restart | Yes | Yes (if was running) | No (if was manually stopped) | No |
| Restarts after host reboot | Yes | Yes | Only if not manually stopped before reboot | No |
| Use case | Critical stateful services (DB, message broker) | App services — crash protection without hiding config errors | Like `always` but respects manual stops | Dev environments, one-off jobs, init containers |

---

## Task 4: Custom Dockerfiles in Compose

Instead of using a pre-built image for your app, use `build:` in your compose file to build from a Dockerfile
```yml
services:
  web:
    build: . # Custom Dockerfile imported in Compose
    container_name: python-app
    restart: always
```

Make a code change in your app
```html
<h2>Existing Users</h2> <!-- Old code -->
<h2>User Management Dashboard</h2> <!-- Replaced with -->
```

Rebuild and restart with one command
```bash
docker compose up --build
```
Compose rebuilds the image and restarts only the `web` container.

---

## Task 5: Named Networks & Volumes

Define **explicit networks** in your compose file instead of relying on the default
```yml
networks:
  python-app-network:
```
- Services on the same network talk to each other by service name (e.g., `mysql`, `redis`).
- Without an explicit network, Compose creates a default one, but explicit networks let you:
  - Isolate services (e.g., `mysql` not reachable from outside the network)
  - Use multiple networks in the same compose file (e.g., a separate admin network)

Define **named volumes** for database data
```yml
volumes:
  python-mysql-data:
```
- Data persists across `docker compose down` and `docker compose up`. Without named volumes, MySQL data is lost every time the container is removed.   
- To fully wipe: `docker compose down -v` **-v** removes named volumes too.

Add **labels** to your services for better organization
```yml
services:
  mysql:
    labels:
      project: "python-app"
      role: "database"
```
Labels don't change runtime behavior. They're metadata usually used for filtering , `docker ps --filter label=project=python-app`; monitoring tools and cleanup scripts.

---

## Task 6: Scaling (Bonus)

Try scaling your web app to 3 replicas using `docker compose up --scale`
```bash
docker compose up --scale web=3
```

What happens?  
- Compose tries to start 3 `web` containers.

What breaks?   
- It fails on the port mapping.
```yml
ports:
  - "8000:8000"
```

Why doesn't simple scaling work with port mapping?     
- Three containers cannot all bind to port `8000` on the host.
- Only one process can own a host port at a time.
- The second and third containers crash with a bind error.

How to fix it?    
Use `expose` instead of `ports`, and put a load balancer (Nginx or Traefik) in front:
```yml
web:
  expose:
    - "5000"         # visible inside Docker network only
  # no ports: mapping
```
- Then scale freely and let the load balancer distribute traffic.
- This is why simple `--scale` doesn't work with port mapping.
- It's not a Compose limitation, it's how TCP ports work.
