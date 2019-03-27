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

RUN  apt-get update \
	 && apt-get install -y libgmp-dev re2c libmhash-dev
RUN ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/local/include/
RUN docker-php-ext-configure gmp
RUN docker-php-ext-install gmp

# Fixing image issues
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && docker-php-ext-install gd exif bcmath gmp
RUN pecl install opencensus-alpha
RUN echo "extension=opencensus.so" >> /usr/local/etc/php/conf.d/docker-php-ext-opencensus.ini

#RUN bash <(curl -Ss https://my-netdata.io/kickstart.sh) install bash

#Install prom node exporter
RUN apt-get install -y wget
RUN wget https://github.com/prometheus/node_exporter/releases/download/v0.17.0/node_exporter-0.17.0.linux-amd64.tar.gz
RUN tar xvfz node_exporter-0.17.0.linux-amd64.tar.gz
RUN  cd node_exporter-0.17.0.linux-amd64
RUN nohup ./node_exporter &
#install xdebug
RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini

#Install php-soap
RUN apt-get install -y libxml2-dev \
    && pear install -a SOAP-0.13.0 \
    && docker-php-ext-install soap;

EXPOSE 9000 9111
COPY ./conf/www.conf /usr/local/etc/php/conf.d/www.conf
COPY ./conf/php.ini /usr/local/etc/php/php.ini
# cleanup
USER root
RUN apt-get clean && \
	rm -rfv \
	/tmp/* \
	/var/lib/apt/lists/* \
	/var/tmp/*