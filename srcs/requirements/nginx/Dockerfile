FROM debian:bookworm

# Install required packages
RUN apt-get update && apt-get install -y \
    nginx \
    openssl \
    bash \
    && rm -rf /var/lib/apt/lists/*

# Create required directories
RUN mkdir -p /etc/nginx/ssl /run/nginx

# Copy Nginx configuration
COPY conf/default.conf /etc/nginx/nginx.conf

# Copy TLS initialization script
COPY tools/config.sh /config.sh
RUN chmod +x /config.sh

EXPOSE 443

ENTRYPOINT ["/config.sh"]
