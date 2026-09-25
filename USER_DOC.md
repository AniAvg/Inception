# User Documentation

## Accessing the Website

The WordPress website is available at:

```text
https://anavagya.42.fr
```

HTTPS is provided by the NGINX container using TLS.

## Starting the Project

From the project root:

```bash
make
```

To build the images and start all services:

```bash
make build
```

## Stopping the Project

```bash
make down
```

## Checking the Services

```bash
docker ps
```

The following containers should be running:

```text
nginx
wordpress
mariadb
```

## WordPress

After opening the website, WordPress can be configured through its installation page.

The WordPress administration panel is available at:

```text
https://anavagya.42.fr/wp-admin
```

## Troubleshooting

If the website is not accessible:

```bash
docker ps
docker logs nginx
docker logs wordpress
docker logs mariadb
```

To check the NGINX configuration:

```bash
docker exec nginx nginx -t
```

