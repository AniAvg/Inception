# User Documentation

## Accessing the Website

The WordPress website is available at:

```text
https://anavagya.42.fr
```

The website is served through NGINX using HTTPS and TLS.

HTTP on port 80 is not used by the project.

## Starting the Project

From the project root:

```bash
make
```

To build the Docker images and start all services:

```bash
make build
```

After starting the project, check the containers:

```bash
docker ps
```

The following containers should be running:

```text
nginx
wordpress
mariadb
```

## Stopping the Project

To stop the infrastructure:

```bash
make down
```

## WordPress Administration

The WordPress administration panel is available at:

```text
https://anavagya.42.fr/wp-admin
```

Use the WordPress administrator credentials created for this project.

The WordPress administrator account should use a username that does not contain `admin` or `Admin`.

WordPress is configured as part of the project and should not require the initial WordPress installation page during normal use.

## Credentials

Database configuration is stored in the local:

```text
srcs/.env
```

The file contains configuration such as:

```text
DB_NAME
DB_USER
DB_PASS
DB_ROOT
```

Real passwords must remain private and must not be committed to the Git repository.

WordPress administrator credentials are separate from the MariaDB credentials and should also be kept private.

## Basic Checks

Check running containers:

```bash
docker ps
```

Check the NGINX configuration:

```bash
docker exec nginx nginx -t
```

Check the container logs:

```bash
docker logs nginx
docker logs wordpress
docker logs mariadb
```

Test HTTPS:

```bash
curl -k -I https://anavagya.42.fr
```

## Troubleshooting

If the website is not accessible, first check that all three containers are running:

```bash
docker ps
```

Then check their logs:

```bash
docker logs nginx
docker logs wordpress
docker logs mariadb
```

If NGINX is not working, test its configuration:

```bash
docker exec nginx nginx -t
```

If the project needs to be rebuilt after a configuration change:

```bash
make re
```

