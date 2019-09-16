FROM php:7.4.0RC1-fpm-alpine3.10

RUN docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install opcache \
    && docker-php-ext-install mysqli \
    && docker-php-ext-enable mysqli \
    && rm -rf /tmp/*

COPY config/zzz-adminer.conf /usr/local/etc/php-fpm.d/zzz-adminer.conf
COPY config/adminer.ini /usr/local/etc/php/conf.d/adminer.ini
COPY adminer.php /www/adminer.php

EXPOSE 22222
