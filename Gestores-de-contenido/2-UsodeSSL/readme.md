# Generar un certificado SSL con Lets Encrypt
## Realizado por: José García Torrescusa.

_En esta práctica aprenderemos a generar un certificado SSL para nuestra web de WordPress por ejemplo, para ello usaremos el generador de Let's Encrypt para el certificado y No-IP para tener un DNS dinámico activado._

_HTTPS o protocolo seguro de hipertexto, es un protocolo de la capa de aplicación HTTP, destinado a cifrar la información de los datos del hipertexto._
---
1. Iniciamos nuestra instancia EC2 y abrimos en el grupo de seguridad los puertos 22, 80 y 443 (SSH, HTTP, HTTPS). Después le asignaremos una IP Elástica de AWS, ya que cuando se inicie de nuevo la máquina no cambie de IP y perdamos la certificación.

2. Sacamos la IP Pública de nuestra máquina, en este caso será la IP Elástica asociada que tendremos con la instancia EC2.

3. Necesitamos registrar nuestra IP en un dominio, para ello nos vamos a la web de No-IP y crearemos una cuenta, podemos usar el correo coorporativo, en mi caso usaré el dominio hopto. (La IP del dominio es la de nuestra máquina EC2).

4. Ahora accedemos a la instancia EC2 por SSH para instalar el certbot, para ello tendremos que usar el instalador de paquetes snap, que lo tendremos que instalar si no lo tenemos previamente.
`sudo apt install snapd`
`sudo snap install core && sudo snap refresh core`

5. Eliminamos si existiese alguna instalación previa a cerbot. E instalamos certbot y le creamos un enlace simbólico en el sistema.
`sudo apt autoremove certbot -y`
`sudo snap install --classic certbot`
`sudo ln -fs /snap/bin/certbot /usr/bin/certbot`

6. Obtenemos el certificado y configuramos nuestro servidor Apache. En la última pregunta le introducimos nuestro nombre del dominio, que será el dominio que hemos generado con No-IP.
`sudo certbot --apache`

7. Ya tendríamos HTTPS en nuestro sitio web.

# Importante!!!
_Si queremos que WordPress cargue una vez realizado este cambio tendremos que acceder al fichero wp-config.php y añadiremos debajo de la línea `define( 'DB_COLLATE', '');` los siguientes datos._
`define( 'WP_HOME', 'https://gtj.hopto.org' );`
`define( 'WP_SITEURL', 'https://gtj.hopto.org');`
