FROM php:5.6-cli

MAINTAINER "Caudalie" <caudalie.com>

ARG DEBIAN_FRONTEND=noninteractive

ENV COMPOSER_ALLOW_SUPERUSER 1

ARG MAGENTO2_PUB
ENV MAGENTO2_PUB $MAGENTO2_PUB

ARG MAGENTO2_PRI
ENV MAGENTO2_PRI $MAGENTO2_PRI

ENV FORCE_FULL_BUILD 20200715

RUN  \
    apt-get update \
    && apt-get install -y \
        ack-grep dos2unix gettext git gnupg graphviz jq nano python python3 python-pip python3-pip shellcheck sshpass unzip zip \
        libfreetype6-dev libicu-dev libjpeg62-turbo-dev libmcrypt-dev libpng-dev libxml2-utils libxslt-dev \
        python-dev python3-dev \
    && curl -sL https://deb.nodesource.com/setup_8.x | bash \
    && apt-get install -y nodejs \
    && apt-get clean autoclean \
    && apt-get autoremove

RUN \
    pip install awscli

RUN \
    curl -sL -o /usr/local/bin/composer https://getcomposer.org/composer.phar \
    && chmod +x /usr/local/bin/composer \
    && echo "memory_limit=512M" > /usr/local/etc/php/conf.d/php.ini \
    && echo "phar.readonly=Off" >> /usr/local/etc/php/conf.d/php.ini

RUN \
    pecl install redis xdebug-2.5.5 \
    && docker-php-ext-enable redis xdebug \
    && docker-php-ext-install bcmath gd iconv intl mcrypt pdo_mysql soap xsl zip

RUN \
    composer global require squizlabs/php_codesniffer \
    && ln -s ~/.composer/vendor/bin/phpcs /usr/local/bin/phpcs \
    && ln -s ~/.composer/vendor/bin/phpcbf /usr/local/bin/phpcbf \
    && composer global require phpmd/phpmd \
    && ln -s ~/.composer/vendor/bin/phpmd /usr/local/bin/phpmd \
    && composer global require sebastian/phpcpd \
    && ln -s ~/.composer/vendor/bin/phpcpd /usr/local/bin/phpcpd \
    && composer global require phpmetrics/phpmetrics \
    && ln -s ~/.composer/vendor/bin/phpmetrics /usr/local/bin/phpmetrics \
    && composer global config --auth http-basic.repo.magento.com $MAGENTO2_PUB $MAGENTO2_PRI \
    && composer create-project --repository=https://repo.magento.com magento/marketplace-eqp ~/marketplace-eqp \
    && curl -sL -o ~/phpda.pubkey https://raw.github.com/mamuz/PhpDependencyAnalysis/master/download/phpda.pubkey \
    && curl -sL -o ~/phpda https://raw.github.com/mamuz/PhpDependencyAnalysis/master/download/phpda \
    && chmod +x ~/phpda \
    && curl -sL -o /usr/local/bin/pdepend http://static.pdepend.org/php/latest/pdepend.phar \
    && chmod +x /usr/local/bin/pdepend \
    && curl -sL -o /usr/local/bin/jsawk http://github.com/micha/jsawk/raw/master/jsawk \
    && chmod +x /usr/local/bin/jsawk \
    && curl -sL -o /usr/local/bin/slack https://raw.githubusercontent.com/rockymadden/slack-cli/master/src/slack \
    && chmod +x /usr/local/bin/slack

RUN \
    mkdir -p ~/eslint/standard ~/eslint/semistandard \
    && npm install -g bower eslint gulp grunt jshint jslint semistandard standard \
    && npm install -g eslint-plugin-import eslint-plugin-node eslint-plugin-promise eslint-plugin-standard \
    && npm install -g eslint-config-standard eslint-config-semistandard \
    && npm install -g jsonlint yaml-lint \
    && echo '{"extends": ["standard"]}' > ~/eslint/standard/.eslintrc.json \
    && echo '{"extends": ["semistandard"]}' > ~/eslint/semistandard/.eslintrc.json

RUN \
    mkdir -p ~/.ssh \
    && chmod 700 ~/.ssh \
    && echo "" > ~/.ssh/known_hosts
