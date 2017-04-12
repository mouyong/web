FROM nginx:alpine

ARG PHP_UPSTREAM=php-fpm

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories

RUN apk update \
 && apk upgrade \
 && apk add --no-cache bash \
 && adduser -D -H -u 1000 -s /bin/bash www-data \
 && rm /etc/nginx/conf.d/default.conf 

RUN echo -e "upstream php-upstream { \n\t\
server ${PHP_UPSTREAM}:9000; \n\
}" > /etc/nginx/conf.d/upstream.conf

CMD ["nginx"]

EXPOSE 80 443

WORKDIR /etc/nginx
