FROM php:7.2.34-fpm

RUN pecl channel-update pecl.php.net

RUN apt-get update && apt-get install -y \
    libmagickwand-dev \
    libssh2-1-dev \
    libzip-dev \
    zlib1g-dev \
    libssl-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    git \
    unzip \
    libicu-dev \
    libxml2-dev \
    libbz2-dev \
    libgmp-dev \
    libldap2-dev \
    libpq-dev \
    libpspell-dev \
    libtidy-dev \
    unixodbc-dev \
    autoconf \
    automake \
    zlib1g-dev \
    make \
    && apt-get clean

RUN pecl install redis-4.3.0 && docker-php-ext-enable redis

RUN pecl install imagick && docker-php-ext-enable imagick

RUN pecl install ssh2 && docker-php-ext-enable ssh2

RUN docker-php-ext-install bcmath

RUN docker-php-ext-install bz2

RUN docker-php-ext-install calendar

RUN docker-php-ext-install exif

RUN docker-php-ext-install gettext

RUN docker-php-ext-install gmp

RUN docker-php-ext-install intl

RUN docker-php-ext-install ldap

RUN docker-php-ext-install mbstring

RUN docker-php-ext-install mysqli

RUN docker-php-ext-install pdo

RUN docker-php-ext-install pdo_mysql

RUN docker-php-ext-install pdo_pgsql

RUN docker-php-ext-install pgsql

RUN pecl install msgpack && docker-php-ext-enable msgpack

RUN pecl install igbinary && docker-php-ext-enable igbinary

RUN docker-php-ext-install shmop

RUN docker-php-ext-install sysvmsg

RUN docker-php-ext-install sysvsem

RUN docker-php-ext-install sysvshm

RUN docker-php-ext-install tidy

RUN pecl install zip && docker-php-ext-enable zip

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/freetype2 --with-jpeg-dir=/usr/include \
    && docker-php-ext-install gd

RUN pecl install xlswriter && docker-php-ext-enable xlswriter

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /mc_php/server

RUN apt-get remove -y git unzip && apt-get autoremove -y && rm -rf /var/lib/apt/lists/*
