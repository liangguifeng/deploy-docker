FROM liangguifeng/ubuntu20.04-php8.0:v1.0

LABEL maintainer="liangguifeng<1476982312@qq.com>" version="1.0" license="MIT" app.name="ubuntu20.04-php8.0-swoole4.6"

ARG timezone

RUN set -ex \
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