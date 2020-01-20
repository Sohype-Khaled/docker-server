ARG PHP_FPM_VERSION

FROM php:${PHP_FPM_VERSION}-fpm

# Set Environment Variables
ENV DEBIAN_FRONTEND noninteractive

RUN set -xe; \
    apt-get update -yqq && apt-get install -yqq \
    apt-utils \
    libzip-dev \
    zip \
    libxslt-dev \
    unzip && \
    docker-php-ext-configure zip --with-libzip && \
    docker-php-ext-install pdo_mysql zip xsl pcntl exif && \
    curl --silent --show-error https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

#
#--------------------------------------------------------------------------
# Final Touch
#--------------------------------------------------------------------------
#

COPY ./laravel.ini /usr/local/etc/php/conf.d
COPY ./xlaravel.pool.conf /usr/local/etc/php-fpm.d/

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog && \
    usermod -u 1000 www-data

ADD ./startup.sh /opt/startup.sh

RUN sed -i 's/\r//g' /opt/startup.sh
CMD ["/bin/bash","/opt/startup.sh"]

EXPOSE 9000