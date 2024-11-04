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

