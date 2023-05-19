#
#--------------------------------------------------------------------------
# Image Setup
#--------------------------------------------------------------------------
#

FROM php:8.2-fpm

MAINTAINER Tiknil <info@tiknil.com>

#
#--------------------------------------------------------------------------
# Software's Installation
#--------------------------------------------------------------------------
#
# Installing tools and PHP extentions using "apt", "docker-php", "pecl",
#

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    curl \
    libmemcached-dev \
    libzip-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libwebp-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    git \
    zip \
    unzip \
  && rm -rf /var/lib/apt/lists/*

# Install the PHP mcrypt extention
# RUN docker-php-ext-install mcrypt

# Install the PHP pdo_mysql extention
RUN docker-php-ext-install pdo_mysql

# EXIF to read exif image tags
RUN docker-php-ext-install exif
# Zip extension
RUN docker-php-ext-configure zip \
  && docker-php-ext-install zip
  # Install the PHP gd library
RUN docker-php-ext-configure gd \
    --with-jpeg \
    --with-webp \
    --with-freetype=/usr/include/freetype2 && \
    docker-php-ext-install gd

RUN echo "alias phpunit='php /var/www/vendor/bin/phpunit'" >> ~/.bashrc

# Install php redis
RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  docker-php-ext-enable redis

# Install composer
RUN curl --silent --show-error https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

ADD ./conf.ini /usr/local/etc/php/conf.d

RUN usermod -u 1000 www-data

WORKDIR /var/www

ENV COMPOSER_MEMORY_LIMIT=-1

CMD ["php-fpm"]

EXPOSE 9000
