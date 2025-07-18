# Docker compose file
version: '3.8'

networks:
  wp_net:
    name: wp_net
    driver_opts:
      com.docker.network.bridge.name: wp_net

volumes:
  wp_db:
    name: wp_db
  wp_wp:
    name: wp_wp

services:
  db:
    image: mysql:5.7
    container_name: mysql
    volumes:
      - wp_db:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: sdfascsdvsfdvweliuoiquowecefcwaefef
      MYSQL_DATABASE: DockerMe
      MYSQL_USER: DockerMe
      MYSQL_PASSWORD: sdfascsdvsfdvweliuoiquowecefcwaefef
    networks:
      - wp_net

  wordpress:
    image: wordpress:latest
    container_name: wordpress
    volumes:
      - wp_wp:/var/www/html/
    depends_on:
      - db
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: DockerMe
      WORDPRESS_DB_NAME: DockerMe
      WORDPRESS_DB_PASSWORD: sdfascsdvsfdvweliuoiquowecefcwaefef
    ports:
      - 8000:80
    networks:
      - wp_net

  nginx:
    image: nginx:alpine
    container_name: nginx
    restart: always
    depends_on:
      - wordpress
    volumes:
      - ./nginx/conf.d:/etc/nginx/conf.d
      - ./nginx/certs:/etc/nginx/certs
    ports:
      - 80:80
      - 443:443
    networks:
      - wp_net
