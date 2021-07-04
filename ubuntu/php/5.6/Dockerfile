FROM ubuntu:20.04

LABEL maintainer="liangguifeng<1476982312@qq.com>" version="1.0" license="MIT" app.name="ubuntu20.04-php8.0"

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
    && apt-get install -y $PHPIZE_DEPS \
    # clear
    && php -v \
    && php -m \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"