FROM composer:2.0 as vendor

COPY database/ database/
COPY composer.json composer.json
COPY composer.lock composer.lock
RUN composer install \
    --ignore-platform-reqs \
    --no-interaction \
    --no-plugins \
    --no-scripts \
    --prefer-dist


#
# Frontend
#
FROM node:14.9 as frontend
RUN mkdir -p /app/public
COPY package.json webpack.mix.js yarn.lock /app/
COPY resources/ /app/resources/
WORKDIR /app
RUN yarn install && yarn production


# #
# # Application
# #
FROM php:7.4-fpm

WORKDIR /var/www/html/

# Install PHP dependencies
RUN apt-get update -y && apt-get install -y libzip-dev zip unzip libpng-dev libjpeg-dev zlib1g-dev procps vim
RUN docker-php-ext-install pdo pdo_mysql zip exif gd

# Copy Composer dependencies
COPY . /var/www/html
COPY --from=vendor /app/vendor/ /var/www/html/vendor/
COPY --from=frontend /app/public/js/ /var/www/html/public/js/
COPY --from=frontend /app/public/css/ /var/www/html/public/css/
COPY --from=frontend /app/mix-manifest.json /var/www/html/mix-manifest.json

RUN php artisan config:cache
RUN php artisan route:cache


