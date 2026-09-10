# Day 32 – Docker Volumes & Networking

## Task 1: The Problem

Run a Postgres or MySQL container

```bash
docker run -d --name pg-test -e POSTGRES_PASSWORD=postgres postgres:alpine
```

Create some data inside it (a table, a few rows — anything)

```bash
docker exec -it pg-test psql -U postgres -c "CREATE TABLE users (id SERIAL PRIMARY KEY, name VARCHAR(50));"
docker exec -it pg-test psql -U postgres -c "INSERT INTO users (name) VALUES ('I am a DevOps Engineer');"
docker exec -it pg-test psql -U postgres -c "SELECT * FROM users;"
```

Stop and remove the container

```bash
docker stop pg-test && docker rm pg-test
```

Run a new one — is your data still there?

```bash
docker run -d --name pg-test -e POSTGRES_PASSWORD=postgres postgres:alpine
```

Check if data exists

```bash
docker exec -it pg-test psql -U postgres -c "SELECT * FROM users;"
```
which led to:
```
ERROR: relation "users" does not exist
LINE 1: SELECT * FROM users;
                      ^
```
**What happened?**: Deleting the container also deleted the data from the postgres database. It's called **Data Ephemerality**.   
**Why it happend?**: Container writable layers are tied to the container lifecycle. Deleting the container deletes its writable storage layer.

---

## Task 2: Named Volumes

Create a named volume

```bash
docker volume create pgdata
```

Verify volume

```bash
docker volume ls
docker volume inspect pgdata
```

Run the same database container, but this time **attach the volume** to it

```bash
docker run -d --name pg-test -v pgdata:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postgres postgres:14-alpine
```

Add some data, stop and remove the container

```bash
docker exec -it pg-test psql -U postgres -c "CREATE TABLE users (name VARCHAR(50));"
docker exec -it pg-test psql -U postgres -c "INSERT INTO users (name) VALUES ('I am a Senior DevOps Engineer!');"
docker stop pg-test && docker rm pg-test
```

Run a brand new container with the **same volume**

```bash
docker run -d --name pg-test-new -v pgdata:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postgres postgres:14-alpine
```

Is the data still there?

```bash
docker exec -it pg-test-new psql -U postgres -c "SELECT * FROM users;"
```
**Yes**. Docker containers are **ephemeral** in state. Volumes are created in the host machine's system, not **inside** the container. Hence, **data persistence** happens.

---

## Task 3: Bind Mounts

Create a folder on your host machine with an `index.html` file

```bash
mkdir nginx-test && cd nginx-test
```
`index.html`:
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Welcome to Our Website</title>
</head>

<body>
    <h1>This is the Nginx Welcome Page</h1>
    <p>This is the short description for the Nginx welcome page.</p>
</body>

</html>
```

Run an Nginx container and **bind mount** your folder to the Nginx web directory

```bash
docker run -d --name nginx-test -p 8080:80 -v $(pwd):/usr/share/nginx/html:ro nginx:alpine
```

Access the page in your browser

```
http://localhost:8080
```

Edit the `index.html` on your host — refresh the browser

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Welcome to Our Website</title>
</head>

<body>
    <h1>This is the Nginx Welcome Page</h1>
    <p>This is the short description for the Nginx welcome page.</p>
    <p>This is the next line I have added after updation.</p> <!-- This is the new line added. -->
</body>

</html>
```

**What is the difference between a named volume and a bind mount?**

| Named Volume| Bind Mount |
| :--- | :--- |
| Managed entirely by Docker (`/var/lib/docker/volumes/` on Linux). | Direct mapping to any arbitrary host path. |
| Isolated from host filesystem modifications. | Depends on the host filesystem structure. |
| Ideal for databases and production data. | Ideal for local development and source code hot-reloading. |

---

## Task 4: Docker Networking Basics

List all Docker networks on your machine

```bash
docker network ls
```

Inspect the default `bridge` network

```bash
docker network inspect bridge
```

Run two containers on the default bridge

```bash
docker run -d --name John alpine sleep 3600
docker run -d --name Alex alpine sleep 3600
```

Can they ping each other by **name**?

```bash
docker exec John ping Alex
```
which gives:
```
ping: bad address 'Alex'
```
**Fails**: default `bridge` does NOT support automatic service discovery.

Can they ping each other by **IP**?

```bash
# Find Alex's IP and ping by IP
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' Alex
docker exec John ping <ALEX_IP_ADDRESS> # Succeeds
```

---

## Task 5: Custom Networks

Create a custom bridge network called `my-app-net`

```bash
docker network create my-app-net
```

Run two containers on `my-app-net`

```bash
docker run -d --name John --network my-app-net alpine sleep 3600
docker run -d --name Alex --network my-app-net alpine sleep 3600
```

Can they ping each other by **name** now?

```bash
docker exec John ping Alex
```

**Why does custom networking allow name-based communication but the default bridge doesn't?**
- **User-defined** networks automatically enable Docker's internal DNS resolver (listening on `127.0.0.11`), allowing container names to resolve directly to IP addresses.
- The default **bridge** disables this to maintain backward compatibility.

---

## Task 6: Put It Together

Create a custom network and volume

```bash
docker network create app-network
docker volume create db-data
```

Run a **database container** (MySQL/Postgres) on that network with a volume for data

```bash
docker run -d \
  --name mydb \
  --network app-network \
  -v db-data:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=postgres \
  postgres:16-alpine
```

Run an **app container** (use any image) on the same network

```bash
docker run -d --name web-app --network app-network nginx:alpine
```

Verify the app container can reach the database by container name

```bash
docker exec web-app ping mydb
```
