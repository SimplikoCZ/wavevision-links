FROM php:8.5-cli

# Install system dependencies and PHP extensions required by the project
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    libzip-dev \
    zlib1g-dev \
    libicu-dev \
    curl && \
    docker-php-ext-install zip intl && \
    pecl install xdebug && docker-php-ext-enable xdebug && \
    echo "xdebug.mode=coverage" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /app

# Copy composer files and install dependencies (including dev for tests)
COPY composer.json composer.lock /app/
RUN composer install --no-interaction --prefer-dist

# Copy the rest of the source code
COPY . .

# Default command runs the test suite
CMD ["vendor/bin/phpunit"]
