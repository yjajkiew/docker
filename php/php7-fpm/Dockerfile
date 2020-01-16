FROM php:7-fpm

# Install pdo_mysql
RUN apt-get update \
  && echo 'deb http://packages.dotdeb.org jessie all' >> /etc/apt/sources.list \
  && echo 'deb-src http://packages.dotdeb.org jessie all' >> /etc/apt/sources.list \
  && apt-get install -y wget \
  && wget https://www.dotdeb.org/dotdeb.gpg \
  && apt-key add dotdeb.gpg \
  && apt-get update \
  && apt-get install -y git \
  && apt-get install -y libmemcached* libpng12-dev libmcrypt-dev libjpeg62-turbo-dev libjpeg-dev libpq-dev libfreetype6-dev libmcrypt-dev \
  && apt-get install -y php7.0-memcache php7.0-memcached php7.0-mysql php7.0-curl php7.0-json php7.0-gd php7.0-memcached php7.0-geoip \
  && docker-php-ext-install pdo_mysql mysqli pdo gd mcrypt opcache \
  && git clone https://github.com/php-memcached-dev/php-memcached /usr/src/php/ext/memcached \
  && cd /usr/src/php/ext/memcached && git checkout -b php7 origin/php7 \
  && docker-php-ext-configure memcached \
  && docker-php-ext-install memcached

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
