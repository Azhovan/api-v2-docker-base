FROM php:7.0-fpm-alpine

ENV APP_DIR /server/http
WORKDIR ${APP_DIR}

# Install PHP dependencies
# See https://github.com/docker-library/php
RUN set -ex \
    && apk add --no-cache libpq libmagic libmcrypt libmcrypt-dev icu-dev \
    && apk add --no-cache --virtual .build-dependencies ${PHPIZE_DEPS} postgresql-dev \
    && docker-php-ext-install bcmath intl mcrypt opcache pcntl pdo_pgsql pdo_mysql \
    && pecl install mongodb redis \
    && docker-php-ext-enable mongodb redis \
    && apk del .build-dependencies

RUN set -ex \
    && sed -i -e "s/;request_terminate_timeout\s*=[^\n]*/request_terminate_timeout = 300/g" /usr/local/etc/php-fpm.d/www.conf \
    && sed -i -e "s/pm.max_children\s*=[^\n]*/pm.max_children = 24/g" /usr/local/etc/php-fpm.d/www.conf \
    && sed -i -e "s/pm.min_spare_servers\s*=[^\n]*/pm.min_spare_servers = 7/g" /usr/local/etc/php-fpm.d/www.conf \
    && sed -i -e "s/pm.start_servers\s*=[^\n]*/pm.start_servers = 7/g" /usr/local/etc/php-fpm.d/www.conf \
    && sed -i -e "s/pm.max_spare_servers\s*=[^\n]*/pm.max_spare_servers = 17/g" /usr/local/etc/php-fpm.d/www.conf \
    && php-fpm --test
