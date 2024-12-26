FROM php:7.3-fpm-alpine

# Install dependencies
RUN apk add --no-cache unixodbc-dev autoconf build-base supervisor git

# Install SQL Server PDO drivers
RUN apk add --no-cache --virtual .build-deps curl && \
    curl -fSL https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > /etc/apk/keys/microsoft.asc && \
    echo "https://packages.microsoft.com/alpine/edge/main" >> /etc/apk/repositories && \
    apk add --no-cache msodbcsql18 mssql-tools && \
    apk del .build-deps1-1_amd64.apk

RUN pecl install pdo_sqlsrv sqlsrv \
    && docker-php-ext-enable pdo_sqlsrv sqlsrv

# Set working directory
WORKDIR /var/www/html

# Copy existing application code
COPY . .

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Install application dependencies
RUN composer install --no-dev --optimize-autoloader

# Expose port
EXPOSE 9000
