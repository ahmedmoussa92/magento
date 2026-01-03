# Use official PHP image with Apache
FROM php:8.2-apache

# Set working directory
WORKDIR /var/www/html

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libicu-dev \
    libxml2-dev \
    libxslt1-dev \
    zip unzip git curl wget libonig-dev \
    && docker-php-ext-install -j$(nproc) intl pdo_mysql mbstring soap xsl gd zip opcache bcmath \
    && docker-php-ext-configure gd --with-freetype --with-jpeg

# Enable Apache rewrite module
RUN a2enmod rewrite

# Install Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Copy Magento project files (adjust if your Magento root is different)
COPY . /var/www/html

# Set file permissions (adjust user if needed)
RUN chown -R www-data:www-data /var/www/html \
    && find var generated pub/static pub/media app/etc -type f -exec chmod 644 {} \; \
    && find var generated pub/static pub/media app/etc -type d -exec chmod 755 {} \;

# Expose Apache port
EXPOSE 80

# Start Apache in foreground
CMD ["apache2-foreground"]
