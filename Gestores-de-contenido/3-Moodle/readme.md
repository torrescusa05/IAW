# Instalación paso a paso de Moodle
## Realizado por: José García Torrescusa.
_En esta práctica aprenderemos a como instalar Moodle y solucionar posibles errores que nos puedan salir en el camino. Al final de la práctica le aplicaremos un certificado SSL para que nuestro sitio web sea seguro, usaremos instancias EC2 de Amazon._
---
1. Antes de instalar Moodle tendremos que ver los requisitos que necesitamos como extensiones de PHP, configuraciones previas, etc... En nuestro caso al tener establecido un LAMP funcional estaremos listos para la instalación.

2. Para instalar Moodle tendremos que instalar git en nuestra instancia tendremos que pasar el fichero de moodle por ssh, para ello usamos el comando scp. Después lo movemos de directorio.
`sudo scp -i wordpress.pem  moodle-latest-501.tgz ubuntu@18.215.151.120:/home/ubuntu`
`tar -xvf moodle-latest-501.tgz moodle/`
`sudo mv moodle-latest-501.tgz /var/www/html`

3. Aseguramos los ficheros de moodle, para ello cambiamos el propietario de la carpeta y añadimos permisos restrictivos.
`chown -R root moodle/`
`chmod -R 0755 moodle/`

4. Creamos una base de datos que haga referencia a Moodle, para ello primero creamos una base de datos para que podamos añadir emojis y creamos un usuario y contraseña que tenga permisos para modificar la base de datos.
`CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;`
`CREATE USER 'moodleuser'@'localhost' IDENTIFIED BY 'usuario';`
`GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,CREATE TEMPORARY TABLES,DROP,INDEX,ALTER ON moodle.* TO 'moodleuser'@'localhost';`

5. Creamos un directorio para almacenar la información web de Moodle, es importante que no esté en el directorio raíz de publicación del servidor, ya que sería un gran fallo de seguridad. También lo podemos obviar, ya que el instalador lo creará solo.
`mkdir moodledata`
`sudo chmod -R 0777 moodledata/`
`sudo chown -R root:root moodledata/`

6. Una vez realizado todos estos pasos podemos elegir dos formas para poder instalar Moodle, a través de CLI o por interfaz web. En nuestro caso lo realizaremos vía web.
_Paso a paso por CLI_
```sudo chown www-data /var/www/html/moodle ```
```cd /var/www/moodle/admin/cli```
```sudo -u www-data /usr/bin/php install.php```
```chown -R www-data /var/www/html/moodle```
```php install.php```

_Vía web_
- Elegimos el idioma, e instalamos dos extensiones de php:
`sudo apt install php-curl`
`sudo apt install php-zip`
`sudo apt install php-gd`
`sudo apt install php-intl`
`sudo apt install php-mbstring`
`sudo apt install php-xmrlpc`
`sudo apt install php-soap`

- Indicamos donde está el fichero de datos de moodle. En mi caso lo he puesto en /var/www/modledata.

- Escogemos que nuestro motor de base de datos es MariaDB. Y conectamos nuestra base de datos creada anteriormente. Moodle nos generará un fichero config.php que lo tendremos que introducir en el directorio raíz de Moodle. Y cambiamos el parámetro: **$CFG->wwwroot    = 'http://[IP]/moodle/public' (quitamos /public y dejamos solo /moodle). 

