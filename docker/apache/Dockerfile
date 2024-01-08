# Author : Abdullahil Kafi
FROM httpd:2.4-alpine

ENV TZ=${TZ:-UTC}
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

FROM php:8.2.12-apache

# Install PHP extensions and other dependencies
RUN apt-get update && apt-get upgrade -y

# Install PHP extensions and other dependencies
RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y libzip-dev libicu-dev \
    libpng-dev libjpeg-dev libfreetype6-dev \
    curl unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install mysqli pdo_mysql zip intl gd exif opcache \
    && a2enmod rewrite

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer


RUN a2enmod ssl && a2enmod socache_shmcb    

COPY ./ssl/mycert.crt /etc/ssl/certs/mycert.crt
COPY ./ssl/mycert.key /etc/ssl/private/mycert.key

# Update default-ssl.conf to point to the certificate and key
RUN sed -i '/SSLCertificateFile.*snakeoil\.pem/c\SSLCertificateFile /etc/ssl/certs/mycert.crt' /etc/apache2/sites-available/default-ssl.conf && \
    sed -i '/SSLCertificateKeyFile.*snakeoil\.key/cSSLCertificateKeyFile /etc/ssl/private/mycert.key\' /etc/apache2/sites-available/default-ssl.conf

# Enable the SSL-enabled site
RUN a2ensite default-ssl

# Update Apache configurations for localhost
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

ENV APP_ROOT=/var/www
ENV SERVER_NAME=localhost
ENV DOCUMENT_ROOT=${APP_ROOT}/html/doorbird.com
ENV APACHE_LOG_DIR=${APP_ROOT}/docker/apache/logs
ENV APACHE_RUN_GROUP=www-data
ENV APACHE_RUN_USER=www-data

# RUN apk add --update --no-cache tzdata

WORKDIR ${APP_ROOT}

RUN mkdir -p ${DOCUMENT_ROOT}
RUN chown -R ${APACHE_RUN_USER}:${APACHE_RUN_USER} ${DOCUMENT_ROOT}

RUN ln -s ${APP_ROOT}/html/doorbird.com/ ${DOCUMENT_ROOT}

COPY ./httpd.conf /usr/local/apache2/conf/httpd.conf

# Expose ports
EXPOSE 80
EXPOSE 443


