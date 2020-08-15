ARG PHP_VERSION
FROM php:7.4.8-fpm
LABEL maintainer="inhere <543492227@qq.com>" version="1.0"

# --build-arg timezone=Asia/Shanghai
ARG timezone
# default use www-data user
ARG work_user=www-data

ENV TIMEZONE=${timezone:-"Asia/Shanghai"} \
    PHPREDIS_VERSION=5.3.1 \
    SWOOLE_VERSION=4.4.19 \
    MONGODB_VERSION=1.8.0 \
    COMPOSER_ALLOW_SUPERUSER=1

COPY ./sources.list .
# Libs -y --no-install-recommends
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak && mv sources.list /etc/apt/  &&  rm -Rf /var/lib/apt/lists/* \
    && apt-get update \
    && apt-get install -y \
        curl wget git zip unzip less vim procps lsof tcpdump htop openssl \
        zlibc       \
        libonig-dev \
        libzip-dev \
        libz-dev \
        libssl-dev \
        libnghttp2-dev \
        libpcre3-dev \
        libjpeg-dev \
        libpng-dev \
        libfreetype6-dev \
        supervisor \
# Install PHP extensions
    && docker-php-ext-install \
       bcmath gd pdo_mysql mbstring sockets zip sysvmsg sysvsem sysvshm
# Install Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH
# Install redis extension
RUN wget http://pecl.php.net/get/redis-${PHPREDIS_VERSION}.tgz -O /tmp/redis.tar.tgz \
    && pecl install /tmp/redis.tar.tgz \
    && rm -rf /tmp/redis.tar.tgz \
    && docker-php-ext-enable redis \
# Install mongodb extension
     && wget http://pecl.php.net/get/mongodb-${MONGODB_VERSION}.tgz -O /tmp/mongodb.tar.tgz \
     && pecl install /tmp/mongodb.tar.tgz \
     && rm -rf /tmp/mongodb.tar.tgz \
     && docker-php-ext-enable mongodb \
# Install swoole extension
    && wget https://github.com/swoole/swoole-src/archive/v${SWOOLE_VERSION}.tar.gz -O swoole.tar.gz \
    && mkdir -p swoole \
    && tar -xf swoole.tar.gz -C swoole --strip-components=1 \
    && rm swoole.tar.gz \
    && ( \
        cd swoole \
        && phpize \
        && ./configure --enable-mysqlnd --enable-sockets --enable-openssl --enable-http2 \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r swoole \
    && docker-php-ext-enable swoole \
# Clear dev deps
    && apt-get clean \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
# Timezone
    && cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" > /etc/timezone \
    && echo "[Date]\ndate.timezone=${TIMEZONE}" > /usr/local/etc/php/conf.d/timezone.ini