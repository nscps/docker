# Base image
FROM php:7.4-fpm

# Maintainer
LABEL maintainer="nilosoares@gmail.com"

## Update user permission
RUN usermod -u 1000 www-data

# Update/Install dependencies
RUN apt update && apt install -y \
    apt-utils \
    build-essential \
    git \
    wget \
    unzip \
    vim \
    zip \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libicu-dev \
    libzip-dev

## Installs PHP Extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-configure intl && \
    docker-php-ext-install -j$(nproc) gd intl pdo_mysql mysqli zip

## Installs PECL Extensions
RUN pecl install ds && \
    pecl install xdebug-3.0.1 && \
    docker-php-ext-enable ds xdebug

# Installs Composer
RUN wget https://raw.githubusercontent.com/composer/getcomposer.org/e3e43bde99447de1c13da5d1027545be81736b27/web/installer -O - -q | php -- --install-dir=/usr/local/bin --filename=composer

## Create folders and fix permissions
RUN mkdir -p /var/www/.composer && \
    chown -R www-data:www-data /var/www/.composer