FROM php:8.4-apache

# 1. Cài đặt thư viện hệ thống
RUN apt-get update && apt-get install -y \
    libzip-dev zip unzip libpng-dev libffi-dev libssl-dev patch curl \
    libxml2-dev libonig-dev libgmp-dev \
    && docker-php-ext-install bcmath gd ffi zip gmp mbstring xml iconv

# 2. Tải và cấu hình browscap.ini
RUN mkdir -p /usr/local/etc/ && \
    curl -o /usr/local/etc/browscap.ini http://browscap.org/stream?q=Lite_PHP_BrowsCapINI && \
    echo "browscap=/usr/local/etc/browscap.ini" > /usr/local/etc/php/conf.d/browscap.ini

# 3. Ép Apache chạy cổng của Render
RUN sed -i 's/80/${PORT}/g' /etc/apache2/sites-available/000-default.conf /etc/apache2/ports.conf

# 4. QUAN TRỌNG: Thiết lập thư mục làm việc và copy code
WORKDIR /var/www/html
# Copy toàn bộ code từ thư mục chứa Dockerfile vào máy chủ
COPY . .

# 5. Phân quyền để Apache có thể đọc file
RUN chown -R www-data:www-data /var/www/html && chmod -R 755 /var/www/html

# 6. Kích hoạt mod_rewrite
RUN a2enmod rewrite

EXPOSE 80
