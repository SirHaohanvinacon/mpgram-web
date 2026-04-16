FROM php:8.4-apache

# Cài đầy đủ các thư viện từ file gốc của bạn
RUN apt-get update && apt-get install -y \
    libzip-dev zip unzip libpng-dev libffi-dev libssl-dev patch curl \
    libxml2-dev libonig-dev libgmp-dev \
    && docker-php-ext-install bcmath gd ffi zip gmp mbstring xml iconv

# Lệnh "thần thánh" để Render nhận diện được cổng
RUN sed -i 's/80/${PORT}/g' /etc/apache2/sites-available/000-default.conf /etc/apache2/ports.conf

WORKDIR /var/www/html
COPY . .

# Kích hoạt chế độ rewrite cho link đẹp
RUN a2enmod rewrite

EXPOSE 80
