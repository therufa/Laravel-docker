FROM php:7.3-fpm

# Install core packages
RUN apt-get update -qq && apt-get install -yqq --no-install-recommends \
        curl \
        libfreetype6-dev \
        libjpeg-dev \
        libmcrypt-dev \
        libmemcached-dev \
        libpng-dev \
        libpq-dev \
        libssl-dev \
        libz-dev \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pdo_pgsql \
    && docker-php-ext-configure gd \
        --with-jpeg-dir=/usr/lib \
        --with-freetype-dir=/usr/include/freetype2 \
    && docker-php-ext-install gd

# Extension: bcmath
ARG PHP_EXT_BCMATH=false
RUN if [ ${PHP_EXT_BCMATH} = true ]; then \
    docker-php-ext-install bcmath \
;fi

# Extension: mysqli
ARG PHP_EXT_MYSQLI=false
RUN if [ ${PHP_EXT_MYSQLI} = true ]; then \
    docker-php-ext-install mysqli \
;fi

# Extension: imap
ARG PHP_EXT_IMAP=false
RUN if [ ${PHP_EXT_IMAP} = true ]; then \
    apt-get update -yqq && \
    apt-get install -yqq libc-client-dev libkrb5-dev && \
    rm -r /var/lib/apt/lists/* && \
    docker-php-ext-configure imap --with-kerberos --with-imap-ssl && \
    docker-php-ext-install imap \
;fi

# Extension: apcu
ARG PHP_EXT_APCU=false
RUN if [ ${PHP_EXT_APCU} = true ]; then \
    pecl install apcu; \
    docker-php-ext-enable apcu \
;fi

# Extension: exif
ARG PHP_EXT_EXIF=false
RUN if [ ${PHP_EXT_EXIF} = true ]; then \
    docker-php-ext-install exif \
;fi

# Extension: opcache
ARG PHP_EXT_OPCACHE=false
RUN if [ ${PHP_EXT_OPCACHE} = true ]; then \
    docker-php-ext-install opcache \
;fi

# Image optimizers
ARG PHP_EXT_IMAGE_OPTIMIZERS=false
RUN if [ ${PHP_EXT_IMAGE_OPTIMIZERS} = true ]; then \
    apt-get update -yqq && \
    apt-get install -y jpegoptim optipng pngquant gifsicle \
;fi

# Extension: ImageMagick
ARG PHP_EXT_IMAGEMAGICK=false
RUN if [ ${PHP_EXT_IMAGEMAGICK} = true ]; then \
    apt-get update -yqq && \
    apt-get install -yqq libmagickwand-dev imagemagick && \
    pecl install imagick && \
    docker-php-ext-enable imagick \
;fi

# Extension: intl
ARG PHP_EXT_INTL=false
RUN if [ ${PHP_EXT_INTL} = true ]; then \
    apt-get update -yqq && \
    apt-get install -yqq zlib1g-dev libicu-dev g++ && \
    docker-php-ext-configure intl && \
    docker-php-ext-install intl \
;fi

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

RUN usermod -u 1000 www-data
WORKDIR /var/www
CMD ["php-fpm"]
EXPOSE 9000