# Inception

This project is part of the **42 Inception** curriculum. The goal is to set up a small infrastructure using **Docker** and **Docker Compose**, following strict rules regarding security, isolation, and reproducibility.

The infrastructure includes:

* **Nginx** with TLS (HTTPS)
* **WordPress** with **PHP-FPM**
* **MariaDB** database
* Persistent volumes
* A dedicated Docker network

All services are containerized and orchestrated via `docker-compose`.

---

## Architecture Overview

```
Client (Browser)
   ↓ HTTPS (443)
Nginx (TLS)
   ↓ FastCGI (9000)
WordPress (PHP-FPM)
   ↓ TCP (3306)
MariaDB
```

---

## Requirements

* Docker
* Docker Compose
* Linux (Debian-based recommended)

---

## Project Structure

```
.
├── Makefile
├── secrets/
├── srcs/
│   ├── docker-compose.yml
│   └── requirements/
│       ├── nginx/
│       ├── wordpress/
│       └── mariadb/
```

---

## How to Run

```bash
make
```

Stop the infrastructure:

```bash
make down
```

---

## Access

Add this line to `/etc/hosts`:

```
127.0.0.1 jbergfel.42.fr
```

Then open:

```
https://jbergfel.42.fr
```

> ⚠️ The SSL certificate is self-signed. Browser warnings are expected.

---

## Security Notes

* HTTPS only (TLS 1.2 / 1.3)
* No plaintext credentials in Dockerfiles
* Secrets managed via Docker secrets
* No external services (e.g., Let's Encrypt)

---
