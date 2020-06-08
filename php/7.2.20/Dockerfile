ARG PHP_VERSION
FROM registry.cn-hangzhou.aliyuncs.com/hangjia/php:${PHP_VERSION}-fpm

RUN apk add --no-cache file \
    && apk --update add imagemagick
RUN docker-php-ext-install -j$(nproc) bcmath imagick



WORKDIR /var/www/html