FROM php:7.3.16-fpm-stretch

MAINTAINER Nicolas BONNIOT <nicolas@devgiants.fr>

ARG UID
ARG TIMEZONE
ARG PROJECT_NAME
ARG APPLICATION_WEB_PORT


RUN apt-get update && apt-get install -y \
    openssl \
    git \
    # Needed for composer
    unzip \
    nano \
    wget \
    curl \
    gnupg \
    apt-transport-https \
    zlib1g-dev \
    libzip-dev \
    libxml2-dev \
    g++\
    vim \
    net-tools \
    procps \
    htop

#    software-properties-common

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
&& printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini \
&& "date"

RUN pecl install xdebug-2.9.4


# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_mysql zip xml
RUN docker-php-ext-enable xdebug


#COPY ./docker/images/php-fpm7.3/config/usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Composer install
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

RUN wget https://get.symfony.com/cli/installer -O - | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony

WORKDIR /var/www/html

#RUN php-fpm -D

#RUN chmod -R 777 /usr/local/share/ca-certificates

EXPOSE 9000

# Prepare certificates handling
RUN usermod -a -G staff www-data
RUN chown root:www-data /etc/ssl/certs
RUN chmod 775 /etc/ssl/certs
RUN chown -R www-data:www-data /var/www


#USER www-data
#RUN usermod -u ${UID} www-data
#RUN symfony server:ca:install
#RUN symfony local:server:start -d

#RUN service php7.1-fpm start
#
#CMD php-fpm7.1 -F

