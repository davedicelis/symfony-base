FROM php:8.5-apache

ARG UID

# Create user with same permissions as host and some useful stuff
RUN adduser -u ${UID} --disabled-password --gecos "" appuser \
    && mkdir /home/appuser/.ssh \
    && echo "StrictHostKeyChecking no" >> /home/appuser/.ssh/config \
    && echo "alias sf=/var/www/html/bin/console" >> /home/appuser/.bashrc \
    && chown -R appuser:appuser /home/appuser/

# Install packages and PHP extensions
RUN apt-get update \
    # common libraries and extensions
    && apt-get install -y --no-install-recommends git acl openssl openssh-client wget unzip zip \
    && apt-get install -y --no-install-recommends libpng-dev zlib1g-dev libzip-dev libxml2-dev libicu-dev \
    && docker-php-ext-install intl zip \
    # for MySQL
    && docker-php-ext-install pdo_mysql \
    # XDEBUG and APCu (OPcache is built into PHP since 8.5)
    && pecl install xdebug apcu \
    && docker-php-ext-enable xdebug apcu \
    && rm -rf /var/lib/apt/lists/* /tmp/pear

# Install Composer (latest 2.x) from the official image
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Install Symfony CLI
RUN curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | bash \
    && apt-get install -y --no-install-recommends symfony-cli \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /var/www/html

# Config XDEBUG
COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# Update Apache config
COPY ./default.conf /etc/apache2/sites-available/default.conf
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf \
    && a2enmod rewrite \
    && a2dissite 000-default \
    && a2ensite default

# Use the development php.ini
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

WORKDIR /var/www/html
