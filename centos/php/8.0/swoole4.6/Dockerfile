FROM liangguifeng/ubuntu:v1

LABEL maintainer="liangguifeng<1476982312@qq.com>" version="1.0" license="MIT" app.name="ubuntu-php8-swoole4.6"

ARG timezone

ENV TIMEZONE=${timezone:-"Asia/Shanghai"} \
    APP_ENV=prod \
    SCAN_CACHEABLE=(true) \
    # install and remove building packages
    PHPIZE_DEPS="autoconf dpkg-dev dpkg file g++ gcc libc-dev make php8.0-dev pkgconf re2c libtool automake libaio-dev"

RUN set -ex \
    && apt-get update \
    && apt-get install -y software-properties-common \
    && add-apt-repository ppa:ondrej/php \
    && apt-get install -y tzdata curl wget \
    && ln -fs /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && dpkg-reconfigure -f noninteractive tzdata \
    && apt-get install -y php8.0 \
    php8.0-bcmath \
    php8.0-curl \
    php8.0-dom \
    php8.0-gd \
    php8.0-mbstring \
    php8.0-mysql \
    php8.0-redis \
    php8.0-zip
    && apt-get install -y git \
    && apt-get install -y $PHPIZE_DEPS \
    # install composer
    && curl -sfL https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer \
    && chmod +x /usr/bin/composer \
    && composer self-update 2.0.13 \
    # download swoole
    && cd /tmp \
    && curl -SL "https://github.com/swoole/swoole-src/archive/refs/tags/v4.6.7.tar.gz" -o swoole.tar.gz \
    && ls -alh \
    # php extension:swoole
    && mkdir -p swoole \
    && tar -xf swoole.tar.gz -C swoole --strip-components=1 \
    && ( \
        cd swoole \
        && phpize \
        && ./configure --enable-mysqlnd --enable-openssl --enable-http2 \
        && make -s -j$(nproc) && make install \
    ) \
    && echo "memory_limit=1G" > /etc/php/8.0/cli/conf.d/00-default.ini \
    && echo "extension=swoole.so" > /etc/php/8.0/cli/conf.d/50-swoole.ini \
    && echo "swoole.use_shortname = 'Off'" >> /etc/php/8.0/cli/conf.d/50-swoole.ini \
    # clear
    && php -v \
    && php -m \
    && php --ri swoole \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"

