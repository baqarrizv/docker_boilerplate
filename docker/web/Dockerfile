FROM php:8.1.0-apache

WORKDIR /var/www/html

# Set non-interactive mode
ENV DEBIAN_FRONTEND=noninteractive

# Update and upgrade system packages
RUN apt-get -y update --fix-missing --no-install-recommends \
    && apt-get -y upgrade

# Install essential utilities and libraries
RUN apt-get -yq install apt-utils nano wget dialog lsb-release

# Install development libraries
RUN apt-get update && \
    apt-get install -y libsqlite3-dev libsqlite3-0 libcurl4-openssl-dev libonig-dev libpng-dev libjpeg-dev libfreetype6-dev


# Update package lists again
RUN apt-get update

RUN docker-php-ext-install pdo_mysql 
RUN docker-php-ext-install pdo_sqlite
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install curl

RUN apt-get -y install libzip-dev
RUN docker-php-ext-install zip

RUN apt-get -y install libicu-dev
RUN docker-php-ext-install -j$(nproc) intl
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install gettext
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install -j$(nproc) gd

#install wkhtmptopdf
RUN apt-get install -y fontconfig libxrender1 xfonts-75dpi \
    && apt --fix-broken install -y xvfb

RUN wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.stretch_amd64.deb
RUN dpkg -i wkhtmltox_0.12.5-1.stretch_amd64.deb
RUN apt-get -f install -y

#Apache Config
RUN a2enmod rewrite
COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

