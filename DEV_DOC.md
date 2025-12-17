# Developer Documentation

This document explains **how the infrastructure is built and configured**, and is intended for developers and evaluators.

---

## Containers Overview

### Nginx

* Base image: Debian Bullseye
* Acts as reverse proxy
* Terminates TLS (HTTPS)
* Exposes port 443 only

### WordPress

* PHP-FPM only (no nginx)
* Connects to MariaDB
* Receives requests via FastCGI

### MariaDB

* Persistent database
* Credentials loaded via Docker secrets

---

## SSL Configuration

* SSL is **auto-signed** using OpenSSL
* Generated **inside the Nginx container**
* TLS versions: 1.2 and 1.3
* Certificate CN: `jbergfel.42.fr`

This ensures:

* No dependency on external services
* Full reproducibility

---

## Docker Network

A custom Docker bridge network is used:

```
srcs_inception
```

All containers communicate internally through this network.

---

## Volumes

### WordPress Data

```
/home/jbergfel/data/wordpress
```

### MariaDB Data

```
/home/jbergfel/data/mariadb
```

These volumes ensure data persistence across container restarts.

---

## Environment Variables & Secrets

Sensitive data is stored using Docker secrets:

* Database password
* Root password
* WordPress admin password
* Secondary user password

Secrets are mounted under:

```
/run/secrets/
```

No credentials are hardcoded in Dockerfiles.

---

## Build & Deployment Flow

1. `docker compose build`
2. Containers generate internal configuration
3. SSL certificates are created at build time
4. Services start in dependency order

---

## Evaluation Compliance

This project respects all mandatory requirements of the Inception subject:

* Dedicated Dockerfiles per service
* TLS enabled on Nginx
* No use of `latest` tags
* No external services
* Persistent volumes
* Custom Docker network

---

## Maintenance

To rebuild everything from scratch:

```bash
docker compose down -v
docker compose build --no-cache
docker compose up -d
```
