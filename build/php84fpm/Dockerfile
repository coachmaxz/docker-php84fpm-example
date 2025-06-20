FROM php:8.4.7-fpm
LABEL maintainer="Eakkabin Jaikeawma <eakakbin.max@outlook.com>"

ARG PSR_VERSION=1.2.0

RUN set -xe && \
    apt-get update && apt-get install -y build-essential curl git gzip libzip-dev zip libonig-dev --no-install-recommends && \
    docker-php-ext-install -j$(nproc) iconv pdo mysqli pdo_mysql mbstring sockets zip && \
    docker-php-ext-configure zip && \
    ## Download PHP Composer ##
    curl -sS https://getcomposer.org/installer | php && chmod +x composer.phar && \
    mv composer.phar /usr/local/bin/composer && \
    cd .. && \
    ## Download PSR ##
    curl -LO https://github.com/jbboehr/php-psr/archive/v${PSR_VERSION}.tar.gz && \
    tar xzf ${PWD}/v${PSR_VERSION}.tar.gz && \
    docker-php-ext-install -j $(getconf _NPROCESSORS_ONLN) \
        ${PWD}/php-psr-${PSR_VERSION} \
    && \
    ## Remove all temp files ##
    rm -r \
        ${PWD}/v${PSR_VERSION}.tar.gz \
        ${PWD}/php-psr-${PSR_VERSION} \
    && \
    apt-get clean && apt autoremove -y && \
    rm -r /var/lib/apt/lists/* && rm -rf /tmp/* /var/tmp/*

WORKDIR /usr/share/nginx/html
EXPOSE 9000

CMD ["php-fpm"]