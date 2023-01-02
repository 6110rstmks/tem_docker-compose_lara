FROM php:7.3-apache

COPY ./000-default.conf /etc/apache2/sites-available/000-default.conf

COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer

RUN apt-get update  \
    && apt-get -y install \
    git \
    zip \
    unzip \
    vim \
    && docker-php-ext-install pdo_mysql bcmath mbstring \
    && a2enmod rewrite
    
WORKDIR /var/www/html

EXPOSE 80

# We need a user with the same UID/GID as the host user
# so when we execute CLI commands, all the host file's permissions and ownership remain intact.
# Otherwise commands from inside the container would create root-owned files and directories.
ARG uid
RUN useradd -G www-data,root -u $uid -d /home/devuser devuser
RUN mkdir -p /home/devuser/.composer && \
    chown -R devuser:devuser /home/devuser
