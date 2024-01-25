FROM php:8.2-apache

# Add php.ini
COPY php.ini /usr/local/etc/php/

# apache setting
RUN cd /etc/apache2/mods-enabled \
    && ln -s ../mods-available/rewrite.load

ADD 000-default.conf /etc/apache2/sites-enabled/

# 必要なパッケージをインストール
RUN apt-get update \
  && apt-get install -y \
  zlib1g-dev \
  libzip-dev \
  p7zip \
  gcc \
  make \
  git \
  unzip \
  vim \
  libpng-dev \
  libjpeg-dev \
  libfreetype6-dev \
  libmcrypt-dev \
  libpq-dev \
  curl \
  gnupg \
  openssl \
  && docker-php-ext-install pdo_pgsql pgsql \
  && docker-php-ext-configure gd --with-freetype --with-jpeg \
  && docker-php-ext-install -j$(nproc) gd

# Composer install
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

# Composer setting
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

# Laravel Installerをグローバルにインストール
RUN composer global require "laravel/installer"

WORKDIR /var/www

# permission
RUN chown -R www-data:www-data /var/www