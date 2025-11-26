FROM php:7.1-fpm

# Ajustar repositórios do Debian Buster (agora no archive)
RUN sed -i 's|deb.debian.org|archive.debian.org|g' /etc/apt/sources.list \
    && sed -i 's|security.debian.org|archive.debian.org|g' /etc/apt/sources.list \
    && sed -i '/buster-updates/d' /etc/apt/sources.list \
    && apt-get update

# Agora sim funciona:
RUN apt-get install -y \
    apache2 libapache2-mod-fcgid \
    zip unzip git curl libzip-dev \
    libicu-dev libxml2-dev libpng-dev libjpeg-dev libfreetype6-dev \
    libonig-dev libmagickwand-dev unixodbc-dev libmemcached-dev \
    libssl-dev libcurl4-openssl-dev \
    && rm -rf /var/lib/apt/lists/*

# Ativar módulos
RUN a2enmod proxy proxy_fcgi setenvif rewrite headers expires actions

# Configurar PHP-FPM para ouvir em TCP
RUN sed -i "s|listen = .*|listen = 9000|g" /usr/local/etc/php-fpm.d/www.conf

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

# Criar VirtualHost
RUN printf '%s\n' \
'<VirtualHost *:8080>' \
'    DocumentRoot /var/www/html' \
'' \
'    <Directory /var/www/html>' \
'        AllowOverride All' \
'        Require all granted' \
'    </Directory>' \
'' \
'    <FilesMatch "\.php$">' \
'        SetHandler "proxy:fcgi://127.0.0.1:9000"' \
'    </FilesMatch>' \
'' \
'</VirtualHost>' \
> /etc/apache2/sites-available/000-default.conf

# Apache na porta 8080
RUN sed -i 's/80/8080/' /etc/apache2/ports.conf

# Copiar arquivos
COPY . /var/www/html
RUN chown -R www-data:www-data /var/www/html

EXPOSE 8080

# Rodar PHP-FPM + Apache corretamente
CMD php-fpm -F & apache2ctl -D FOREGROUND