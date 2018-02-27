FROM php:5.6-cli

MAINTAINER "Jeremy CURNY" <jeremy.curny.pro@gmail.com>

ARG DEBIAN_FRONTEND=noninteractive

RUN  \
    apt-get update \
    && apt-get install -y graphviz git unzip zip \
    && apt-get clean autoclean \
    && apt-get autoremove

RUN \
    curl -sL -o /usr/local/bin/composer https://getcomposer.org/composer.phar \
    && chmod +x /usr/local/bin/composer

RUN \
    pecl install xdebug-2.5.5 \
    && docker-php-ext-enable xdebug