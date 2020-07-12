FROM  php:7.4.7-fpm-alpine3.12
LABEL maintainer="Pedro Rodrigues <pir.pedro@gmail.com>"

RUN apk add --no-cache --virtual \
    openssl bash mysql-client nodejs npm sudo libzip-dev

#ext-zip is necessary only for laravel installation
RUN docker-php-ext-install pdo pdo_mysql bcmath zip\ 
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin \
    && ln -s /usr/local/bin/composer.phar /usr/local/bin/composer

RUN addgroup -g 1000 -S app && adduser -u 1000 -S app -G app -s /bin/bash \
    && adduser app wheel \
    && sed -e 's;^# \(%wheel.*NOPASSWD.*\);\1;g' -i /etc/sudoers \
    && mkdir -p /usr/src/app \
    && chown -R app:app /usr/src \
    && chmod -R 777 /usr/src/app


WORKDIR /var/www
RUN rm -rf html \
    && ln -s /usr/src/app/public html \
    && chown -R www-data:www-data /var/www/*

USER app
WORKDIR /usr/src/app
RUN composer global require laravel/installer \
    && echo "export PATH=\$HOME/.composer/vendor/bin:\$PATH" > $HOME/.bashrc

EXPOSE 9000
ENTRYPOINT ["php-fpm"]