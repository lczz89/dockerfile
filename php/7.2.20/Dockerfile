ARG PHP_VERSION
FROM registry.cn-hangzhou.aliyuncs.com/hangjia/php:${PHP_VERSION}-fpm

RUN apk add --no-cache $PHPIZE_DEPS \
    && docker-php-ext-enable -j$(nproc) bcmath

WORKDIR /var/www/html