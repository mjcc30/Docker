
FROM php:7.4-apache-buster
ARG DEBIAN_FRONTEND=noninteractive
# Update
RUN apt-get -y update --fix-missing && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*
# Utils
RUN apt-get -y update && apt-get -y --no-install-recommends install apt-utils locales nano wget dialog libsqlite3-dev libsqlite3-0 default-mysql-client zlib1g-dev libzip-dev libicu-dev &&     apt-get -y --no-install-recommends install --fix-missing apt-utils build-essential git curl libonig-dev && \ 
    apt-get -y --no-install-recommends install --fix-missing libcurl4 libcurl4-openssl-dev zip unzip openssl &&     rm -rf /var/lib/apt/lists/* &&     curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
# locales
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen &&     echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen &&     locale-gen
# Install xdebug
RUN pecl install xdebug-2.8.0 &&     docker-php-ext-enable xdebug
# Other PHP7 Extensions
RUN docker-php-ext-install pdo_mysql pdo_sqlite mysqli curl tokenizer json zip -j4 intl mbstring gettext
# Enable apache modules
RUN a2enmod rewrite headers
# Cleanup
RUN rm -rf /usr/src/*
WORKDIR /var/www/html
RUN echo " <?php phpinfo(); ?> " > index.php

