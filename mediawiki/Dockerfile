FROM php:7.4-fpm-alpine

# PHP extensions
RUN apk update && \
    apk add --no-cache \
        libjpeg-turbo \
        libpng \
        freetype \
        icu \
        libxslt \
        postgresql-libs \
        gmp \
        imagemagick-libs \
        libgomp \
        libzip && \
    apk add --no-cache \
        --virtual .phpize-deps \
        libjpeg-turbo-dev \
        libpng-dev \
        freetype-dev \
        icu-dev \
        libxslt-dev \
        postgresql-dev \
        gmp-dev \
        imagemagick-dev \
        libzip-dev \
		$PHPIZE_DEPS && \
    docker-php-source extract && \
    docker-php-ext-configure gd \
        --with-freetype \
        --with-jpeg && \
    docker-php-ext-configure opcache --enable-opcache && \
    docker-php-ext-install -j"$(nproc)" \
        intl \
        bcmath \
        xsl \
        zip \
        pgsql \
        pdo_pgsql \
        gmp \
        gd \
        opcache \
        mysqli \
        pdo \
        pdo_mysql && \
    pecl install \
        apcu \
        imagick && \
    docker-php-ext-enable \
        apcu \
        imagick \
        pdo_mysql && \
    docker-php-source delete && \
    apk del .phpize-deps && \
    rm -rf /tmp/* /var/cache/apk/*

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
RUN { \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=60'; \
		echo 'opcache.fast_shutdown=1'; \
		echo 'opcache.enable_cli=1'; \
	} > /usr/local/etc/php/conf.d/opcache-recommended.ini


# APC settings
RUN { \
		echo 'apc.shm_size=256M'; \
		echo 'apc.ttl=7200'; \
	} > /usr/local/etc/php/conf.d/apcu-overrides.ini


# https://github.com/wikimedia/mediawiki-docker/blob/master/stable/Dockerfile
ENV MEDIAWIKI_MAJOR_VERSION 1.37
ENV MEDIAWIKI_BRANCH REL1_31
ENV MEDIAWIKI_VERSION 1.37.1
ENV MEDIAWIKI_SHA512 9a8a1be9b0a9ffa506226c8e74cbd817a5ea39f404cf5931530896e5d831e57b7b56ff44d775c00dae88be16f01c9a12bf7d10d25c055674d8de4376d7d9c942

# MediaWiki setup
RUN curl -fSL "https://releases.wikimedia.org/mediawiki/${MEDIAWIKI_MAJOR_VERSION}/mediawiki-${MEDIAWIKI_VERSION}.tar.gz" -o mediawiki.tar.gz \
	&& echo "${MEDIAWIKI_SHA512} *mediawiki.tar.gz" | sha512sum -c - \
	&& tar -xz --strip-components=1 -f mediawiki.tar.gz \
	&& rm mediawiki.tar.gz \
	&& chown -R www-data:www-data extensions skins cache images


CMD ["php-fpm"]
