ARG PHP_VERSION
FROM registry.cn-hangzhou.aliyuncs.com/hangjia/php:${PHP_VERSION}-fpm

RUN apt-get install ImageMagick \
    && apt-get install libmagickwand-dev libmagickcore-dev \
    && docker-php-ext-install -j$(nproc) bcmath imagick



WORKDIR /var/www/html