# Menggunakan base image php yang sudah tersedia di Docker Hub
FROM php:8.1-apache

# Menyalin semua file dari direktori proyek ke direktori kerja di dalam container
COPY . /var/www/html

# Menginstall dependensi yang diperlukan untuk Laravel
RUN apt-get update && apt-get install -y \
    libzip-dev \
    unzip \
    && docker-php-ext-install zip

# Mengaktifkan mod_rewrite di Apache
RUN a2enmod rewrite

# Mengatur kepemilikan file dan folder agar dimiliki oleh pengguna www-data (user Apache)
RUN chown -R www-data:www-data /var/www/html/storage

# Menyalin file .env.example dan mengubah namanya menjadi .env
RUN cp .env.example .env

# Menginstall dependencies Laravel menggunakan Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer install --no-dev --optimize-autoloader

# Menjalankan migrasi database dan menjalankan perintah key:generate
RUN php artisan migrate --force && php artisan key:generate

# Menjalankan perintah serve untuk menjalankan server Laravel
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]
