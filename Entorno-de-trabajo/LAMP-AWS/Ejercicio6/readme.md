# Aprende a instalar aplicaciones sobre LAMP en AWS
## Creado por: **José García Torrescusa**

_En este fichero el paso a paso de como instalar phpMyAdmin, Adminer y GoAccecs, y que configuración debemos realizar para conectarnos a ellos._

---
# phpMyAdmin
1. Para instalarlo usaremos apt además de instalar a la vez varias bibliotecas.
`sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl -y`

2. Nos saldrá que necesitaremos reconfigurar un servicio y aquí seleccionamos apache2 y seguimos. Después nos pedirá que si queremos confirmar el uso de la base de datos del paquete "dbconfig-common" para configurarla, indicamos Yes, y establecemos una contraseña para el servicio.

3. Una vez instalado nos vamos al navegador e introducimos en la barra navegadora:
http://[DNS Público o IP Pública de la EC2]/phpmyadmin

---
# Adminer
1. Para instalar adminer tendremos que descargar el paquete desde GitHub, para ello primero creamos un directorio en /var/www/html.
`sudo mkdir -p /var/www/html/adminer`

2. Con el comando wget descargamos desde la dirección del enlace, y para indicar la ruta usamos el parámetro -P.
`wget https://github.com/vrana/adminer/releases/download/v4.8.1/adminer-4.8.1-mysql.php -P /var/www/html/adminer`

3. Para acceder por él, hacemos el mismo paso que en phpMyAdmin, buscamos el servidor y añadimos a la ruta /adminer/[nombre del fichero.php].

---
# GoAccess
1. Ahora instalamos un analizador de logs, esto nos permite encontrar más fácil que esta sucediendo en nuestro servidor. Para instalarlo usamos apt.
`sudo apt install goaccess -y`

2. Para usar esta utilidad tenemos que tipear en la terminal:
`goaccess /var/log/apache2/access.log -c`