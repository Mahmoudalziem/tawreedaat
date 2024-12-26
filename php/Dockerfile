FROM php:7.3-fpm

RUN apt-get update && apt-get install -y \
    curl gnupg apt-transport-https && \
    curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
    curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
    apt-get update && ACCEPT_EULA=Y apt-get install -y \
    msodbcsql18 mssql-tools && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install pdo pdo_mysql


# Set working directory
WORKDIR /var/www/html

# Copy existing application code
COPY . .

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Install application dependencies
# RUN composer install --no-dev --optimize-autoloader

# Expose port
EXPOSE 9000
