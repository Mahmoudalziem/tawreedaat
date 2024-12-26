FROM php:7.3-fpm-alpine

# Install dependencies
RUN apk add --no-cache unixodbc-dev autoconf build-base supervisor git

# Install SQL Server PDO drivers
RUN curl -O https://download.microsoft.com/download/e/1/4/e14df6c7-44e6-4b74-9b87-55bc36d92c62/msodbcsql18_18.2.1.1-1_amd64.apk && \
    curl -O https://download.microsoft.com/download/e/1/4/e14df6c7-44e6-4b74-9b87-55bc36d92c62/mssql-tools18_18.2.1.1-1_amd64.apk && \
    apk add --allow-untrusted mssql-tools18_18.2.1.1-1_amd64.apk && \
    rm msodbcsql18_18.2.1.1-1_amd64.apk mssql-tools18_18.2.1.1-1_amd64.apk

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
