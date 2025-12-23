# User Documentation

This document explains **how to use** the Inception infrastructure once it is set up.

---

## Starting the Project

From the project root:

```bash
make
```

This command:

* Builds all Docker images
* Creates volumes and network
* Starts all services

---

## Stopping the Project

```bash
make down
```

To fully reset data (⚠️ deletes volumes):

```bash
make fclean
```

---

## Website Access

URL:

```
https://jbergfel.42.fr
```

Admin panel:

```
https://jbergfel.42.fr/wp-admin
```

---

## WordPress Users

### Administrator

* Username: `super_jbergfel`
* Role: Administrator

### Secondary User

* Username: `jbergfel`
* Role: Author

Passwords are stored securely using Docker secrets.

---

## Expected Behavior

* HTTPS is always enabled
* HTTP is not exposed
* WordPress content persists after restart
* Database data persists after restart

---

## Common Issues

### Browser SSL Warning

This is normal due to the self-signed certificate.

### Containers Not Starting

Run:

```bash
docker ps
docker logs nginx
```

## Verifications

Make sure that NGINX is exposed in port 443:

```
docker ps
```

```
curl -vk https://login.42.fr
```

Check if SSL/TLS is actiave:

```
openssl s_client -connect login.42.fr:443
```

## Verify ist the users is set-up correctly

```
# Enter mariadb docker bash:
docker exec -it mariadb bash


# Connect to WordPress DB:
mysql -u root -p

#Select WordPress DB:
USE wordpress;


# List the users:
SELECT ID, user_login, user_email FROM wp_users;
```

