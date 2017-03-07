FROM php:7.1.2-fpm

RUN sed -i 's/deb\.debian\.org/mirrors.aliyun.com/' /etc/apt/sources.list && sed -i 's/security\.debian\.org/mirrors.aliyun.com\/debian-security/' /etc/apt/sources.list

RUN apt-get update && apt-get install -y \ 
	--no-install-recommends \
	libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng12-dev \
 && rm -rf /var/lib/apt/lists/* \
 && docker-php-ext-configure gd \
    --with-freetype-dir=/usr \
    --with-jpeg-dir=/usr \
 && docker-php-ext-install -j$(nproc) \
    gd \
    mcrypt \
    zip \
    pdo_mysql \
    opcache \
 && rm -rf /var/www/html

COPY ./conf/php.ini /usr/local/etc/php/conf.d/
COPY ./conf/opcache-recommended.ini /usr/local/etc/php/conf.d/

WORKDIR /var/www/
