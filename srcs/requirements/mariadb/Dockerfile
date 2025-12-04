FROM debian:bookworm

# Install required packages
RUN apt-get update && apt-get install -y \
    mariadb-server \
    mariadb-client \
    bash \
    && rm -rf /var/lib/apt/lists/*

# Create required directories
RUN mkdir -p /run/mysqld /var/lib/mysql \
    && chown -R mysql:mysql /run/mysqld /var/lib/mysql

# Copy configuration files and initialization script
COPY tools/config.sh /config.sh
RUN chmod +x /config.sh

# Expose MariaDB port
EXPOSE 3306

ENTRYPOINT ["/config.sh"]
