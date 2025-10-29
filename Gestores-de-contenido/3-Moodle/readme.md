# Instalación paso a paso de Moodle
## Realizado por: José García Torrescusa.
_En esta práctica aprenderemos a como instalar Moodle y solucionar posibles errores que nos puedan salir en el camino. Al final de la práctica le aplicaremos un certificado SSL para que nuestro sitio web sea seguro, usaremos instancias EC2 de Amazon._
---
1. Antes de instalar Moodle tendremos que ver los requisitos que necesitamos como extensiones de PHP, configuraciones previas, etc... En nuestro caso al tener establecido un LAMP funcional estaremos listos para la instalación.
```sudo apt install php php-cli php-fpm php-mysql php-pgsql php-zip php-gd php-mbstring php-curl php-xml php-intl php-soap php-xmlrpc libapache2-mod-php -y```

2. Para instalar Moodle tendremos que instalar git en nuestra instancia tendremos que pasar el fichero de moodle por ssh, para ello usamos el comando scp. Después lo movemos de directorio.
`sudo scp -i wordpress.pem  moodle-latest-501.tgz ubuntu@18.215.151.120:/home/ubuntu`
`tar -xvf moodle-latest-501.tgz moodle/`
`sudo mv moodle/ /var/www/html/`

3. Aseguramos los ficheros de moodle, para ello cambiamos el propietario de la carpeta y añadimos permisos restrictivos.
`sudo chown -R www-data:www-data /var/www/html/moodle`
`sudo chmod -R 755 /var/www/html/moodle`

4. Creamos una base de datos que haga referencia a Moodle, para ello primero creamos una base de datos para que podamos añadir emojis y creamos un usuario y contraseña que tenga permisos para modificar la base de datos.
`CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;`
`CREATE USER 'moodleuser'@'localhost' IDENTIFIED BY 'usuario';`
`GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,CREATE TEMPORARY TABLES,DROP,INDEX,ALTER ON moodle.* TO 'moodleuser'@'localhost';`

5. Creamos un directorio para almacenar la información web de Moodle, es importante que no esté en el directorio raíz de publicación del servidor, ya que sería un gran fallo de seguridad. También lo podemos obviar, ya que el instalador lo creará solo.
`sudo mkdir /var/moodledata`
`sudo chown -R www-data:www-data /var/moodledata`
`sudo chmod -R 770 /var/moodledata`

6. Una vez realizado todos estos pasos podemos elegir dos formas para poder instalar Moodle, a través de CLI o por interfaz web. En nuestro caso lo realizaremos vía web.
_Paso a paso por CLI_
```sudo chown www-data /var/www/html/moodle ```
```cd /var/www/moodle/admin/cli```
```sudo -u www-data /usr/bin/php install.php```
```chown -R www-data /var/www/html/moodle```
```php install.php```

_Vía web_

- Escogemos que nuestro motor de base de datos es MariaDB. Y conectamos nuestra base de datos creada anteriormente. Moodle nos generará un fichero config.php que lo tendremos que introducir en el directorio raíz de Moodle.

- Elegimos el idioma, y si nos alse el siguiente error (Incorrect $CFG->wwwroot in config.php. It should not end in /public) Tendremos que realizar lo siguiente:

- Buscar el index.php de moodle y cambiar la siguiente línea por esto:
`@header('Location: /var/www/html/moodle/public/');`

- Cambiamos la línea que hace referencia al error, poniendo tan solo nuestra url directamente.
`$CFG->wwwroot   = 'http://ec2-54-167-87-77.compute-1.amazonaws.com';` 

- Cambiamos el sitio default de apache2 y apuntamos a la carpeta /public de moodle.
```
    # Directiva DocumentRoot
    DocumentRoot /var/www/html/moodle/public
```
- URL del error, con su solución en el mismo foro: https://moodle.org/mod/forum/discuss.php?d=469996

- Para cambiar el valor max_input_vars tendremos que irnos al directorio /etc/php8.3/apache2/php.ini y buscar el parámetro max_input_vars. Tendremos que cambiar el 1000 por 5000. (Descomentar la línea, quitar **;**). Simire2_


