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

USER root
# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

# Configure non-root user.
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}
RUN groupmod -o -g ${PGID} www-data && \
    usermod -o -u ${PUID} -g www-data www-data

<<<<<<< HEAD
WORKDIR /var/www



CMD ["php-fpm"]


=======
RUN sed -i 's/\r//g' /opt/startup.sh
CMD ["/bin/bash","/opt/startup.sh"]
RUN chown -R www-data:www-data /var/www

>>>>>>> 6f4559a31ddea3235f5b6755acb5e30801a20d0f
EXPOSE 9000
