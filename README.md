*This project has been created as part of the 42 curriculum by anavagya.*

# Inception

## Description

**Inception** is a 42 system administration project focused on building a small web infrastructure using **Docker Compose** inside a Virtual Machine.

The infrastructure consists of three separate containers:

* **NGINX** — HTTPS entry point with TLS 1.2/1.3.
* **WordPress + PHP-FPM** — website and PHP processing.
* **MariaDB** — WordPress database.

The services communicate through a dedicated Docker network. Persistent WordPress files and database data are stored in Docker named volumes.

```text
                    HTTPS :443
                         |
                       NGINX
                         |
                    PHP-FPM :9000
                         |
                     WordPress
                         |
                    MariaDB :3306
```

The website is available at:

```text
https://anavagya.42.fr
```

## Project Structure

```text
.
├── Makefile
├── README.md
├── USER_DOC.md
├── DEV_DOC.md
└── srcs/
    ├── .env
    ├── docker-compose.yml
    └── requirements/
        ├── nginx/
        ├── wordpress/
        ├── mariadb/
        └── tools/
```

Each service has its own Dockerfile and is built locally from an Alpine Linux base image.

## Main Design Choices

### Docker

Docker provides isolated environments for each service while Docker Compose manages the complete infrastructure.

Each service runs in its own container:

```text
NGINX
WordPress + PHP-FPM
MariaDB
```

Ready-made application images are not used.

### Virtual Machines vs Docker

**Virtual Machine:**

* Runs a complete operating system.
* Provides stronger system-level isolation.
* Uses more resources.

**Docker:**

* Containers share the host kernel.
* Containers are lightweight and start quickly.
* Each service can have its own isolated environment.

In this project, the VM provides the required environment and Docker isolates the application services.

### Secrets vs Environment Variables

**Environment variables** are convenient for configuration such as database names and usernames.

**Secrets** are designed for sensitive information such as passwords and credentials because they can be provided to containers without putting them directly into configuration files.

The project uses environment variables through `.env`, while sensitive credentials must remain outside the public Git repository.

### Docker Network vs Host Network

A **Docker network** provides isolated communication between containers using service names such as:

```text
mariadb
wordpress
```

A **host network** removes this network isolation by using the host's network directly.

This project uses a dedicated Docker network because the services only need to communicate with each other.

### Docker Volumes vs Bind Mounts

**Docker named volumes** are managed by Docker and are suitable for persistent application data.

**Bind mounts** directly map a specific host directory into a container.

The project uses two Docker named volumes:

```text
db-volume → MariaDB database
wp-volume → WordPress files
```

## Instructions

### Prerequisites

* Linux Virtual Machine
* Docker
* Docker Compose
* Make

### Build and Start

From the project root:

```bash
make build
```

### Start

```bash
make
```

### Stop

```bash
make down
```

### Rebuild

```bash
make re
```

### Check Services

```bash
docker ps
```

The expected containers are:

```text
nginx
wordpress
mariadb
```

### Test NGINX

```bash
docker exec nginx nginx -t
```

### Test HTTPS

```bash
curl -k -I https://anavagya.42.fr
```

## Resources

* [Docker Documentation](https://docs.docker.com/)
* [Docker Compose Documentation](https://docs.docker.com/compose/)
* [NGINX Documentation](https://nginx.org/en/docs/)
* [PHP Documentation](https://www.php.net/docs.php)
* [WordPress Developer Resources](https://developer.wordpress.org/)
* [MariaDB Documentation](https://mariadb.com/docs/)
* [Alpine Linux Documentation](https://docs.alpinelinux.org/)

### AI Usage

AI was used as a learning and debugging assistant during the project.

It helped with:

* understanding Docker concepts;
* understanding NGINX, PHP-FPM, MariaDB and Docker networking;
* debugging Dockerfiles and Compose configuration;
* checking commands and interpreting errors;
* organizing and reviewing the project documentation.

The infrastructure was built, configured and tested manually inside the Virtual Machine.

