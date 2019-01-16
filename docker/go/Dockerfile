FROM php:7.1.14-fpm

MAINTAINER Maju Ansari

RUN ls
RUN apt-get update

RUN apt-get install -y \
	vim \
	git \
	wget \
    freetds-dev \
    libicu-dev \
    libpq-dev \
    libmcrypt-dev

RUN wget https://dl.google.com/go/go1.11.2.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf go1.11.2.linux-amd64.tar.gz
RUN export PATH=$PATH:/usr/local/go/bin
ENV GOPATH /go
#ENV GO111MODULE=on
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH

RUN apt-get clean && \
	rm -rfv \
	/tmp/* \
	/var/lib/apt/lists/* \
	/var/tmp/*