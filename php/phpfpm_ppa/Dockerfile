FROM ubuntu:18.04
MAINTAINER khanhicetea@gmail.com

RUN apt clean && apt -y update && apt install -y locales curl software-properties-common git \
  && locale-gen en_US.UTF-8 
RUN LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php
RUN apt update
RUN apt install -y php7.3-bcmath php7.3-bz2 php7.3-cli php7.3-common php7.3-curl \
                php7.3-cgi php7.3-dev php7.3-fpm php7.3-gd php7.3-gmp php7.3-imap php7.3-intl \
                php7.3-json php7.3-ldap php7.3-mbstring php7.3-mysql \
                php7.3-odbc php7.3-opcache php7.3-pgsql php7.3-phpdbg php7.3-pspell \
                php7.3-readline php7.3-recode php7.3-soap php7.3-sqlite3 \
                php7.3-tidy php7.3-xml php7.3-xmlrpc php7.3-xsl php7.3-zip \
                php-tideways php-redis
RUN apt clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN sed -i "s/;date.timezone =.*/date.timezone = UTC/" /etc/php/7.3/cli/php.ini
RUN sed -i "s/;date.timezone =.*/date.timezone = UTC/" /etc/php/7.3/fpm/php.ini
RUN sed -i "s/display_errors = Off/display_errors = On/" /etc/php/7.3/fpm/php.ini
RUN sed -i "s/upload_max_filesize = .*/upload_max_filesize = 10M/" /etc/php/7.3/fpm/php.ini
RUN sed -i "s/post_max_size = .*/post_max_size = 12M/" /etc/php/7.3/fpm/php.ini
RUN sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php/7.3/fpm/php.ini

RUN sed -i -e "s/pid =.*/pid = \/var\/run\/php7.3-fpm.pid/" /etc/php/7.3/fpm/php-fpm.conf
RUN sed -i -e "s/error_log =.*/error_log = \/proc\/self\/fd\/2/" /etc/php/7.3/fpm/php-fpm.conf
RUN sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php/7.3/fpm/php-fpm.conf
RUN sed -i "s/listen = .*/listen = 9000/" /etc/php/7.3/fpm/pool.d/www.conf
RUN sed -i "s/;catch_workers_output = .*/catch_workers_output = yes/" /etc/php/7.3/fpm/pool.d/www.conf

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r "if (hash_file('sha384', 'composer-setup.php') === 'a5c698ffe4b8e849a443b120cd5ba38043260d5c4023dbf93e1558871f1f07f58274fc6f4c93bcfd858c6bd0775cd8d1') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer && \
    chmod +x /usr/local/bin/composer

# Add new user
RUN addgroup -S php && adduser -D -u 1000 -G php php
USER php
RUN composer global require hirak/prestissimo

VOLUME ["/var/www"]
WORKDIR /var/www

EXPOSE 9000
CMD ["php-fpm7.3"]
