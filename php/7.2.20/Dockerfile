ARG PHP_VERSION
FROM registry.cn-hangzhou.aliyuncs.com/hangjia/php:7.2.20-fpm

RUN apk add --update --no-cache autoconf g++ imagemagick-dev libtool make pcre-dev \
    && pecl install imagick \
    && docker-php-ext-enable imagick
RUN docker-php-ext-install -j$(nproc) bcmath
RUN apk add ffmpeg
RUN  apk add libpng-dev freetype-dev libjpeg-turbo-dev \
        && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/  \
        && docker-php-ext-install -j$(nproc) gd && docker-php-ext-install mysqli
        && apk del autoconf g++ libtool make pcre-dev

WORKDIR /home/www