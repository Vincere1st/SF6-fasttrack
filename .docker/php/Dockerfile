FROM php:8.3-fpm

# Install dependencies
RUN apt-get update && apt-get install -y \
    libicu-dev \
    libxslt1-dev \
    libpq-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libssl-dev \
    libsodium-dev \
    libcurl4-openssl-dev \
    libzip-dev \
    libxml2-dev \
    libonig-dev \
    librabbitmq-dev \
    unzip \
    git \
    curl \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) \
    intl \
    pdo_pgsql \
    xsl \
    gd \
    opcache \
    sodium \
    zip \
    bcmath \
    && pecl install amqp redis \
    && docker-php-ext-enable amqp redis

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Install Symfony CLI
RUN curl -sS https://get.symfony.com/cli/installer | bash \
    && mv /root/.symfony*/bin/symfony /usr/local/bin/symfony

# Set permissions for the working directory
WORKDIR /var/www/sf6
RUN chown -R www-data:www-data /var/www/sf6

# Set the user to www-data
USER www-data

# Expose port 9000 and start php-fpm server
EXPOSE 8000
CMD ["php-fpm"]