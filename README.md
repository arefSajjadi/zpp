ervices:
  app:
    container_name: "api.ziarat"
    image: php-laravel:8.2
    volumes:
      - ./src:/var/www/
      - ./php.ini:/usr/local/etc/php/php.ini
    ports:
      - "127.0.0.1:6060:80"
    restart: always
    networks:
      - database_mysql_network
  staging:
    container_name: "api-staging.ziarat"
    image: php-laravel:8.2
    volumes:
      - ./src:/var/www/
      - ./php.ini:/usr/local/etc/php/php.ini
    environment:
      - APP_ENV=staging
      - DB_DATABASE=ziarat_staging
      - APP_NAME=ziarat_staging
    ports:
      - "127.0.0.1:6090:80"
    restart: always
    networks:
      - database_mysql_network


networks:
  database_mysql_network:
    external: true



FROM php:8.2-apache

MAINTAINER Aref Sajjadi <arefsajjadi80@gmail.com>

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        zlib1g-dev \
        libxml2-dev \
        libzip-dev \
        libonig-dev \
        xvfb \
        libfontconfig \
        wkhtmltopdf \
        cron \
        vim \
        graphviz \
        libssl-dev \
        libxrender1 \
        fontconfig \
        libxtst6 \
        xfonts-75dpi \
        xfonts-base \
        xz-utils \
    && docker-php-ext-configure gd \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install zip \
    && docker-php-ext-install soap \
    && docker-php-source delete

RUN ln -snf /usr/share/zoneinfo/Asia/Tehran /etc/localtime && echo Asia/Tehran > /etc/timezone && dpkg-reconfigure -f noninteractive tzdata
RUN pecl install redis && docker-php-ext-enable redis
RUN docker-php-ext-install pcntl

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php && php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

ADD apache.conf /etc/apache2/sites-enabled/000-default.conf
RUN printf "\nServerName localhost" >> /etc/apache2/apache2.conf && a2enmod rewrite && a2enmod headers

RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

WORKDIR /var/www
