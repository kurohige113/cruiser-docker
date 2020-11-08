FROM php:7.3.20-apache-stretch

RUN ["apt-get", "update"]
RUN apt-get install -y libzip-dev zip && docker-php-ext-install zip

# install vim
RUN ["apt-get", "install", "-y", "vim"]

WORKDIR /var/www/leichttraktor