FROM php:7-fpm

LABEL maintainer="lux@hilux.fr"

WORKDIR /project/html

EXPOSE 9000

RUN apt-get update && apt-get install -y zlib1g-dev libicu-dev libpq-dev imagemagick zip unzip dnsutils

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN rm /etc/localtime
RUN ln -s /usr/share/zoneinfo/Europe/Paris /etc/localtime
RUN "date"

RUN echo 'alias sf="php bin/console"' >> ~/.bashrc

RUN docker-php-ext-install opcache \
    && docker-php-ext-install intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install zip

RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > $PHP_INI_DIR/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=On" >> $PHP_INI_DIR/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=On" >> $PHP_INI_DIR/conf.d/xdebug.ini \
    && echo "xdebug.remote_connect_back=On" >> $PHP_INI_DIR/conf.d/xdebug.ini

RUN pecl install apcu \
  && docker-php-ext-enable apcu