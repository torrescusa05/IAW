## Configurando Apache2 a través de sus ficheros de configuración
# Realizado por: José García Torrescusa.
---
_En este fichero aprenderemos a como configurar el servicio de apache2._
---
# Ejercicio1
1. Para cambiar el puerto por defecto del servicio http / https tendremos que irnos al fichero de configuración _ports.conf_ (etc/apache2/ports.conf).
```sudo nano /etc/apache2/ports.conf```

2. Una vez modificado el fichero tendremos que cambiar el puerto del sitio virtual por
defecto, esto lo podremos encontrar en el fichero _000-default.conf_ y cambiaremos la directiva **_<VirtualHost *(puerto que hemos puesto anteriormente)>_**.
```sudo nano /etc/apache2/sites-available/000-default.conf```

En caso de tener un sitio seguro por ssl tendremos que cambiar la directiva VirtualHost en el fichero _default-ssl.conf_.
```sudo nano /etc/apache2/sites-available/default-ssl.conf```

3. Reiniciamos el servicio de apache2 y comprobamos que la configuración funciones. **(Recuerda cambiar el reenvío de puertos NAT).**

---
# Ejercicio2
1. Para cambiar la carpeta pública de contenidos por defecto de Apache2 tendremos que acceder de nuevo al fichero de configuración _000-default.conf_. Tocaremos la directiva **DocumentRoot** y añadiremos la ruta nueva que hemos creado anteriormente.
```DocumentRoot [/home/usuario/sitioweb]```

2. Creamos un fichero en el directorio nuevo creado con cualquier texto. Cambiaremos los permisos de la carpeta y el propietario de dicha carpeta para que todo el mundo tenga acceso al documento. (Error Forbidden 403).
```sudo chown -R www-data:www-data /home/usuario/sitioweb/``` 
```sudo chmod 755 /home/usuario/```
```sudo chmod 755 /home/usuario/sitioweb```

3. Nos iremos al fichero de configuración para añadir el parámetro (Directory) para
asignar por defecto el fichero que hayamos creado en el directorio nuevo.
```
<Directory /home/usuario/sitioweb>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```
> Options Indexes FollowSymLinks --> Permite listar el directorio de ficheros que tenga ese recurso.
> AllowOverride None --> No permite el uso de fichero .htaccess
> Require all granted --> Permite que todos los usuarios puedan acceder al recurso.
---
# Ejercicio 3
1. Para desactivar o activar un módulo de apache2 usaremos los comandos `a2enmod` para activar y el comando `a2dismod`. Una vez realizado el comando reiniciamos el servicio.
---
# Ejercicio 4
1. Ahora configuraremos la directiva (DirectoryIndex), esta nos permitirá cargar una página por defecto cuando apuntemos al raíz del sitio web. Para ello nos iremos al fichero _000-default.conf_ y añadimos los ficheros que queremos que salgan por defecto.
```DirectoryIndex index.html index.php```
---
# Ejercicio 5
1. Crearemos dos hosts virtuales. Para ello crearemos dos ficheros de configuración en el directorio _/etc/apache2/sites-available_.
```sudo nano /etc/apache2/sites-available/sitio1.conf```

2. Creamos dos directorios nuevos relacionados con los sitios virtuales y un index.html para comprobar después su acceso.
```sudo mkdir -p /var/www/sitio1 && sudo nano /var/www/sitio1/index.html```

3. Ahora configuraremos los ficheros de los sitios virtuales, para ello añadiremos las condiciones y variables de configuración para este fichero.
`sudo nano /etc/apache2/sites-available/sitio1.conf`
```<VirtualHost *:80>
        ServerAdmin sitio1@sitio1.com
        ServerName sitio1.com
        DocumentRoot /var/www/sitio1

        # El bloque <Directory> es necesario para aplicar permisos a la carpeta
        <Directory /var/www/sitio1>
                Options Indexes FollowSymLinks
                AllowOverride All
                Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

4. Desactivamos el sitio por defecto creado mediante el comando _a2dissite_ y activamos los dos sitios virtuales con el comando _a2insite_. Después reiniciamos el servicio.
`sudo a2dissite 000-default.conf`
`sudo a2insite sitio1.conf`

5. Nos vamos al fichero /etc/hosts de nuestra máquina anfitrión y virtual y añadimos los servicios virtuales creados anteriormente. (127.0.0.1    sitio1.com). IMPORTANTE: Si no funciona los puertos ver el fichero ports.conf.
`sudo nano /etc/hosts`

6. Buscamos el sitio virtual.
http://sitio1.com:8080 --> Puerto reenviador de puertos.

---
# Ejercicio 6
1. Nos vamos al fichero ports.conf y añadimos que el servidor Apache2 escucha también por un segundo puerto.
`sudo nano /etc/apache2/ports.conf`
```
Listen 80
<VirtualHost *:80>
    DocumentRoot /var/www/sitio1/index.html 
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

Listen 8080
<VirtualHost *:8080>
    DocumentRoot /var/www/sitio2/index.html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

2. Nos vamos al sitio 2 y cambiamos el puerto por defecto.
`sudo nano /etc/apache2/sites-available/sitio2.conf`

3. Reiniciamos el servicio. (Crea una regla de reenvío de puertos en VBox)

---
# Ejercicio 7
1. Para consultar hosts virtuales activos tendremos que usar el comando o listamos el contenido de la carpeta /etc/apache2/sites-enabled:
`sudo apache2ctl -S && ls -l /etc/apache2/sites-enabled/`

---
# Ejercicio 8
1. Para comprobar la sintaxis de Apache2:
`sudo apache2ctl configtest`

---
# Ejercicio 9
1. Para ocultar la versión de apache tendremos que añadir los ficheros de los sitios los siguientes parámetros: ServerSignature Off ServerTokens Prod.
```                                         
ServerSignature Off
ServerTokens Prod
<VirtualHost *:80>
        ServerAdmin sitio1@sitio1.com
        ServerName sitio1.com
        DocumentRoot /var/www/sitio1

        # El bloque <Directory> es necesario para aplicar permisos a la carpeta
        <Directory /var/www/sitio1>
                Options Indexes FollowSymLinks
                AllowOverride All
                Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

2. Para comprobar las cabeceras http usaremos el comando:
`curl -Iv http://sitio1.com`

---
# Ejercicio10
- Ficheros logs:
- access.log --> Fichero de acceso,
- error.log --> Fichero de errores.

