## Instalación paso a paso de WordPress
# Realizado por: José García Torrescusa.

1. Creamos una instancia nueva con LAMP en AWS o una VM, en mi caso usaré una plantilla de una máquina (LAMP del ejercicio 7) de AWS que creé en el anterior ejercicio. Si es una VM seguiremos los pasos de instalación:
```sudo apt update && sudo apt upgrade ```
```sudo apt install mariadb-server ```
```sudo apt install apache2 ```
```sudo mariadb-secure-installation ```
- Enter directamente si es la primera vez que lo iniciamos
- n
- Y
- Y
- Y
- Y
```sudo apt install php-fpm php-mysql -y ```
```sudo a2enmod proxy_fcgi setenvif ```
```sudo a2enconf php8.4-fpm (o versión 8.3)```

2. Instalamos wordpress en el directorio de publicación de apache (var/www/html) mediante wget.
```wget https://wordpress.org/latest.tar.gz -P /tmp ```

3. Descomprimimos el fichero mediante la herramienta tar. Explicación de parámetros:
- -x --> Indica que queremos extraer el contenido del archivo.
- -z --> Indica que queremos descomprimir el archivo.
- -v --> Habilita el modo verboso para mostrar por pantalla el proceso de descompresión.
- -f --> Se utiliza para indicar cúal es el nombre del archivo de entrada.
- -c --> Se utiliza para indicar cuál es el directorio destino.
```tar -xzvf /tmp/latest.tar.gz -C /tmp ```

4. Movemos el contenido de la carpeta wordpress al archivo de publicación.
```sudo mv -f /tmp/wordpress/* /var/www/html ```

5. Creamos la base de datos y el usuario que usaremos para WordPress. Primero definimos las variables que vamos a usar con el comando export.
```export WORDPRESS_DB_NAME="wordpress_db"```
```export WORDPRESS_DB_USER="torrescusa"```
```export WORDPRESS_DB_PASSWORD="Simire2"```
```export IP_CLIENTE_MYSQL="localhost" ```

```sudo mysql -u root <<< "DROP DATABASE IF EXISTS $WORDPRESS_DB_NAME"```
```sudo mysql -u root <<< "CREATE DATABASE $WORDPRESS_DB_NAME"```
```sudo mysql -u root <<< "CREATE USER $WORDPRESS_DB_USER@$IP_CLIENTE_MYSQL IDENTIFIED BY '$WORDPRESS_DB_PASSWORD'"```
```sudo mysql -u root <<< "GRANT ALL PRIVILEGES ON $WORDPRESS_DB_NAME.* TO $WORDPRESS_DB_USER@$IP_CLIENTE_MYSQL"```

6. Creamos un fichero de configuración wp-config.php basado en el fichero wp-config-sample.php.
```sudo cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php ```

7. En este paso tenemos que modificar las variables por defecto, por las que hemos usado en el comando export.
```sudo nano wp-config.php ```
```// ** Database settings - You can get this info from your web host ** //```
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress_db' );

/** Database username */
define( 'DB_USER', 'torrescusa' );

/** Database password */
define( 'DB_PASSWORD', 'Simire2' );

/** Database hostname */
define( 'DB_HOST', 'localhost' );

/** Database charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The database collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );

/**#@+

8. Cambiamos el propietario de la carpeta contenedora html, y a todos sus herederos.
```sudo chown -R www-data:www-data /var/www/html/ ```

9. Para tener enlaces permanentes creamos un fichero oculto de acceso. Añadimos el contenido siguiente.
`sudo nano .htaccess`
#BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
# END WordPress

10. Habilitamos el módulo rewrite de Apache y reiniciamos apache.
`sudo a2enmod rewrite`
`sudo systemctl restart apache2`

11. Nos vamos al navegador y buscamos http://[ip | NS máquina]/wp-config.php, seleccionamos el idioma y creamos el sitio web.


