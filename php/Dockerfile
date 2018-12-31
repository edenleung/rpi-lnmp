ARG PHP_VERSION
FROM php:${PHP_VERSION}-fpm
RUN apt update
RUN apt-get install -y \
		libfreetype6-dev \
		libjpeg62-turbo-dev \
		libmcrypt-dev \
		libpng-dev \
        unixodbc-dev \
        gcc g++ make autoconf libc-dev pkg-config

RUN docker-php-ext-configure gd \
    --with-gd \
    --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install gd

RUN docker-php-ext-install -j$(nproc) pdo_mysql \
    && docker-php-ext-install mysqli

RUN docker-php-ext-install pcntl

ARG PHP_SWOOLE=false
RUN if [ ${PHP_SWOOLE} != false ]; then \
    curl -O http://pecl.php.net/get/swoole-${PHP_SWOOLE}.tgz -L \
    && pecl install swoole-${PHP_SWOOLE}.tgz \
    && docker-php-ext-enable swoole \
;fi

ARG PHP_REDIS=false
RUN if [ ${PHP_REDIS} != false ]; then \
    curl -O http://pecl.php.net/get/redis-${PHP_REDIS}.tgz -L \
    && pecl install redis-${PHP_REDIS}.tgz \
    && docker-php-ext-enable redis \
;fi
