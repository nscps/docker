# Base image
FROM php:7.0-fpm

# Maintainer
LABEL maintainer="nilosoares@gmail.com"

# Environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV NVM_VERSION=0.33.2
ENV NODE_VERSION=8.4.0
ENV PATH /bin/versions/node/v${NODE_VERSION}/bin/:/bin/versions/node/v${NODE_VERSION}/lib/node_modules/npm/bin/:${PATH}

# Update user permission
RUN usermod -u 1000 www-data

RUN apt-get update && apt-get install -y \
    apt-utils \
    build-essential \
    git \
    g++ \
    make \
    python-software-properties \
    python \
    unzip \
    vim \
    zip \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
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

# Installs NodeJS
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v${NVM_VERSION}/install.sh | bash
RUN . ~/.nvm/nvm.sh && nvm install ${NODE_VERSION} && nvm alias default ${NODE_VERSION}

# Installs Gulp, Python, and Apidoc
RUN npm install -g gulp node-gyp apidoc

# Create folders and fixs permissions
RUN mkdir -p /var/www/.composer && \
    mkdir -p /var/www/.config && \
    mkdir -p /var/www/.npm && \
    mkdir -p /var/www/.ssh && \
    chown -R www-data:www-data /var/www/.composer && \
    chown -R www-data:www-data /var/www/.config && \
    chown -R www-data:www-data /var/www/.npm && \
    chown -R www-data:www-data /var/www/.ssh