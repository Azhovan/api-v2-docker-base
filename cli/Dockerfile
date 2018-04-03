FROM php:7.0-cli-alpine

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
