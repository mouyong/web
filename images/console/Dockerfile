FROM php:7

RUN sed -i 's/deb\.debian\.org/mirrors.aliyun.com/' /etc/apt/sources.list && sed -i 's/security\.debian\.org/mirrors.aliyun.com\/debian-security/' /etc/apt/sources.list

RUN apt-get update && apt-get install -y \ 
	software-properties-common \
	apt-transport-https \
	ca-certificates \
	curl \
    libfreetype6-dev \
    vim \
    wget \
    sudo \
    zip \
    sqlite \
    mysql-client \
    libjpeg62-turbo-dev \
    libpng12-dev \
 && docker-php-ext-configure gd \
    --with-freetype-dir=/usr \
    --with-jpeg-dir=/usr \
 && docker-php-ext-install -j$(nproc) \
    gd \
    pdo_mysql



#RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
# && php -r "if (hash_file('SHA384', 'composer-setup.php') === '55d6ead61b29c7bdee5cccfb50076874187bd9f21f65d8991d46ec5cc90518f447387fb9f76ebae1fbbacf329e583e30') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
# && php composer-setup.php \
# && php -r "unlink('composer-setup.php');" \
# && mv composer.phar /usr/local/bin/composer

ADD composer /usr/local/bin/composer
RUN chmod +x /usr/local/bin/composer

# redis
ENV REDIS_VERSION 3.2.8
ENV REDIS_DOWNLOAD_URL http://download.redis.io/releases/redis-$REDIS_VERSION.tar.gz

RUN wget -O redis.tar.gz "$REDIS_DOWNLOAD_URL" \
 && mkdir -p /usr/src/redis \
 && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
 && rm redis.tar.gz \
 && make -C /usr/src/redis \
 && make -C /usr/src/redis install \
 && rm -r /usr/src/redis


ARG USERPASS=www-data

RUN usermod -u 1000 -s /bin/bash -G sudo,www-data -d /home/www-data www-data \
 && mkdir /home/www-data \
 && chown -R www-data:www-data /home/www-data \
 && echo www-data:${USERPASS} | chpasswd

USER www-data

RUN echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"' >> ~/.bashrc \
 && . ~/.bashrc \
 && composer config -g repo.packagist composer https://packagist.phpcomposer.com \
 && composer global require laravel/installer


RUN echo 'alias redis-cli="redis-cli -h redis"' >> ~/.bashrc \
 && echo 'alias mysql="mysql -h mysql"' >> ~/.bashrc \
 && . ~/.bashrc

USER root

ARG INSTALL_NODE=false
ENV NODE_VERSION=6.10.0
ENV NODE_DOWNLOAD_URL=https://nodejs.org/dist/v6.10.0/node-v${NODE_VERSION}-linux-x64.tar.xz
RUN if [ ${INSTALL_NODE} = true ]; then \
    mkdir /usr/local/node \
 && wget -O node.tar.xz "$NODE_DOWNLOAD_URL" \
 && tar -Jxf node.tar.xz -C /usr/src/node --strip-components=1 \
 && rm node.tar.xz \
 && echo 'export PATH="$PATH:/usr/local/node/bin"' >> ~/.bashrc \
 && . ~/.bashrc \
;fi

ARG INSTALL_YARN=false
RUN if [ ${INSTALL_YARN} = true ]; then \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
 && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
 && apt-get update \
 && apt-get install -y \ 
	yarn \
 && yarn config set registry 'https://registry.npm.taobao.org' \
;fi


USER www-data

WORKDIR /var/www/
