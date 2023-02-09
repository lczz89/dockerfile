FROM alpine:latest
ENV GRPC_MODULE_DEPS  gcc g++ cmake linux-headers libzip-dev make
COPY grpc /root/grpc
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \
        set -eux; \
    	apk add --no-cache --update --virtual .all-deps $GRPC_MODULE_DEPS
RUN cd /root/grpc && mkdir -p cmake/build && cd cmake/build && cmake ../.. \
    && make protoc grpc_php_plugin

FROM php:8.1-fpm-alpine

ENV PHP_MODULE_DEPS  zlib-dev libmemcached-dev cyrus-sasl-dev libpng-dev libxml2-dev krb5-dev curl-dev icu-dev libzip-dev openldap-dev imap-dev postgresql-dev imagemagick-dev libpng-dev freetype-dev libjpeg-turbo-dev  libwebp-dev

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \
        set -eux; \
    	apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
        && apk add --no-cache --update --virtual .all-deps $PHP_MODULE_DEPS
RUN docker-php-ext-install -j$(nproc) bcmath
RUN docker-php-ext-configure gd --with-webp=/usr/include/webp --with-freetype=/usr/include/freetype2 --with-jpeg=/usr/include/ \
        && docker-php-ext-install  gd
RUN docker-php-ext-install pdo pdo_mysql mysqli ldap soap imap  dom opcache
RUN apk add librdkafka-dev && pecl install rdkafka && docker-php-ext-enable rdkafka && docker-php-ext-install sockets && pecl install -o -f redis
RUN rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
    && docker-php-ext-enable sockets \
    && pecl install msgpack && docker-php-ext-enable msgpack \
    && pecl install igbinary && docker-php-ext-enable igbinary \
    && printf "\n\n\n\n\n\n\n\n\n\n" | pecl install memcached \
    && docker-php-ext-enable memcached && docker-php-ext-install zip
RUN apk add composer
RUN apk add linux-headers
RUN pecl install grpc && touch $PHP_INI_DIR/conf.d/docker-php-ext-grpc.ini && echo 'extension=grpc.so'> $PHP_INI_DIR/conf.d/docker-php-ext-grpc.ini
RUN pecl install protobuf && touch $PHP_INI_DIR/conf.d/docker-php-ext-protobuf.ini && echo 'extension=protobuf.so'>> $PHP_INI_DIR/conf.d/docker-php-ext-protobuf.ini
RUN apk add protobuf
RUN apk add --no-cache --repository http://dl-cdn.alpinelinux.org/alpine/v3.13/community/ --allow-untrusted gnu-libiconv
ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php
RUN apk del  .phpize-deps  && rm -rf /var/cache/apk/* /tmp/* /var/tmp/* /root/grpc
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
#修改为nuix通信方式
RUN sed -i 's/listen = 9000/listen = \/dev\/shm\/php8-fpm.socket/g' $PHP_INI_DIR/../php-fpm.d/zz-docker.conf \
      && echo  'listen.mode = 0666' >> $PHP_INI_DIR/../php-fpm.d/zz-docker.conf
COPY --from=0 /root/grpc/cmake/build/grpc_php_plugin  /usr/local/bin/grpc_php_plugin
WORKDIR /home/www
STOPSIGNAL SIGQUIT
