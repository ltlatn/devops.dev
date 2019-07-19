FROM php:7.3.7-stretch

RUN apt-get update && apt-get install -y libmcrypt-dev libpq-dev zlib1g-dev libpng-dev libjpeg-dev ffmpeg unzip curl less \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo_pgsql pgsql bcmath zip gd

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get install -y --no-install-recommends --no-install-suggests nano cron rclone

RUN apt-get install -y libjpeg-dev libpng-dev libfreetype6-dev

RUN docker-php-ext-configure gd \
    --enable-gd-native-ttf \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2

RUN docker-php-ext-install gd

WORKDIR /var/www
