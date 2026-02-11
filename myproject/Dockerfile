FROM php:8.2-fpm

# Instalar dependencias del sistema
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    unzip \
    sqlite3 \
    libsqlite3-dev \
    && rm -rf /var/lib/apt/lists/*

# Instalar extensiones PHP necesarias
RUN docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install -j$(nproc) \
    pdo_mysql \
    pdo_sqlite \
    gd \
    mbstring \
    xml \
    zip \
    bcmath opcache

# Instalar Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Establecer directorio de trabajo
WORKDIR /app

# Copiar archivos del proyecto
COPY . /app/

# Instalar dependencias de PHP
RUN composer install --optimize-autoloader --no-dev

# Permisos para Laravel
RUN chown -R www-data:www-data /app && \
    chmod -R 755 /app && \
    chmod -R 777 /app/storage /app/bootstrap/cache

# Exponer puerto
EXPOSE 9000

# Usuario www-data
USER www-data

CMD ["php-fpm"]
