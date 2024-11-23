FROM php:7.2.34-fpm as builder

# 更新 PECL 通道
RUN pecl channel-update pecl.php.net

# 安装系统依赖
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
    make \
    && rm -rf /var/lib/apt/lists/*

# 安装 PHP PECL 扩展
RUN pecl install \
    redis-4.3.0 \
    imagick \
    ssh2 \
    msgpack \
    igbinary \
    zip \
    xlswriter \
    && docker-php-ext-enable \
    redis \
    imagick \
    ssh2 \
    msgpack \
    igbinary \
    zip \
    xlswriter

# 安装 PHP 核心扩展
RUN docker-php-ext-install -j$(nproc) \
    bcmath \
    bz2 \
    calendar \
    exif \
    gettext \
    gmp \
    intl \
    ldap \
    mbstring \
    mysqli \
    pdo \
    pdo_mysql \
    pdo_pgsql \
    pgsql \
    shmop \
    sysvmsg \
    sysvsem \
    sysvshm \
    tidy \
    sockets \
    opcache

# 配置并安装 GD
RUN docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include/freetype2 \
    --with-jpeg-dir=/usr/include \
    && docker-php-ext-install -j$(nproc) gd

# 安装 Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# 在 builder 阶段末尾，创建 opcache 配置文件
RUN echo '\
zend_extension=opcache.so\n\
opcache.enable=1\n\
opcache.enable_cli=1\n\
opcache.validate_timestamps=1\n\
opcache.revalidate_freq=1\n\
opcache.memory_consumption=128\n\
opcache.interned_strings_buffer=8\n\
opcache.max_accelerated_files=12000\n\
opcache.fast_shutdown=1\n\
' > /usr/local/etc/php/conf.d/opcache.ini

# 使用多阶段构建创建最终镜像
FROM php:7.2.34-fpm

# 复制已编译的扩展和配置
COPY --from=builder /usr/local/lib/php/extensions/ /usr/local/lib/php/extensions/
COPY --from=builder /usr/local/etc/php/conf.d/ /usr/local/etc/php/conf.d/
COPY --from=builder /usr/local/bin/composer /usr/local/bin/composer

# 安装运行时必需的库
RUN apt-get update && apt-get install -y \
    libmagickwand-6.q16-6 \
    libssh2-1 \
    libzip4 \
    libicu63 \
    libpq5 \
    libtidy5deb1 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /mc_php/mc
