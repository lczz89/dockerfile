ARG PHP_VERSION
FROM registry.cn-hangzhou.aliyuncs.com/hangjia/php:7.2.20-fpm

RUN apk add --update --no-cache libstdc++ autoconf automake g++ unzip imagemagick-dev libtool make pcre-dev zlib-dev linux-headers \
    && pecl install imagick \
    && docker-php-ext-enable imagick
RUN docker-php-ext-install -j$(nproc) bcmath
RUN pecl install grpc  && pecl install protobuf \
    && touch $PHP_INI_DIR/conf.d/docker-php-ext-grpc.ini && echo 'extension=grpc.so'> $PHP_INI_DIR/conf.d/docker-php-ext-grpc.ini \
    && touch $PHP_INI_DIR/conf.d/docker-php-ext-protobuf.ini && echo 'extension=protobuf.so'>> $PHP_INI_DIR/conf.d/docker-php-ext-protobuf.ini
RUN apk add libpng-dev freetype-dev libjpeg-turbo-dev zip \
        && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/  \
        && docker-php-ext-install -j$(nproc) gd && docker-php-ext-install mysqli
RUN apk add protobuf
RUN apk add composer
RUN PHP_EXT_DIR=`php -i|grep "^extension_dir"|awk '{ print $3"/" }'` && chmod -R +x $PHP_EXT_DIR/* \
    && apk del libstdc++ autoconf automake g++ libtool make pcre-dev  zlib-dev unzip

WORKDIR /home/www
