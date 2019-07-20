FROM php:7.3.7-fpm-stretch

RUN apt-get update \
    && apt-get install -y libmcrypt-dev libpq-dev zlib1g-dev libpng-dev libjpeg-dev libzip-dev libfreetype6-dev \
        ffmpeg unzip curl less nano cron rclone

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN docker-php-ext-configure pgsql \
        --with-pgsql=/usr/local/pgsql

RUN docker-php-ext-configure gd \
        --enable-gd-native-ttf \
        --with-jpeg-dir=/usr/lib \
        --with-freetype-dir=/usr/include/freetype2

RUN docker-php-ext-install pdo_pgsql pgsql bcmath zip gd

WORKDIR /var/www
