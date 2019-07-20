FROM php:7.3.7-fpm-stretch

RUN apt-get update \
    && apt-get install -y \
        build-essential curl git python ffmpeg unzip less autoconf g++ make nano cron rclone \
        libglib2.0-dev libmcrypt-dev libpq-dev zlib1g-dev libpng-dev libjpeg-dev libfreetype6-dev libzip-dev

RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql
RUN docker-php-ext-configure gd \
    --enable-gd-native-ttf \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2

RUN docker-php-ext-install pdo_pgsql pgsql bcmath zip gd

RUN pecl install redis \
    && docker-php-ext-enable redis

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN cd /tmp \
    && git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git \
    && export PATH=`pwd`/depot_tools:"$PATH" \
    && fetch v8 \
    && cd v8 \
    && git checkout 6.4.388.18 \
    && gclient sync \
    && tools/dev/v8gen.py -vv x64.release -- is_component_build=true \
    && ninja -C out.gn/x64.release/ \
    && cp out.gn/x64.release/lib*.so /usr/lib/ && cp -R include/* /usr/include \
    && cp out.gn/x64.release/natives_blob.bin /usr/lib \
    && cp out.gn/x64.release/snapshot_blob.bin /usr/lib \
    && cd out.gn/x64.release/obj \
    && ar rcsDT libv8_libplatform.a v8_libplatform/*.o

RUN printf "\n" | pecl install v8js \
    && echo 'extension=v8js.so' >> /usr/local/etc/php/conf.d/30-v8js.ini \
    && kill -USR2 1

WORKDIR /var/www
EXPOSE 9000
