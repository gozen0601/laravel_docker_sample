FROM php:8.2-apache

ADD php.ini /usr/local/etc/php/

RUN cd /etc/apache2/mods-enabled \
    && ln -s ../mods-available/rewrite.load

ADD 000-default.conf /etc/apache2/sites-enabled/

# 必要なパッケージをインストール
RUN apt-get update \
  && apt-get install -y zlib1g-dev mariadb-client vim libzip-dev unzip p7zip \
  && docker-php-ext-install zip pdo_mysql

# Composer install
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

# Composerの設定
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

# Laravel Installerをグローバルにインストール
RUN composer global require "laravel/installer"

WORKDIR /var/www