# Developer Documentation

## Project Structure

The project uses Docker Compose with three services:

```text
NGINX → WordPress/PHP-FPM → MariaDB
```

Each service has its own Dockerfile and is built from an Alpine Linux base image.

## Building

From the project root:

```bash
make build
```

The Makefile starts Docker Compose and builds the required images.

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

## Rebuilding

After changing a Dockerfile or configuration:

```bash
make re
```

To stop the infrastructure:

```bash
make down
```

## Environment Variables

The project configuration is stored in:

```text
srcs/.env
```

Example:

```env
DOMAIN_NAME=anavagya.42.fr

DB_NAME=wordpress
DB_USER=wpuser
DB_PASS=your_database_password
DB_ROOT=your_root_password
```

The actual passwords must be kept private and should not be committed to the Git repository.

## Persistent Data

The project uses two Docker named volumes:

```text
db-volume → MariaDB data
wp-volume → WordPress data
```

Persistent data is stored under:

```text
/home/anavagya/data/
```

## Network

The containers communicate through the Docker network:

```text
srcs_inception
```

NGINX is the public HTTPS entry point, while WordPress and MariaDB communicate internally through the Docker network.

