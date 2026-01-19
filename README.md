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
