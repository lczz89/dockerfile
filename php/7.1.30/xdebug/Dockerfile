FROM registry.cn-hangzhou.aliyuncs.com/zev-php/php:7.1.30-fpm

COPY pecl/ /usr/src/pel/

RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install /usr/src/pel/xdebug* \
    && docker-php-ext-enable xdebug \
    && rm -rf /usr/src/pel/xdebug*

WORKDIR /var/www/html