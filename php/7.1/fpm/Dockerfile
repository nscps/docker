# Base image
FROM php:7.1-fpm

# Maintainer
LABEL maintainer="nilosoares@gmail.com"

# Environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Update user permission
RUN usermod -u 1000 www-data

# Installs dependencies
RUN apt-get update && apt-get install -y \
    apt-utils \
    git \
    unzip \
    vim \
    zip \

    # Dependencies for php-gd
    libfreetype6-dev \
    libjpeg62-turbo-dev \

    # Dependency for php-mcrypt
    libmcrypt-dev \

    # Dependencies for php-zip and intl
    zlib1g-dev \
    libicu-dev

# Installs PHP Extensions
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-configure intl && \
    docker-php-ext-install -j$(nproc) gd mcrypt pdo_mysql mysqli zip

# Installs Xdebug
RUN pecl install xdebug && docker-php-ext-enable xdebug

# Installs Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Installs Sami
RUN curl -O http://get.sensiolabs.org/sami.phar && \
    mv sami.phar /usr/local/bin/sami && \
    chmod 755 /usr/local/bin/sami