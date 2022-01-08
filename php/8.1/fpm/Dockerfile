# Base image
FROM php:8.1-fpm

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
    docker-php-ext-install -j$(nproc) ctype curl gd iconv intl pdo_mysql mysqli session sysvsem xml zip

# Installs PECL Extensions
RUN pecl install ds && \
    pecl install xdebug-3.1.2 && \
    docker-php-ext-enable xdebug

# Installs Composer
RUN wget https://raw.githubusercontent.com/composer/getcomposer.org/d4525508b60af43a52f274d70315bfed4d671fd3/web/installer -O - -q | php -- --install-dir=/usr/local/bin --filename=composer

# Create folders and fix permissions
RUN mkdir -p /var/www/.composer && \
    chown -R www-data:www-data /var/www/.composer