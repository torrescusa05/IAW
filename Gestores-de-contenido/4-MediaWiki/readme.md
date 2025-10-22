# Instalación paso a paso de MediaWiki
## Realizado por: José García Torrescusa.
_MediaWiki es un software libre y de código abierto que se utiliza para crear y gestionar wikis, siendo su ejemplo más conocido Wikipedia. Permite la creación de contenido de forma colaborativa, donde múltiples usuarios pueden editar y organizar información conjuntamente. Es una plataforma potente, escalable y personalizable, con funcionalidades para seguir cambios, gestionar permisos de usuario, y manejar contenido multimedia._
---
1. Seguiremos los pasos de la documentación y descargamos los complementos php que necesitamos para instalar este gestor. También tendremos que instalar php8.1.0, para ello tendremos que añadir el repositorio de descarga en los paquetes de aptitude.
`sudo apt install php-intl php-mbstring php-apcu php-curl php-mysql php-xml`
`sudo apt install software-properties-common`
`sudo add-apt-repository ppa:ondrej/php`
`sudo apt update`
`sudo apt install php8.1`

2. Nos movemos al directorio de publicación de apache2, y descargamos el instalador de MediaWiki. Una vez instalado descomprimimos el fichero comprimido. Después renombramos la carpeta para facilitar el acceso a la misma.
`sudo wget https://releases.wikimedia.org/mediawiki/1.44/mediawiki-1.44.2.zip`
`sudo unzip mediawiki-1.44.2.zip`
`sudo mv mediawiki-1.44.2 mediawiki`

3. Asignaremos permisos y cambiaremos el propietario de la carpeta de mediawiki, para que los usuarios a la hora de acceder a nuestro sitio no tenga problemas de permisos.
`sudo chown -R www-data:www-data /var/www/html/mediawiki`

4. Ahora tocaremos los ficheros de configuración de Apache2 para habilitar el sitio web de mediawiki, para ello nos iremos al directorio de sitios disponibles de apache2, y agregaremos las siguientes líneas:
`sudo nano /etc/apache2/sites-available/mediawiki.conf`
```
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html/mediawiki
        ServerName (DNS de la EC2 / IPv4 Pública)

        <Directory /var/www/html/mediawiki/>
                Options FollowSymLinks
                AllowOverride All
                Require all granted
        </Directory>
</VirtualHost>
```
5. Deshabilitamos el sitio por defecto de apache2 e habilitamos el sitio configurado anteriormente y recargamos el servidor.
`sudo a2dissite 000-default.conf`
`sudo a2ensite mediawiki.conf`
`sudo systemctl reload apache2`

6. Accedemos por vía web al instalador web donde indicaremos el idioma, e indicamos donde se sitúa la base de datos, si no la tenemos creada anteriormente la crearemos. Indicamos el nombre de usuario y contraseña y pasamos ya de más preguntas.
`CREATE DATABASE mediawiki`
`CREATE USER 'wikiuser'@localhost IDENTIFIED BY 'wiki_password';`
`GRANT ALL PRIVILEGES ON mediawiki TO 'wikiuser'@'localhost';`

7. El último paso es almacenar el fichero LocalSettings.php en el mismo lugar que index.php de la instalación de MediaWiki, para ello usaremos el comando scp para pasar este fichero.
`scp -i wordpress.pem Descargas/LocalSettings.php ubuntu@ec2-54-227-101-22.compute-1.amazonaws.com:/home/ubuntu`
`sudo mv /home/ubuntu/LocalSettings.php /var/www/html/mediawiki`
`sudo chown www-data:www-data mediawiki/LocalSettings.php`

8. Por último eliminamos el instalador y el fichero de configuración de mediawiki.
`sudo rm -r /var/www/html/mediawiki/mw-config`

## Si tenemos que cambiar el DNS público al ser una instancia AWS.
1. Irnos al fichero /var/www/html/mediawiki/LocalSettings.php y buscar línea que empiece por $wgServer.
`sudo nano /var/www/html/mediawiki/LocalSettings.php`

2. Modificar fichero de configuración de Apache2 del sitio de mediawiki y reiniciamos el servidor.
`sudo nano /etc/apache2/sites-available/tu-sitio-wiki.conf`


