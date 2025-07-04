# src: https://github.com/qlico/qlico/blob/98f287ff4fcb050de2906eae59d3a6dcc75c2fe8/project-examples/Dockerfile
FROM php:8.4.8-fpm-alpine3.21 AS base
LABEL maintainer="Qlico <hello@qlico.dev>"

# PHP-FPM defaults
ENV FCGI_CONNECT=/php-socket/php-fpm.sock
ENV FCGI_BACKLOG=128
ENV PHP_FPM_PM=dynamic
ENV PHP_FPM_PM_MAX_CHILDREN=5
ENV PHP_FPM_PM_START_SERVERS=2
ENV PHP_FPM_PM_MIN_SPARE_SERVERS=1
ENV PHP_FPM_PM_MAX_SPARE_SERVERS=3
ENV PHP_FPM_PM_PROCESS_IDLE_TIMEOUT=10
ENV PHP_FPM_PM_MAX_REQUESTS=1000

# PHP-OPcache defaults
ENV PHP_OPCACHE_ENABLE=1
ENV PHP_OPCACHE_MEMORY_CONSUMPTION=256
ENV PHP_OPCACHE_MAX_ACCELERATED_FILES=20000
ENV PHP_OPCACHE_REVALIDATE_FREQUENCY=0
ENV PHP_OPCACHE_VALIDATE_TIMESTAMPS=0

ARG LOCAL_USER_ID=1000
ARG LOCAL_GROUP_ID=1000

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN apk add --no-cache --virtual .persistent-deps \
    fcgi \
    git \
    openssh-client \
    patch \
    && chmod +x /usr/local/bin/install-php-extensions \
    && install-php-extensions \
        APCu-stable \
        bcmath \
        exif \
        gettext \
        gmp \
        intl \
        opcache \
        pcntl \
        pdo_mysql \
        pdo_pgsql \
        redis-stable \
        shmop \
        soap \
        sockets \
        sysvmsg \
        sysvsem \
        sysvshm \
        xsl \
        zip-stable \
    && docker-php-source delete \
    && apk --no-cache -U upgrade \
    && rm -rf /tmp/* \
    && addgroup -g $LOCAL_GROUP_ID -S qlico \
    && adduser -u $LOCAL_USER_ID -S qlico -G qlico \
    && mkdir -p /php-socket \
    && chown qlico:qlico /php-socket

# Install composer
COPY --from=composer:2 /usr/bin/composer /usr/local/bin/composer.phar

# Add composer and php scripts for aliases.
COPY qlico/services/php/scripts /usr/local/sbin
RUN chmod +x /usr/local/sbin/composer \
             /usr/local/sbin/php \
             /usr/local/sbin/xphp \
             /usr/local/sbin/php-fpm-healthcheck \
    # Disabled access logs for php-fpm
    && sed -i 's/access.log = \/proc\/self\/fd\/2/access.log = \/proc\/self\/fd\/1/g' /usr/local/etc/php-fpm.d/docker.conf \
    # Remove configuration files which are templated during the entrypoint command
    && rm /usr/local/etc/php-fpm.d/zz-docker.conf /usr/local/etc/php-fpm.d/www.conf.default

# php.ini
COPY qlico/services/php/prod/php.ini $PHP_INI_DIR

# www.cnf
COPY qlico/services/php/www.conf /usr/local/etc/php-fpm.d/www.conf

# Don't run as the default (root) user.
USER qlico

CMD ["php-fpm"]

FROM base AS dev
USER root

RUN apk add --no-cache --virtual . \
    # Local mail handling
    msmtp \
    && install-php-extensions \
        xdebug-stable

# MSMTP config.
COPY qlico/services/php/dev/msmtprc /etc/msmtprc

# Xdebug config.
COPY qlico/services/php/dev/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# php.ini
COPY qlico/services/php/dev/php.ini /usr/local/etc/php

# Don't run as the default (root) user.
USER qlico

CMD ["php-fpm"]

FROM base AS prod
USER root

RUN set -xe \
    # Remove packages we don't want in production
    && curl -LO https://github.com/DataDog/dd-trace-php/releases/latest/download/datadog-setup.php \
    && php datadog-setup.php --php-bin=all --enable-profiling \
    && rm -rf datadog-setup.php \
    && rm -rf /usr/local/sbin/composer \
    && rm -rf /usr/local/sbin/composer \
    && rm -rf /usr/local/bin/composer.phar \
    && rm -rf /usr/local/sbin/xphp \
    && apk del git \
    && rm -rf /usr/bin/git \
    # create app folder
    && mkdir /app \
    && chown qlico: /app

COPY qlico/services/php/zzz-opcache.ini $PHP_INI_DIR

# Copy the application to the Docker image.
COPY --chown=qlico:qlico . /app

# Remove qlico folder.
RUN rm -rf /app/qlico

# Don't run as the default (root) user.
USER qlico

CMD ["php-fpm"]
