FROM debian:bullseye

RUN apt-get update && apt-get install -y \
    nginx \
    openssl \
 && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /etc/nginx/ssl \
 && openssl genrsa -out /etc/nginx/ssl/jbergfel.key 4096 \
 && openssl req -x509 -new -nodes \
    -key /etc/nginx/ssl/jbergfel.key \
    -sha256 -days 365 \
    -out /etc/nginx/ssl/jbergfel.crt \
    -subj "/C=BR/ST=RJ/L=Niteroi/O=42RIO/OU=Inception/CN=jbergfel.42.fr"

COPY conf/default.conf /etc/nginx/sites-available/default

RUN ln -sf /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default \
 && rm -f /etc/nginx/sites-enabled/default.conf

EXPOSE 443

CMD ["nginx", "-g", "daemon off;"]

