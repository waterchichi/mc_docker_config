FROM php:7.2.34-fpm

# 更新 PECL 通道
RUN pecl channel-update pecl.php.net

# 安装系统依赖和 PHP 扩展依赖
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
    && apt-get clean

# 安装 Redis 扩展的 4.x 版本（兼容 PHP 7.2）
RUN pecl install redis-4.3.0 && docker-php-ext-enable redis

# 安装 imagick 扩展
RUN pecl install imagick && docker-php-ext-enable imagick

# 安装 ssh2 扩展
RUN pecl install ssh2 && docker-php-ext-enable ssh2

# 安装 Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /mc_php/server

# 清理缓存
RUN apt-get remove -y git unzip && apt-get autoremove -y && rm -rf /var/lib/apt/lists/*
