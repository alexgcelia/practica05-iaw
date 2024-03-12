# practica05.2-iaw
Este repositorio es para la práctica 5.2 de docker-compose Wordpress del módulo IAW

## Trabajaremos con un archivo docker-compose.yml
### Este es el archivo con la instalación automatizada de nuestro bitnami/wordpress
#### Aqui encontramos la instalación de Wordpress, como podemos comprobar en ella irán las variables que escribirmeos en nuestro archivo .env
```
version: '3'

services:
  wordpress:
    image: bitnami/wordpress:6.4.3
    environment: 
      - WORDPRESS_DB_HOST=mysql
      - WORDPRESS_DB_NAME=${WORDPRESS_DB_NAME}
      - WORDPRESS_DB_USER=${WORDPRESS_DB_USER}
      - WORDPRESS_DB_PASSWORD=${WORDPRESS_DB_PASSWORD}
      - WORDPRESS_BLOG_NAME=${WORDPRESS_BLOG_NAME}
      - WORDPRESS_USERNAME=${WORDPRESS_USERNAME}
      - WORDPRESS_PASSWORD=${WORDPRESS_PASSWORD}
      - WORDPRESS_EMAIL=$(WORDPRESS_EMAIL)
    volumes: 
      - wordpress_data:/bitnami/wordpress
    depends_on:
      - mysql
    restart: always
    networks:
      - frontend-network
      - backend-network
```

#### Aqui encontramos la instalación de MySQL
```
  mysql:
    image: mysql:8.0
    ports:
      - 3306:3306
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_DATABASE=${WORDPRESS_DB_NAME}
      - MYSQL_USER=${WORDPRESS_DB_USER}
      - MYSQL_PASSWORD=${WORDPRESS_DB_PASSWORD}
    volumes:
      - mysql_data:/var/lib/mysql
    restart: always
    networks: 
      - backend-network
```

#### Aqui encontramos la instalación de phpMyAdmin
```        
  phpmyadmin:
    image: phpmyadmin
    ports:
      - 8080:80
    environment: 
      - PMA_HOST=mysql
    restart: always
    networks:
      - frontend-network
      - backend-network
```

#### Aquí encontramos la instalación de https en wordpress (let's encrypt)
```
  https-portal:
    image: steveltn/https-portal:1
    ports:
      - 80:80
      - 443:443
    restart: always
    environment:
      DOMAINS: '${DNS_DOMAIN_SECURE} -> http://wordpress:80'
      STAGE: 'production'
    networks:
      - frontend-network
```

#### Aquí encontramos los volúmenes y redes creadas del fichero
```   
volumes: 
  mysql_data:
  wordpress_data:

networks: 
  backend-network:
  frontend-network:
```

## También trabajremos con un archivo enviroment (.env)
### Este contiene información sobre la instalación automatizada
#### Aquí escribimos nuestro dominio anteriormente creado en NoIP
```
DNS_DOMAIN_SECURE=wordpress-docker.ddns.net
```
#### Aquí encontramos las variables de MySQL
```
WORDPRESS_DB_HOST=mysql
WORDPRESS_DB_NAME=wordpress 
WORDPRESS_DB_USER=alexg
WORDPRESS_DB_PASSWORD=1234567

MYSQL_ROOT_PASSWORD=root
```

#### Aquí escribiremos las variables para nuestro Prestashop
```
WORDPRESS_BLOG_NAME="Sitio web de Alejandro Giménez"
WORDPRESS_USERNAME=admin
WORDPRESS_PASSWORD=admin
WORDPRESS_EMAIL=alexg@iaw.com
```
