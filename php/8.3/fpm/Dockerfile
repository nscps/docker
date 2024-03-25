# Base image
FROM php:8.3.4-fpm

# Maintainer
LABEL maintainer="nilosoares@gmail.com"

# Update user permission
RUN usermod -u 1000 www-data

# Update/Install dependencies
RUN apt update && apt install -y \
    apt-utils \
    build-essential \
    git \
    wget \
    unzip \
    zip \
    libcurl4-openssl-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libicu-dev \
    libxml2-dev \
    libzip-dev

# Installs PHP Extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-configure intl && \
    docker-php-ext-configure opcache --enable-opcache && \
    docker-php-ext-install -j$(nproc) ctype curl gd iconv intl pdo_mysql mysqli opcache session sysvsem xml zip

# Installs PECL Extensions
RUN pecl install ds && \
    pecl install redis && \
    pecl install xdebug-3.3.1 && \
    docker-php-ext-enable ds redis xdebug

# Installs Composer
RUN wget https://raw.githubusercontent.com/composer/getcomposer.org/993f9fec74930f32f7015e71543243bf6d9b9e93/web/installer -O - -q | php -- --install-dir=/usr/local/bin --filename=composer

# Create folders and fix permissions
RUN mkdir -p /var/www/.composer && \
    chown -R www-data:www-data /var/www/.composer
