# Day 33 – Docker Compose: Multi-Container Basics

## Task 1: Install & Verify

Download Docker compose from the official [site](https://docs.docker.com/compose/install/linux/) and verify the version.
```bash
docker compose version # Should show "Docker Compose version v5.5.1"
```

## Task 2: Your First Compose File

Create a folder `compose-basics`
```bash
mkdir compose-basics && cd compose-basics
```

Write a `docker-compose.yml` that runs a single **Nginx** container with port mapping
```yml
services:
  nginx:
    image: nginx:alpine
    container_name: nginx
    ports:
      - "8080:80"
```

Start it with `docker compose up`. Access it in your browser with `http://localhost:8080`. Stop it with `docker compose down`.

## Task 3: Two-Container Setup

Write a docker-compose.yml that runs:

- A **WordPress** container
- A **MySQL** container

They should:

- Be on the same network (Compose does this automatically)
- MySQL should have a named volume for data persistence
- WordPress should connect to MySQL using the service name
```yml
services:
  wordpress:
    image: wordpress:latest
    container_name: wordpress
    ports:
      - "8080:80"
    networks:
      - wordpress-network
    depends_on:
      - mysql

  mysql:
    image: mysql:8.0
    container_name: mysql
    networks:
      - wordpress-network
    volumes:
      - mysql-db-data:/var/lib/mysql
networks:
  wordpress-network:

volumes:
  mysql-db-data:
```
Start it, access WordPress in your browser, and set it up with some data.   
**Verify**: Stop and restart with `docker compose down` and `docker compose up` — is your WordPress data still there? *Yes*.

## Task 4: Compose Commands

| Commands | Meaning |
| :--- | :--- |
| `docker compose up -d` | `-d` flag (short for `--detach`) instructs Docker Compose to build, create, and start the containers in the background, freeing up your terminal prompt immediately. |
| `docker compose ps` | Lists all containers for the Compose project along with their status, ports, and commands. |
| `docker compose logs` | View the logs of **all services** defined in your Docker Compose file. |
| `docker compose logs <service_name>` | View the logs of **a specific service** in Docker Compose. |
| `docker compose stop` | Stop your Docker Compose services **without removing the containers, networks, or volumes**. |
| `docker compose down` | Remove everything created by a specific Docker Compose file (including **containers** and **networks**) |
| `docker compose up --build` | Rebuild your Docker images and apply changes when running your application. |

## Task 5: Environment Variables

Add environment variables directly in your `docker-compose.yml`
```yml
services:
  wordpress:
    image: wordpress:latest
    container_name: wordpress
    environment:
      WORDPRESS_DB_HOST: "mysql"
      WORDPRESS_DB_NAME: "wordpress_db"
      WORDPRESS_DB_USER: "wp_user"
      WORDPRESS_DB_PASSWORD: "wp_secure_password"
```

Create a `.env` file and reference variables from it in your compose file
```env
WORDPRESS_DB_HOST=mysql
WORDPRESS_DB_NAME=wordpress_db
WORDPRESS_DB_USER=wp_user
WORDPRESS_DB_PASSWORD=wp_secure_password
```
Reference them in `docker-compose.yml` using the `${VARIABLE_NAME}` syntax:
```yml
services:
  wordpress:
    image: wordpress:latest
    container_name: wordpress
    environment:
      WORDPRESS_DB_HOST: "{WORDPRESS_DB_HOST}"
      WORDPRESS_DB_NAME: "{WORDPRESS_DB_NAME}"
      WORDPRESS_DB_USER: "{WORDPRESS_DB_USER}"
      WORDPRESS_DB_PASSWORD: "{WORDPRESS_DB_PASSWORD}"
```

Verify the variables are being picked up
```bash
docker compose config
# OR
docker compose exec web env
```
