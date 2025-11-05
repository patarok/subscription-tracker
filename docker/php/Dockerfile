FROM php:8.3-fpm-alpine

ARG WWWGROUP=1000
ARG WWWUSER=1000

ENV COMPOSER_ALLOW_SUPERUSER=1 \
    COMPOSER_HOME=/.composer \
    PATH="/var/www/vendor/bin:${PATH}"

RUN apk add --no-cache \
        bash \
        curl \
        git \
        icu-data-full \
        icu-dev \
        libpng-dev \
        libzip-dev \
        mysql-client \
        oniguruma-dev \
        shadow \
        tzdata \
        unzip \
        nodejs \
        npm \
    && apk add --no-cache --virtual .build-deps \
        autoconf \
        g++ \
        make \
        linux-headers \
    && docker-php-ext-configure intl \
    && docker-php-ext-install \
        bcmath \
        intl \
        pcntl \
        pdo_mysql \
        zip \
    && docker-php-ext-enable opcache \
    && pecl install redis xdebug \
    && docker-php-ext-enable redis xdebug \
    && npm install --global yarn \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && apk del .build-deps \
    && rm -rf /var/cache/apk/* /tmp/pear

WORKDIR /var/www

RUN groupmod -o -g ${WWWGROUP} www-data \
    && usermod -o -u ${WWWUSER} -g ${WWWGROUP} www-data \
    && mkdir -p ${COMPOSER_HOME} \
    && chown -R www-data:www-data /var/www ${COMPOSER_HOME}

CMD ["php-fpm"]
