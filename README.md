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
> 
> Solución: 
>```bash
> servive apache2 start
> service mariadb start
>```

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
### 3.3. Configurar Apache
```bash
//Crea una página
nano etc/apache2/sites-available/wordpress.conf
```
>[!WARNING]
> Para utilizar el comando nano es necesario instalarlo
> ```bash
> apt install nano
>```

Que contenga las siguientes líneas: 
```bash
<VirtualHost *:80>
    DocumentRoot /srv/www/wordpress
    <Directory /srv/www/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Require all granted
    </Directory>
    <Directory /srv/www/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>
```
### 3.4. Habilitar el sitio
```bash
// Habilitar el sitio de WP
a2ensite wordpress
// Habilitar el módulo de reescritura
a2enmod rewrite
// Deshabilitar el sitio predeterminado
a2dissite 000-default
// Recargar Apache para aplicar los cambios
service apache2 reload
```

### 3.5 Comprobar acceso a WordPress
```bash
http://<ip>:<puerto>/wp-admin/setup-config.php
```

### 3.6 Configurar
```bash
mysql -u root

# Crea una nueva base de datos
create database <nombre_BD>;

# Crea un nuevo usuario y aplica una contraseña
create user <nombre_usuario>@localhost identified by '<contraseña>';

# Otorga todos los privilegios al usuario especificado
grant all privileges on <nombre_BD>.* TO <nombre_usuario>@localhost;

# Actualiza los privilegios
fush privileges;

exit;

# Copia el archivo de configuración
cp /srv/www/wordpress/wp-config-sample.php /srv/www/wordpress/wp-config.php

# Reemplaza el nombre de la base de datos en el archivo de configuración
sed -i 's/database_name_here/<nombre_BD>/' /srv/www/wordpress/wp-config.php

# Reemplaza el nombre del usuario en el archivo de configuración
sudo -u www-data sed -i 's/username_here/<nombre_usuario>/' /srv/www/wordpress/wp-config.php

# Reemplaza la contraseña del usuario en el archivo de configuración
sed -i 's/password_here/<contraseña>/' /srv/www/wordpress/wp-config.php
```



