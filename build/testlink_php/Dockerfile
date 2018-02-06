FROM alpine:edge

ENV TIMEZONE UTC
ENV PHP_MEMORY_LIMIT 512M
ENV MAX_UPLOAD 50M
ENV PHP_MAX_FILE_UPLOAD 200
ENV PHP_MAX_POST 100M

RUN apk update && apk upgrade && \
    apk add --update tzdata ssmtp && \
    cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
    echo "${TIMEZONE}" > /etc/timezone && \
    apk add --update \
        php5-mcrypt \
        php5-soap \
        php5-openssl \
        php5-gmp \
        php5-pdo_odbc \
        php5-json \
        php5-dom \
        php5-pdo \
        php5-zip \
        php5-mysql \
        php5-sqlite3 \
        php5-apcu \
        php5-pdo_pgsql \
        php5-bcmath \
        php5-gd \
        php5-odbc \
        php5-pdo_mysql \
        php5-pdo_sqlite \
        php5-pgsql \
        php5-gettext \
        php5-xmlreader \
        php5-xmlrpc \
        php5-ldap \
        php5-bz2 \
        php5-mssql \
        php5-iconv \
        php5-pdo_dblib \
        php5-curl \
        php5-ctype \
        php5-fpm

RUN ln -s /etc/php5 /etc/php && \
    sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php/php-fpm.conf && \
    sed -i -e "s/listen\s*=\s*127.0.0.1:9000/listen = 9000/g" /etc/php/php-fpm.conf && \
    sed -i "s|;date.timezone =.*|date.timezone = ${TIMEZONE}|" /etc/php/php.ini && \
    sed -i "s|memory_limit =.*|memory_limit = ${PHP_MEMORY_LIMIT}|" /etc/php/php.ini && \
    sed -i "s|upload_max_filesize =.*|upload_max_filesize = ${MAX_UPLOAD}|" /etc/php/php.ini && \
    sed -i "s|max_file_uploads =.*|max_file_uploads = ${PHP_MAX_FILE_UPLOAD}|" /etc/php/php.ini && \
    sed -i "s|post_max_size =.*|max_file_uploads = ${PHP_MAX_POST}|" /etc/php/php.ini && \
    sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php/php.ini && \
    echo "FromLineOverride=YES" >> /etc/ssmtp/ssmtp.conf && \
    echo 'sendmail_path = "/usr/sbin/ssmtp -t"' >> /etc/php/php.ini && \
    apk del tzdata && \
    rm -rf /var/cache/apk/*

# Expose ports
EXPOSE 9000

# Entry point
ENTRYPOINT ["/usr/bin/php-fpm5"]

