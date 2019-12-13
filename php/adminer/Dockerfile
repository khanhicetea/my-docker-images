FROM abiosoft/caddy:1.0.3-no-stats as caddy

FROM php:7.4-fpm-alpine

RUN set -ex \
    && apk --no-cache add postgresql-libs postgresql-dev \
    && docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install opcache mysqli pdo_mysql pgsql pdo_pgsql \
    && rm -rf /tmp/*

COPY config/zzz-adminer.conf /usr/local/etc/php-fpm.d/zzz-adminer.conf
COPY config/adminer.ini /usr/local/etc/php/conf.d/adminer.ini
COPY adminer-mysql.php /www/adminer.php
COPY config/Caddyfile /etc/Caddyfile

EXPOSE 8888

COPY --from=caddy /usr/bin/caddy /usr/bin/caddy

ENTRYPOINT ["/usr/bin/caddy"]
CMD ["--conf", "/etc/Caddyfile", "--log", "stdout"]
