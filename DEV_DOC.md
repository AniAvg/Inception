# Developer Documentation

## Prerequisites

The project requires:

* A Linux Virtual Machine
* Docker
* Docker Compose
* Make

The project is designed to run inside the Virtual Machine required by the 42 Inception subject.

## Project Structure

The project uses Docker Compose with three services:

```text
NGINX → WordPress/PHP-FPM → MariaDB
```

The repository contains:

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

Ready-made application images are not used for the three project services.

## Environment Variables

Project configuration is stored in:

```text
srcs/.env
```

The environment file contains values such as:

```env
DOMAIN_NAME=anavagya.42.fr
DB_NAME=wordpress
DB_USER=wpuser
DB_PASS=your_database_password
DB_ROOT=your_root_password
```

Real passwords must remain private and must not be committed to the Git repository.

The Compose file uses these variables when building and starting the services.

## Building and Starting

From the project root:

```bash
make build
```

This builds the Docker images and starts the complete infrastructure with Docker Compose.

To start the infrastructure without rebuilding:

```bash
make
```

## Stopping

To stop the infrastructure:

```bash
make down
```

## Rebuilding

After changing a Dockerfile or configuration:

```bash
make re
```

This stops the current infrastructure and rebuilds the services.

## Docker Compose Commands

The Compose file is located at:

```text
srcs/docker-compose.yml
```

The following command starts the infrastructure directly:

```bash
docker compose -f srcs/docker-compose.yml --env-file srcs/.env up -d
```

To build and start the services:

```bash
docker compose -f srcs/docker-compose.yml --env-file srcs/.env up -d --build
```

To stop the services:

```bash
docker compose -f srcs/docker-compose.yml --env-file srcs/.env down
```

To view the status:

```bash
docker compose -f srcs/docker-compose.yml --env-file srcs/.env ps
```

## Useful Commands

Check running containers:

```bash
docker ps
```

View logs:

```bash
docker logs nginx
docker logs wordpress
docker logs mariadb
```

Enter a container:

```bash
docker exec -it <container> sh
```

Check NGINX configuration:

```bash
docker exec nginx nginx -t
```

Check the WordPress database connection:

```bash
docker exec wordpress php83 -r '$mysqli = new mysqli("mariadb", getenv("DB_USER"), getenv("DB_PASS"), getenv("DB_NAME")); echo $mysqli->connect_error ?: "DB connection OK\n";'
```

## Persistent Data

The project uses two Docker named volumes:

```text
db-volume → MariaDB data
wp-volume → WordPress data
```

The volumes use host directories under:

```text
/home/anavagya/data/
```

The MariaDB data is stored under:

```text
/home/anavagya/data/mariadb/
```

The WordPress data is stored under:

```text
/home/anavagya/data/wordpress/
```

This allows the application data to persist independently from the containers.

After recreating the containers, the persistent data remains in these directories.

## Network

The containers communicate through the Docker network:

```text
srcs_inception
```

NGINX is the public HTTPS entry point.

WordPress/PHP-FPM and MariaDB communicate through the Docker network using their service names.

The project does not use the host network.

## Service Responsibilities

### NGINX

NGINX is the HTTPS entry point and handles TLS connections on port 443.

### WordPress

The WordPress container contains WordPress and PHP-FPM. PHP-FPM processes PHP requests forwarded by NGINX.

### MariaDB

MariaDB stores the WordPress database and its persistent data is stored in the MariaDB volume.

