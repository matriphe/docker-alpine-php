# Use Alpine Linux
FROM alpine:latest

# Maintainer
MAINTAINER Muhammad Zamroni <halo@matriphe.com>

# Environments
ENV TIMEZONE            Asia/Jakarta
ENV PHP_MEMORY_LIMIT    512M
ENV MAX_UPLOAD          50M
ENV PHP_MAX_FILE_UPLOAD 200
ENV PHP_MAX_POST        100M

# Let's roll
RUN	apk update && \
	apk upgrade && \
	apk add --update tzdata && \
	cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
	echo "${TIMEZONE}" > /etc/timezone && \
	apk add --update \
		php-mcrypt \
		php-soap \
		php-openssl \
		php-gmp \
		php-pdo_odbc \
		php-json \
		php-dom \
		php-pdo \
		php-zip \
		php-mysql \
		php-sqlite3 \
		php-apcu \
		php-pdo_pgsql \
		php-bcmath \
		php-gd \
		php-xcache \
		php-odbc \
		php-pdo_mysql \
		php-pdo_sqlite \
		php-gettext \
		php-xmlreader \
		php-xmlrpc \
		php-bz2 \
		php-memcache \
		php-mssql \
		php-iconv \
		php-pdo_dblib \
		php-curl \
		php-ctype \
		php-phar \
		php-cli && \
    
    # Set environments
	sed -i "s|;*date.timezone =.*|date.timezone = ${TIMEZONE}|i" /etc/php/php.ini && \
	sed -i "s|;*memory_limit =.*|memory_limit = ${PHP_MEMORY_LIMIT}|i" /etc/php/php.ini && \
    sed -i "s|;*upload_max_filesize =.*|upload_max_filesize = ${MAX_UPLOAD}|i" /etc/php/php.ini && \
    sed -i "s|;*max_file_uploads =.*|max_file_uploads = ${PHP_MAX_FILE_UPLOAD}|i" /etc/php/php.ini && \
    sed -i "s|;*post_max_size =.*|post_max_size = ${PHP_MAX_POST}|i" /etc/php/php.ini && \
    sed -i "s|;*cgi.fix_pathinfo=.*|cgi.fix_pathinfo= 0|i" /etc/php/php.ini && \
	
	# Cleaning up
	mkdir /www && \
	apk del tzdata && \
	rm -rf /var/cache/apk/*

# Set Workdir
WORKDIR /www

# Expose volumes
VOLUME ["/www"]

# Expose ports
EXPOSE 9000

# Entry point
CMD ["/usr/bin/php", "-a"]