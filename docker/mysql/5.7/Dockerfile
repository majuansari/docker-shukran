FROM mysql:5.7.21



EXPOSE 3306
CMD ["mysqld"]

# cleanup
USER root
RUN apt-get clean && \
	rm -rfv \
	/tmp/* \
	/var/lib/apt/lists/* \
	/var/tmp/*