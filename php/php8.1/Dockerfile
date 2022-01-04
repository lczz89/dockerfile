FROM php:8.1-fpm-alpine

ENV PHP_MODULE_DEPS  zlib-dev libmemcached-dev cyrus-sasl-dev libpng-dev libxml2-dev krb5-dev curl-dev icu-dev libzip-dev openldap-dev imap-dev postgresql-dev imagemagick-dev libpng-dev freetype-dev libjpeg-turbo-dev  libwebp-dev

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \
        set -eux; \
    	apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
        && apk add --no-cache --update --virtual .all-deps $PHP_MODULE_DEPS
RUN docker-php-ext-install -j$(nproc) bcmath
RUN docker-php-ext-configure gd --with-webp=/usr/include/webp --with-freetype=/usr/include/freetype2 --with-jpeg=/usr/include/ \
        && docker-php-ext-install  gd
RUN docker-php-ext-install pdo pdo_mysql mysqli ldap soap imap iconv dom opcache
RUN apk add librdkafka-dev && pecl install rdkafka && docker-php-ext-enable rdkafka && docker-php-ext-install sockets && pecl install -o -f redis
RUN rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
    && docker-php-ext-enable sockets \
    && pecl install msgpack && docker-php-ext-enable msgpack \
    && pecl install igbinary && docker-php-ext-enable igbinary \
    && printf "\n\n\n\n\n\n\n\n\n\n" | pecl install memcached \
    && docker-php-ext-enable memcached && docker-php-ext-install zip
RUN apk add php8-dom php8-xml php8-xmlwriter php8-fileinfo php8-tokenizer composer
RUN apk del  .phpize-deps && rm -rf /var/cache/apk/* /tmp/* /var/tmp/*
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
#修改为nuix通信方式
RUN sed -i 's/listen = 9000/listen = \/dev\/shm\/php8-fpm.socket/g' $PHP_INI_DIR/../php-fpm.d/zz-docker.conf \
      && echo  'listen.mode = 0666' >> $PHP_INI_DIR/../php-fpm.d/zz-docker.conf

WORKDIR /home/www
STOPSIGNAL SIGQUIT
