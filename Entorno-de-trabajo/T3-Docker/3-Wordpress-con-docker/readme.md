# Un Wordpress simple con Docker
## Realizado por: José García Torrescusa.
_Paso a paso de la instalación de Wordpress en Docker, conectado a un contenedor MariaDB entre sí._
---
1. Instalamos docker en nuestra VM.
```
sudo apt update;
sudo apt install docker.io -y;
sudo systemctl enable docker;
sudo systemctl start docker;
sudo usermod -aG docker ${USER};
su - ${USER};
id -nG;
sudo usermod -aG docker "username";
```

2. Vamos a crear un switch virtual en docker, para que los dos contenedores se encuentren en la misma red virtual.
`docker network create red-ejemplo`

3. Creamos dos contenedores, uno para Wordpress y otro para MariaDB.
- MariaDB:
```
docker run -d --name servidor-mariadb \
      --network red-wordpress \
      -e MYSQL_DATABASE=base_datos_wordpress \
      -e MYSQL_USER=user_wp \
      -e MYSQL_PASSWORD=pass_wp \
      -e MYSQL_ROOT_PASSWORD=root \
      mariadb;
```
- Wordpress:
```
docker run -d --name servidor-wordpress \
      --network red-wordpress \
      -e WORDPRESS_DB_HOST=servidor-mariadb \
      -e WORDPRESS_DB_USER=user_wp \
      -e WORDPRESS_DB_PASSWORD=pass_wp \
      -e WORDPRESS_DB_NAME=base_datos_wordpress \
      -p 8080:80 \
      wordpress;
```
