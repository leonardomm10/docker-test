FROM php:7.4-fpm

# Atualizar e instalar dependências do sistema
RUN apt-get update && apt-get install -y \
    zip unzip git curl libzip-dev \
    libicu-dev \
    libxml2-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libmagickwand-dev \
    unixodbc-dev \
    libmemcached-dev \
    libssl-dev \
    libcurl4-openssl-dev \
    && rm -rf /var/lib/apt/lists/*

# Compilar extensões do core
RUN docker-php-ext-install \
    pdo \
    pdo_mysql \
    mysqli \
    intl \
    mbstring \
    bcmath \
    curl \
    xml \
    xmlrpc \
    zip

# Atualizar canal PECL antes de instalar
RUN pecl channel-update pecl.php.net

# Instalar extensões via PECL compatíveis com PHP 7.4
RUN pecl install apcu \
    && pecl install apcu_bc \
    && docker-php-ext-enable apcu --ini-name 10-docker-php-ext-apcu.ini \
    && docker-php-ext-enable apc --ini-name 20-docker-php-ext-apc.ini \
    && pecl install memcached \
    && pecl install imagick \
    && docker-php-ext-enable memcached imagick

COPY php-fpm.conf /usr/local/etc/php-fpm.d/php-fpm.conf

RUN echo "date.timezone = America/Sao_Paulo" >> /usr/local/etc/php/conf.d/php.ini
RUN echo "memory_limit = 128M" >> /usr/local/etc/php/conf.d/php.ini
RUN echo "max_execution_time = 360" >> /usr/local/etc/php/conf.d/php.ini
RUN echo "short_open_tag = on" >> /usr/local/etc/php/conf.d/php.ini

EXPOSE 9000