FROM php:7.3.8-fpm-alpine

RUN apk upgrade --update

# lib dependencies
RUN apk add --no-cache git \
    jpeg-dev \
    libpng-dev \
    freetype-dev \
    libxslt-dev \
    icu-dev \
    libzip-dev \
    libjpeg-turbo-dev \
    $PHPIZE_DEPS

# install extensions
# RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-png --with-gd \
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
    && docker-php-ext-install gd bcmath xsl intl pdo pdo_mysql soap zip opcache

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install PHPCPD
RUN curl -sSL https://phar.phpunit.de/phpcpd.phar -o phpcpd.phar
RUN chmod a+x phpcpd.phar
RUN mv phpcpd.phar /usr/local/bin/phpcpd

# Install PHPMD
RUN curl -sSL https://phpmd.org/static/latest/phpmd.phar -o phpmd.phar
RUN chmod a+x phpmd.phar
RUN mv phpmd.phar /usr/local/bin/phpmd

# Install PHPCS
RUN curl -sSL https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar -o phpcs.phar
RUN chmod a+x phpcs.phar
RUN mv phpcs.phar /usr/local/bin/phpcs

WORKDIR /var/www

# Expose port 9000 and start php-fpm server
EXPOSE 9000

ENTRYPOINT ["/bin/sh", "/home/scripts/startup.sh"]
