FROM php:7.4-apache

### PHP

# we may need some other php modules, but we can first check the enabled modules with
# docker run -it --rm php:7.4-apache php -m
# RUN docker-php-ext-install mbstring 

### Apache

# change the document root to /var/www/html/public
RUN sed -i -e "s/html/html\/public/g" /etc/apache2/sites-enabled/000-default.conf

# enable apache mod_rewrite
RUN a2enmod rewrite

### Laravel application

# copy source files
COPY ./dockerisingLaravelApplication /var/www/html 
RUN ls
RUN apt-get update && apt-get install -y \
        build-essential \
        libpng-dev \
        libjpeg62-turbo-dev \
        libfreetype6-dev \
        locales \
        zip \
        jpegoptim optipng pngquant gifsicle \
        unzip \
        curl\        
        imagemagick \        
        unzip 
        # libzip-dev   

# RUN docker-php-ext-install pdo pdo_mysql zip exif pcntl  xml xmlrpc curl gd imap opcache soap zip intl
# these directories need to be writable by Apache
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache

# copy env file for our Docker image
# COPY .env /var/www/html/.env

RUN php artisan config:cache

# only if you do NOT use anonymous functions in your routes:
RUN php artisan route:cache

### Docker image metadata
COPY  entrypoint.sh /
RUN chmod u+x /entrypoint.sh 
# VOLUME ["/var/www/html/storage", "/var/www/html/bootstrap/cache"]

CMD /entypoint.sh

