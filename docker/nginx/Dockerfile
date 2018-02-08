FROM nginx:1.10

MAINTAINER Maju Ansari

RUN DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y vim
# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log

ADD ./conf/nginx.conf /etc/nginx/

COPY sites/*.conf /etc/nginx/sites-available/

ARG PHP_UPSTREAM=php-fpm:9000

RUN echo "upstream php-upstream { server ${PHP_UPSTREAM}; }" > /etc/nginx/conf.d/upstream.conf

CMD ["nginx"]

EXPOSE 81 443

# cleanup
USER root
RUN apt-get clean && \
	rm -rfv \
	/tmp/* \
	/var/lib/apt/lists/* \
	/var/tmp/*