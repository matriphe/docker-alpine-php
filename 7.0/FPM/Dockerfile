# Use Alpine Linux
FROM alpine:edge

# Maintainer
MAINTAINER Muhammad Zamroni <halo@matriphe.com>

# Environments
ENV TIMEZONE            Asia/Jakarta
ENV PHP_MEMORY_LIMIT    512M
ENV MAX_UPLOAD          50M
ENV PHP_MAX_FILE_UPLOAD 200
ENV PHP_MAX_POST        100M

# Let's roll
RUN	echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
    apk update && \
	apk upgrade && \

    # Set proper time zone
	apk add tzdata && \
	cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
	echo "${TIMEZONE}" > /etc/timezone && \

	# PHP 7 essentials
	apk add  \
        php7-bcmath \
        php7-bz2 \
        php7-ctype \
        php7-curl \
        php7-dom \
        php7-gd \
        php7-gettext \
        php7-gmp \
        php7-iconv \
        php7-json \
        php7-mcrypt \
        php7-openssl \
        php7-soap \
        php7-xmlreader \
        php7-xmlrpc \
        php7-zip && \

	# PHP 7 database drivers
	apk add \
		php7-mysqli \
		php7-sqlite3 \
		php7-odbc \
		php7-pdo \
		php7-pdo_odbc \
		php7-pdo_pgsql \
		php7-pdo_mysql \
		php7-pdo_sqlite \
		php7-pdo_dblib && \

	# PHP 7 caching and debuging, needed only in FPM
	apk add \
        php7-opcache \
        php7-xdebug \
		php7-fpm && \

	# PHP 7 needed only for CLI
	apk add \
        php7-phar \
		php7 && \

    # Set environments
    # Changes to php-fpm.conf
	sed -i "s|;*daemonize\s*=\s*yes|daemonize = no|g" /etc/php7/php-fpm.conf && \
    # Changes to php-fpm pool www.conf
	sed -i "s|;*listen\s*=\s*127.0.0.1:9000|listen = 9000|g" /etc/php7/php-fpm.d/www.conf && \
	sed -i "s|;*listen\s*=\s*/||g" /etc/php7/php-fpm.d/www.conf && \
    # Changes to php.ini
	sed -i "s|;*date.timezone =.*|date.timezone = ${TIMEZONE}|i" /etc/php7/php.ini && \
	sed -i "s|;*memory_limit =.*|memory_limit = ${PHP_MEMORY_LIMIT}|i" /etc/php7/php.ini && \
    sed -i "s|;*upload_max_filesize =.*|upload_max_filesize = ${MAX_UPLOAD}|i" /etc/php7/php.ini && \
    sed -i "s|;*max_file_uploads =.*|max_file_uploads = ${PHP_MAX_FILE_UPLOAD}|i" /etc/php7/php.ini && \
    sed -i "s|;*post_max_size =.*|post_max_size = ${PHP_MAX_POST}|i" /etc/php7/php.ini && \
    sed -i "s|;*cgi.fix_pathinfo=.*|cgi.fix_pathinfo= 0|i" /etc/php7/php.ini && \

    # Symlink php7 to php
    ln -s /usr/bin/php7 /usr/bin/php && \

    # Setup composer, original code from https://getcomposer.org/download/
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r "if (hash_file('SHA384', 'composer-setup.php') === 'e115a8dc7871f15d853148a7fbac7da27d6c0030b848d9b3dc09e2a0388afed865e6a3d6b3c0fad45c48e2b5fc1196ae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer --quiet && \
    rm composer-setup.php && \

    # Cleaning up
	mkdir /www && \
	apk del tzdata && \
	rm -rf /var/cache/apk/*

# Copy PHP configurations for opcache and xdebug
COPY conf.d/* /etc/php7/conf.d/

# Set Workdir
WORKDIR /www

# Expose volumes
VOLUME ["/www"]

# Expose ports
EXPOSE 9000 9009

# Entry point
ENTRYPOINT ["/usr/sbin/php-fpm7"]
