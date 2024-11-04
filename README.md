# Tarea 4
Instalar Wordpress en un contenedor Docker con Ubuntu

## 1. Utiliza la imagen de  Ubuntu 22
### 1.1. Descargar la imagen de Ubuntu 22
```bash
docker pull ubuntu:22.04
```
### 1.2. Crear un contenedor con la imagen de Ubuntu 22
```bash
//Es necesario asignarle un puerto al contenedor
docker run -d -p 7000:80 --name ub ubuntu:22.04 tail -f /dev/null
```

## 2. Instalar LAMP en dicho contenedor
### 2.1. Acceder al contenedor
```bash
docker exec -it ub sh
```

### 2.2. Actualizar lista de paquetes
```bash
apt update
``` 
### 2.3. Instalar Apache
```bash
apt install -y apache2 apache2-utils
```
### 2.4. Instalar MariaDB
```bash
apt install -y mariadb-server mariadb-client
//aseguramos la instalacion
sudo mysql_secure_installation
```
>[!WARNING]
> ERROR 2002(HY000) la base de datos no está iniciada
> Solución: servive apache2 start y service mariadb start

### 2.5. Instalar PHP
```bash
apt install -y php php-mysql libapache2-mod-php
//Reiniciar el servicio de apache
systemctl restart apache2
```

## 3. Instalar Wordpress
### 3.1. Instalar dependencias
```bash
apt install ghostscript \
            php-bcmath \
            php-curl \
            php-imagick \
            php-intl \
            php-json \
            php-mbstring \
            php-mysql \
            php-xml \
            php-zip
```
### 3.2. Instalar Wordpress
```bash
//Creamos un directorio
mkdir -p /srv/www
//Cambiamos la propiedad al usuario www-data
chown www-data: /srv/www
//Descargamos Wordpress y la extraemos en /srv/www
curl https://wordpress.org/latest.tar.gz | tar -xz -C /srv/www
```
>[!WARNING]
> Para utilizar el comando curl es necesario instalarlo
> ```bash
> apt install curl
>```

