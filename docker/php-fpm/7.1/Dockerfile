FROM php:7.1.14-fpm

MAINTAINER Maju Ansari

RUN apt-get update

RUN apt-get -y install vim
RUN apt-get install -y \
    freetds-dev \
    libicu-dev \
    libpq-dev \
    libmcrypt-dev \
 && cp -s /usr/lib/x86_64-linux-gnu/libsybdb.so /usr/lib/ \
 && docker-php-ext-configure pdo_mysql \
 && docker-php-ext-install \
    intl \
    mbstring \
    mcrypt \
    pcntl \
    pdo_dblib \
    pdo_mysql \
    pdo_pgsql \
    pgsql \
    zip \
    opcache


RUN apt-get install -y --no-install-recommends \
        libmagickwand-dev \
    && rm -rf /var/lib/apt/lists/* \
    && pecl install imagick-3.4.3 \
    && docker-php-ext-enable imagick
# Install composer and add its bin to the PATH.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

EXPOSE 9000
COPY ./conf/www.conf /usr/local/etc/php/conf.d/www.conf
COPY ./conf/php.ini /usr/local/etc/php.ini
# cleanup
USER root
RUN apt-get clean && \
	rm -rfv \
	/tmp/* \
	/var/lib/apt/lists/* \
	/var/tmp/*