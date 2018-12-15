# Base image
FROM php:7.3-fpm

# Maintainer
LABEL maintainer="nilosoares@gmail.com"

# Environment
ENV DEBIAN_FRONTEND=noninteractive
ENV NVM_VERSION=0.33.11
ENV NODE_VERSION=11.4.0
ENV PATH /bin/versions/node/v${NODE_VERSION}/bin/:/bin/versions/node/v${NODE_VERSION}/lib/node_modules/npm/bin/:${PATH}

# Update user permission
RUN usermod -u 1000 www-data

# Update/Install dependencies
RUN apt-get update && apt-get install -y \
    apt-utils \
    build-essential \
    git \
    unzip \
    vim \
    zip \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libicu-dev \
    libzip-dev

# Installs PHP Extensions
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-configure intl && \
    docker-php-ext-install -j$(nproc) gd intl pdo_mysql mysqli zip

# Installs PECL Extensions
RUN pecl install ds && \
    pecl install xdebug-2.7.0beta1 && \
    docker-php-ext-enable ds xdebug

# Installs Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Installs Sami
RUN curl -O http://get.sensiolabs.org/sami.phar && \
    mv sami.phar /usr/local/bin/sami && \
    chmod 755 /usr/local/bin/sami

# Installs NodeJS
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v${NVM_VERSION}/install.sh | bash
RUN . ~/.nvm/nvm.sh && nvm install ${NODE_VERSION} && nvm alias default ${NODE_VERSION}

# Create folders and fix permissions
RUN mkdir -p /var/www/.composer && \
    mkdir -p /var/www/.config && \
    mkdir -p /var/www/.npm && \
    mkdir -p /var/www/.ssh && \
    chown -R www-data:www-data /var/www/.composer && \
    chown -R www-data:www-data /var/www/.config && \
    chown -R www-data:www-data /var/www/.npm && \
    chown -R www-data:www-data /var/www/.ssh
