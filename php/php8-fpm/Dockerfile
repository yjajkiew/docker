FROM php:8-fpm

# Get repository and install wget and vim
RUN apt-get update && apt-get install --no-install-recommends -y \
        wget \
        vim \
        git \
        unzip

# Install PHP extensions deps
RUN apt-get update \
    && apt-get install --no-install-recommends -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
	libmagickwand-dev \
        libmcrypt-dev \
        libpng-dev \
	libonig-dev \
        libzip-dev \
        libicu-dev \
        g++ \
        unixodbc-dev \
        libxml2-dev \
        libaio-dev \
        libmemcached-dev \
        freetds-dev \
	libssl-dev \
	openssl \
	gcc \
	make \
	autoconf \
	libc-dev \
	libgmp-dev \
	pkg-config

# Install PHP extensions
RUN apt-get update \
  && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
  && docker-php-ext-configure intl \
  && pecl install redis \
  && pecl install memcached \
  && docker-php-ext-install \
            iconv \
            intl \
            gd \
            mysqli \
	    pdo \
            pdo_mysql \
            soap \
            sockets \
            zip \
            pcntl \
            ftp \
	    intl \
	    bcmath \
	    gmp \
  && docker-php-ext-enable \
	    mysqli \
            redis \
            memcached \
            opcache 

# install imagick
# use github version for now until release from https://pecl.php.net/get/imagick is ready for PHP 8
RUN mkdir -p /usr/src/php/ext/imagick \
    && curl -fsSL https://github.com/Imagick/imagick/archive/06116aa24b76edaf6b1693198f79e6c295eda8a9.tar.gz | tar xvz -C "/usr/src/php/ext/imagick" --strip 1 \
    && docker-php-ext-install imagick

# Install APCu and APC backward compatibility
RUN mkdir -p /usr/src/php/ext/apcu \
    && curl -fsSL https://pecl.php.net/get/apcu | tar xvz -C "/usr/src/php/ext/apcu" --strip 1 \
    && docker-php-ext-install apcu

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

#memory limit
RUN echo 'memory_limit = 512M' >> /usr/local/etc/php/conf.d/docker-php-memlimit.ini

# add status page
RUN echo 'pm.status_path = /php8_status\n' \
	'php_admin_flag[log_errors] = on'\
	 >> /usr/local/etc/php-fpm.d/www.conf
