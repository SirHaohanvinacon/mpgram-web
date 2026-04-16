FROM php:8.4-apache

# 1. Cài đặt đầy đủ thư viện
RUN apt-get update && apt-get install -y \
    libzip-dev zip unzip libpng-dev libffi-dev libssl-dev patch curl \
    libxml2-dev libonig-dev libgmp-dev \
    && docker-php-ext-install bcmath gd ffi zip gmp mbstring xml iconv

# 2. Browscap để nhận diện điện thoại cổ
RUN mkdir -p /usr/local/etc/ && \
    curl -o /usr/local/etc/browscap.ini http://browscap.org/stream?q=Lite_PHP_BrowsCapINI && \
    echo "browscap=/usr/local/etc/browscap.ini" > /usr/local/etc/php/conf.d/browscap.ini

# 3. Chỉnh cổng Render
RUN sed -i 's/80/${PORT}/g' /etc/apache2/sites-available/000-default.conf /etc/apache2/ports.conf

WORKDIR /var/www/html

# 4. Copy TOÀN BỘ file từ thư mục gốc vào (vì Context đã đổi thành '.')
COPY . .

# 5. Phân quyền và kích hoạt mod_rewrite
RUN chown -R www-data:www-data /var/www/html && a2enmod rewrite

EXPOSE 80
