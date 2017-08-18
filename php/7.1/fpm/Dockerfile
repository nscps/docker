# base image
FROM php:7.1-fpm

# maintainer
LABEL maintainer="nilosoares@gmail.com"

# environment variables
ENV DEBIAN_FRONTEND=noninteractive

# users permissions
RUN usermod -u 1000 www-data

# updates
RUN apt-get update && apt-get install -y \
    apt-utils \
    git \
    unzip \
    vim \
    zip \

    # dependencies for php-gd
    libfreetype6-dev \
    libjpeg62-turbo-dev \

    # dependency for php-mcrypt
    libmcrypt-dev \

    # dependencies for php-zip and intl
    zlib1g-dev \
    libicu-dev

# Installs PHP Extensions
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-configure intl && \
    docker-php-ext-install -j$(nproc) gd mcrypt pdo_mysql zip

# Installs composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer