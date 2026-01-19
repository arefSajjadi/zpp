ersion: '3.5'

services:
  mysql:
    image: mysql:5.7.21
    container_name: mysql
    volumes:
      - ./data/mysql/conf.d:/etc/mysql/conf.d/
      - ./my-db:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_DATABASE=ziarat
      - MYSQL_USER=root
      - MYSQL_PASSWORD=${DB_PASSWORD}
    networks:
      - mysql_network

  phpmyadmin:
    container_name: phpmyadmin
    image: phpmyadmin/phpmyadmin
    networks:
      - mysql_network
    environment:
      PMA_HOST: mysql
      PMA_PORT: 3306
    restart: always
    ports:
      - "127.0.0.1:6061:80"

  redis:
    container_name: redis
    image: redis:5-alpine
    restart: always
    networks:
      - mysql_network


networks:
    mysql_network:
        driver: bridge
