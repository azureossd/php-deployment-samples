FROM composer:2.5.8 as build

COPY . /app/
RUN composer install --prefer-dist --no-dev --optimize-autoloader --no-interaction

FROM php:8.2-apache-bullseye as production

COPY --from=build /app /var/www/html
COPY apache/000-default.conf /etc/apache2/sites-available/000-default.conf
COPY apache/mpm_prefork.conf /etc/apache2/mods-available/mpm_prefork.conf

# Change permissions to read/write for storage or else it'll fail to write out logs due to lack of permissions
# Change ownership to www-data for Apache
RUN chmod 777 -R /var/www/html/storage/ && \
    chown -R www-data:www-data /var/www/ && \
    a2enmod rewrite && \
    service apache2 restart

EXPOSE 80