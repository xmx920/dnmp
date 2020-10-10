FROM php:7.2-fpm
COPY ./upload.ini /usr/local/etc/php/conf.d/upload.ini
COPY ./source.list /etc/apt/sources.list

RUN apt-get clean all && apt-get update && apt-get install -y --no-install-recommends --allow-downgrades \
        git \
        zlib1g=1:1.2.8.dfsg-5 zlib1g-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libzip-dev \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo pdo_mysql mysqli \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install bcmath

#RUN apt-get update && apt-get install -y libmagickwand-dev --no-install-recommends && rm -rf /var/lib/apt/lists/*
#RUN pecl install imagick \
#    && docker-php-ext-enable imagick

RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  docker-php-ext-enable redis

COPY ./composer.phar /usr/local/bin/composer
RUN  chmod a+x /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/