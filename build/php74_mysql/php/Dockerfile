FROM php:7.4-fpm

# install php-ext
RUN apt-get update && apt-get install -y git wget gnupg vim unzip libxml2-dev libpng-dev libzip-dev \
  && docker-php-ext-install mbstring dom gd zip \
  && apt-get clean


RUN apt-get install -y default-mysql-client && docker-php-ext-install pdo_mysql


# install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer


# install exment
RUN mkdir /var/www/exment
WORKDIR /var/www/exment
RUN COMPOSER_MEMORY_LIMIT=-1 composer create-project "laravel/laravel=5.6.*" . \
  && COMPOSER_MEMORY_LIMIT=-1 composer require exceedone/exment
RUN php artisan vendor:publish --provider="Exceedone\\Exment\\ExmentServiceProvider"
RUN chown -R www-data:www-data /var/www/exment \
  && chmod 755 -R /var/www/exment/storage \
  && chmod 755 -R /var/www/exment/bootstrap/cache

# remove env file. because use volumes env file
RUN rm /var/www/exment/.env

