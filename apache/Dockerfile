# Usa la imagen base de PHP con Apache
FROM php:8.0-apache

# Copia el archivo de configuración de Apache para http y https
COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

# Copia el entrypoint y asegúrate de que es ejecutable
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Habilita los módulos SSL, rewrite y headers en Apache
RUN a2enmod ssl rewrite headers

# Instala dependencias adicionales para PHP
RUN apt-get update && \
    apt-get install -y libpng-dev libjpeg-dev libfreetype6-dev && \
    docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install -j$(nproc) gd mysqli

# Activa el sitio SSL
RUN a2ensite 000-default.conf

# Configura el entrypoint y expone los puertos HTTP y HTTPS
ENTRYPOINT ["/entrypoint.sh"]
EXPOSE 80 443
