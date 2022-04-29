#FROM php:7.4-fpm
#FROM php:8.1-fpm
FROM library/php:8.1.4-fpm

# Copy composer.lock and composer.json into the working directory
COPY composer.lock composer.json /var/www/html/

# Set working directory
WORKDIR /var/www/html/

# Install dependencies for the operating system software
RUN apt-get update && apt-get install -y

RUN docker-php-ext-install mysqli pdo pdo_mysql

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install composer (php package manager)
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copy existing application directory contents to the working directory
COPY . /var/www/html

# Expose port 9000 and start php-fpm server (for FastCGI Process Manager)
EXPOSE 9000

CMD /bin/bash -c " \
          composer install \
          && composer dump-autoload --optimize \
          && php artisan migrate \
          && php-fpm"
