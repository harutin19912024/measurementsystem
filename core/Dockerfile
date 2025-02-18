# Use PHP with FPM
FROM php:8.1-fpm

# Set working directory inside the container
WORKDIR /core

# Install system dependencies
RUN apt-get update && apt-get install -y \
    zip unzip curl git libpng-dev libjpeg-dev libfreetype6-dev libonig-dev \
    && docker-php-ext-install pdo pdo_mysql mbstring exif pcntl bcmath gd \
    && rm -rf /var/lib/apt/lists/*

# Install Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Copy the composer.json and composer.lock first to leverage Docker's cache
COPY composer.json composer.lock ./

# Install PHP dependencies
RUN composer install --no-dev --optimize-autoloader

# Now copy the rest of the application code
COPY . .

# Set correct permissions
RUN chown -R www-data:www-data /app

# Expose port for PHP-FPM (if needed for communication with Nginx)
EXPOSE 9000

# Start PHP-FPM
CMD ["php-fpm"]
