# Despliegue de una aplicación web LAMP sencilla
## Creado por: **José García Torrescusa**

_En esta práctica realizaremos un despliegue de una pila LAMP en una instancia EC2 con la última versión de Ubuntu Server o Debian sin entorno gráfico._

## Añadir IP Elástica a una instancia.
1. Buscamos en el panel EC2 "Elastic IP", seleccionamos la zona fronteriza y le damos a Asignar.

2. Hacemos clic derecho a la IP y asociamos la IP. En el apartado instancia seleccionamos la instancia que queremos que tenga esta dirección IP. Después elegimos una IP Privada que queremos que tenga nuestra Instancia.

---

## Crear plantilla de lanzamiento
1. Nos iremos a la opción de plantillas e imágenes > Crear plantilla desde una imagen. No damos a la opción de No reboot. De esta forma creamos una imagen del sistema propia para poder usarla cuando queramos.

2. Introducimos los datos y comprobamos que el disco virtual sea el mismo e iniciamos la plantilla.

---

## Paso a paso de la práctica
1. Para que un fichero sql se ejecute directamente desde el sistema operativo, tendremos que usar el comando sudo mariadb -u [usuario] < [ruta del script].
``` sudo mariadb -u usuariofeliz < /home/usuariofeliz/scripts/database.sql. ```

2. Copiamos los ficheros php del directorio, para ello podemos usar cp con gitclone o copiar los ficheros mediante ssh. Modificamos el fichero config.php añadiendo un usuario y contraseña, también asociamos la base de datos.



