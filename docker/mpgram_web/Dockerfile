FROM php:8.4-apache

# 1. Cài đặt các thư viện hệ thống
RUN apt-get update && apt-get install -y \
    libzip-dev zip unzip libpng-dev libffi-dev libssl-dev patch curl \
    libxml2-dev libonig-dev libgmp-dev \
    && docker-php-ext-install bcmath gd ffi zip gmp mbstring xml iconv

# 2. Tải file browscap.ini mà hệ thống đang đòi
RUN mkdir -p /usr/local/etc/ && \
    curl -o /usr/local/etc/browscap.ini http://browscap.org/stream?q=Lite_PHP_BrowsCapINI

# 3. Cấu hình PHP để nhận file vừa tải
RUN echo "browscap=/usr/local/etc/browscap.ini" > /usr/local/etc/php/conf.d/browscap.ini

# 4. Ép Apache chạy cổng của Render
RUN sed -i 's/80/${PORT}/g' /etc/apache2/sites-available/000-default.conf /etc/apache2/ports.conf

WORKDIR /var/www/html
COPY . .

# 5. Kích hoạt mod_rewrite cho link web hoạt động
RUN a2enmod rewrite

EXPOSE 80
