FROM php:7.3-fpm-alpine

MAINTAINER	Heropoo "aiyouyou1000@163.com"

#set a faster repositorie
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories

RUN apk update && apk add build-base autoconf openssl openssl-dev\
	freetype-dev libjpeg-turbo-dev libpng-dev \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install gd iconv pdo pdo_mysql mysqli pcntl

#mongodb
COPY ../nginx-php/mongodb-1.5.3.tgz /root/ 
RUN cd /root && tar -zxvf mongodb-1.5.3.tgz && cd mongodb-1.5.3 && phpize && ./configure && make && make install && docker-php-ext-enable mongodb

#swoole
COPY ../nginx-php/swoole-src-4.2.10.tar.gz /root/ 
RUN cd /root && tar -zxvf swoole-src-4.2.10.tar.gz && cd swoole-src-4.2.10 && phpize && ./configure && make && make install && docker-php-ext-enable swoole

COPY --from=composer /usr/bin/composer /usr/bin/composer

WORKDIR /srv/www