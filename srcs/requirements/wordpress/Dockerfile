FROM debian:bookworm

# System update
RUN apt-get update && apt-get upgrade -y

# Install required packages
RUN apt-get install -y \
    wget \
    curl \
    netcat-traditional \
    php \
    php-mysql \
    php-fpm \
    php-cli \
    mariadb-client \
    && rm -rf /var/lib/apt/lists/*

# Create required directories
RUN mkdir -p /run/php /var/www/html

# Adjust permissions
RUN chown -R www-data:www-data /run/php

# Download and install WordPress
RUN wget https://wordpress.org/latest.tar.gz -P /var/www/ \
    && tar -xzf /var/www/latest.tar.gz -C /var/www/html --strip-components=1 \
    && rm -f /var/www/latest.tar.gz \
    && chown -R www-data:www-data /var/www/html

# Install WP-CLI
RUN wget https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp

# Configure PHP-FPM to listen on port 9000
RUN sed -i 's|listen = /run/php/php8.2-fpm.sock|listen = 9000|' /etc/php/8.2/fpm/pool.d/www.conf

# Redirect logs to stdout
RUN ln -sf /dev/stdout /var/log/php8.2-fpm.log

# Copy configuration script
COPY tools/config.sh /tools/config.sh
RUN chmod +x /tools/config.sh

WORKDIR /var/www/html

EXPOSE 9000

ENTRYPOINT ["sh", "/tools/config.sh"]

CMD ["/usr/sbin/php-fpm8.2", "-F"]
