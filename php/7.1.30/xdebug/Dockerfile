FROM registry.cn-hangzhou.aliyuncs.com/zev-php/php:7.1.30

COPY pecl/ /usr/src/pel/

RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install /usr/src/pel/xdebug* \
    && docker-php-ext-enable xdebug \
    && apk add libzip-dev \
    && docker-php-ext-install zip \
    && rm -rf /usr/src/pel/xdebug*

WORKDIR /var/www/html