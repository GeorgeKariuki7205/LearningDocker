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
        php-cli\
        unzip
        
RUN docker-php-ext-install pdo pdo_mysql mbstring zip exif pcntl php7.4-common php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-curl php7.4-gd php7.4-imagick php7.4-cli php7.4-dev php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip php7.4-intl
# these directories need to be writable by Apache
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache

# copy env file for our Docker image
COPY .env /var/www/html/.env

RUN php artisan config:cache

# only if you do NOT use anonymous functions in your routes:
RUN php artisan route:cache

### Docker image metadata

VOLUME ["/var/www/html/storage", "/var/www/html/bootstrap/cache"]

