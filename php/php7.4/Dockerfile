FROM php:7.4-fpm-alpine

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \
        set -eux; \
    	apk add --update --no-cache autoconf g++ imagemagick-dev libtool make pcre-dev
RUN docker-php-ext-install -j$(nproc) bcmath
RUN  apk add imagemagick-dev libpng-dev freetype-dev libjpeg-turbo-dev  libwebp-dev
RUN docker-php-ext-configure gd --with-webp=/usr/include/webp --with-freetype=/usr/include/freetype2 --with-jpeg=/usr/include/ \
        && docker-php-ext-install  gd
RUN docker-php-ext-install pdo pdo_mysql mysqli
RUN apk add librdkafka-dev && pecl install rdkafka && docker-php-ext-enable rdkafka
RUN apk del autoconf g++ libtool make pcre-dev

RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
#修改为nuix通信方式
RUN sed -i 's/listen = 9000/listen = \/dev\/shm\/php7-fpm.socket/g' $PHP_INI_DIR/../php-fpm.d/zz-docker.conf \
      && echo  'listen.mode = 0666' >> $PHP_INI_DIR/../php-fpm.d/zz-docker.conf

WORKDIR /home/www
