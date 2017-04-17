FROM php:7

RUN sed -i 's/deb\.debian\.org/mirrors.aliyun.com/' /etc/apt/sources.list && sed -i 's/security\.debian\.org/mirrors.aliyun.com\/debian-security/' /etc/apt/sources.list

RUN apt-get update && apt-get install -y \ 
	software-properties-common \
	apt-transport-https \
	ca-certificates \
    git \
	curl \
    libfreetype6-dev \
    vim \
    wget \
    sudo \
    zip \
    bzip2 \
    xz-utils \
    sqlite \
    mysql-client \
    libjpeg62-turbo-dev \
    libpng12-dev \
    python-pip \
 && docker-php-ext-configure gd \
    --with-freetype-dir=/usr \
    --with-jpeg-dir=/usr \
 && docker-php-ext-install -j$(nproc) \
    gd \
    pdo_mysql \
 && pip install -U pip \
 && pip install shadowsocks

ADD composer /usr/local/bin/composer
RUN chmod +x /usr/local/bin/composer

# redis
RUN apt-get install -y redis-tools

ARG USERPASS=www-data

RUN usermod -u 1000 -s /bin/bash -G sudo,www-data -d /home/www-data www-data \
 && mkdir /home/www-data \
 && chown -R www-data:www-data /home/www-data \
 && echo www-data:${USERPASS} | chpasswd \
 && echo 'alias redis-cli="redis-cli -h redis"' >> ~/.bashrc \
 && echo 'alias mysql="mysql -h mysql"' >> ~/.bashrc \
 && echo 'PATH="$PATH:/usr/local/node/bin"' >> ~/.bashrc \
 && echo '\n#alias for cnpm\nalias cnpm="npm --registry=https://registry.npm.taobao.org --cache=$HOME/.npm/.cache/cnpm \
  --disturl=https://npm.taobao.org/dist \
  --userconfig=$HOME/.cnpmrc"' >> ~/.bashrc \
 && . ~/.bashrc

COPY node.tar.xz /usr/local
RUN mkdir /usr/local/node \
 && tar Jxf /usr/local/node.tar.xz -C /usr/local/node --strip-components=1 \
 && rm /usr/local/node.tar.xz

USER www-data

RUN echo 'PATH="$PATH:$HOME/.composer/vendor/bin"' >> ~/.bashrc \
 && . ~/.bashrc \
 && composer config -g repo.packagist composer https://packagist.phpcomposer.com

RUN echo 'alias redis-cli="redis-cli -h redis"' >> ~/.bashrc \
 && echo 'alias mysql="mysql -h mysql"' >> ~/.bashrc \
 && echo 'PATH="$PATH:/usr/local/node/bin"' >> ~/.bashrc \
 && echo '\n#alias for cnpm\nalias cnpm="npm --registry=https://registry.npm.taobao.org --cache=$HOME/.npm/.cache/cnpm \
  --disturl=https://npm.taobao.org/dist \
  --userconfig=$HOME/.cnpmrc"' >> ~/.bashrc \
 && . ~/.bashrc

USER root

RUN echo "alias egrep='egrep --color=auto'" >> ~/.bashrc \
 && echo "alias fgrep='fgrep --color=auto'" >> ~/.bashrc \
 && echo "alias grep='grep --color=auto'" >> ~/.bashrc \
 && echo "alias l='ls -lCF'" >> ~/.bashrc \
 && echo "alias la='ls -A'" >> ~/.bashrc \
 && echo "alias ll='ls -alF'" >> ~/.bashrc \
 && echo "alias ls='ls --color=auto'" >> ~/.bashrc

USER www-data

RUN echo "alias egrep='egrep --color=auto'" >> ~/.bashrc \
 && echo "alias fgrep='fgrep --color=auto'" >> ~/.bashrc \
 && echo "alias grep='grep --color=auto'" >> ~/.bashrc \
 && echo "alias l='ls -lCF'" >> ~/.bashrc \
 && echo "alias la='ls -A'" >> ~/.bashrc \
 && echo "alias ll='ls -alF'" >> ~/.bashrc \
 && echo "alias ls='ls --color=auto'" >> ~/.bashrc

WORKDIR /var/www/
