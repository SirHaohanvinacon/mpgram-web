FROM php:8.4-apache

# 1. Cài đặt các thư viện hệ thống cần thiết
RUN apt-get update && apt-get install -y \
    libzip-dev zip unzip libpng-dev libffi-dev libssl-dev patch curl \
    libxml2-dev libonig-dev libgmp-dev \
    && docker-php-ext-install bcmath gd ffi zip gmp mbstring xml iconv

# 2. Cài đặt Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# 3. Tải browscap.ini cho điện thoại cổ
RUN mkdir -p /usr/local/etc/ && \
    curl -o /usr/local/etc/browscap.ini http://browscap.org/stream?q=Lite_PHP_BrowsCapINI && \
    echo "browscap=/usr/local/etc/browscap.ini" > /usr/local/etc/php/conf.d/browscap.ini

# 4. Cấu hình cổng cho Render
RUN sed -i 's/80/${PORT}/g' /etc/apache2/sites-available/000-default.conf /etc/apache2/ports.conf

WORKDIR /var/www/html
COPY . .

# 5. Chạy Composer để cài thư viện
RUN composer install --no-dev --optimize-autoloader

# 6. ÁP DỤNG CÁC BẢN VÁ (PATCHES) - Theo lời nhắc của shinovon
RUN patch -p0 < patches/InternalDoc.php.patch && \
    patch -p0 < patches/Files.php.patch && \
    patch -p0 < patches/UpdateHandler.php.patch

# 7. Phân quyền và kích hoạt mod_rewrite
RUN chown -R www-data:www-data /var/www/html && a2enmod rewrite

EXPOSE 80
