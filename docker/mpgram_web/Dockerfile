FROM php:8.4-apache

# 1. Cài đặt thư viện hệ thống (Thêm libpng, libjpeg, libfreetype cho Captcha)
RUN apt-get update && apt-get install -y \
    libzip-dev zip unzip libpng-dev libjpeg-dev libfreetype6-dev libffi-dev libssl-dev patch curl \
    libxml2-dev libonig-dev libgmp-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install bcmath gd ffi zip gmp mbstring xml iconv

# 2. Cài đặt Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# 3. Tải browscap.ini
RUN mkdir -p /usr/local/etc/ && \
    curl -o /usr/local/etc/browscap.ini http://browscap.org/stream?q=Lite_PHP_BrowsCapINI && \
    echo "browscap=/usr/local/etc/browscap.ini" > /usr/local/etc/php/conf.d/browscap.ini

# 4. Cấu hình Port Render
RUN sed -i 's/80/${PORT}/g' /etc/apache2/sites-available/000-default.conf /etc/apache2/ports.conf

WORKDIR /var/www/html
COPY . .

# 5. Cài đặt thư viện và áp dụng Patch theo ý shinovon
RUN composer install --no-dev --optimize-autoloader && \
    patch -p0 < patches/InternalDoc.php.patch && \
    patch -p0 < patches/Files.php.patch && \
    patch -p0 < patches/UpdateHandler.php.patch

# 6. QUAN TRỌNG: Tạo thư mục session/temp và phân quyền cực mạnh cho Captcha
RUN mkdir -p s sessions tmp && chown -R www-data:www-data /var/www/html && chmod -R 777 /var/www/html/s /var/www/html/tmp

RUN a2enmod rewrite
EXPOSE 80
