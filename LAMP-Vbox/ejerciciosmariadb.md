## Configurando mariadb a través de sus ficheros de configuración
# Realizado por: José García Torrescusa.
---
_En este ejercicio aprenderemos a cómo acceder a mariadb / mysql a través de la terminal CLI_
---
# Ejercicio 1
1. Primero instalamos el servicio de mysql / mariadb server en nuestra máquina, para ello usaremos apt.
`sudo apt install mariadb-server -y`

2. Para iniciar los servicios de mariadb usaremos el comando service o systemctl. Este comando nos servirá también para ver el estado del servicio o pararlo cuando sea necesario. También podemos iniciarlo a través del script de lanzamiento del programa, que se encuentra en la ruta **/etc/init.d/mariadb**.
`systemctl start mariadb`
`systemctl stop mariadb`
`systemctl status mariadb`
`sudo /etc/init.d/mariadb`

3. Los archivos de conf. de mariadb se encuentran en la carpeta /etc/mysql/mariadb.cnf.
`nano /etc/mysql/mariadb.cnf`

4. Para ver los logs del servidor tendremos que ir a:
`sudo cat /var/log/mariadb/mariadb.log`

5. Realizamos la instalación segura mediante el comando:
`sudo mariadb-secure-installation`
---
# Ejercicio 2
1. Para acceder dentro de mariadb tendremos que poner en la terminal `sudo mariadb` y para consultar las bases de datos existentes que hay en nuestro servidor usaremos el comando.
`SHOW DATABASES`

2. Para crear una base de datos usamos CREATE y para borrar la base de datos DROP.
`CREATE DATABASE [nombre bd]`
`DROP DATABASE [nombre bd]`

3. Para seleccionar una BD usamos USE.
`USE [nombre bd]`

4. Para listar las tablas usamos SHOW TABLES una vez que hayamos seleccionado la base de datos.
`SHOW TABLES`

5. Para que nos aparezca la estructura de las tablas con los tipos de datos que contiene usaremos:
`DESCRIBE [nombre de la tabla]`
---
# Ejercicio 3
1. Para eliminar un usuario de una BD, tendremos que usar el comando DROP.
`DROP USER IF EXISTS 'user'@'localhost;'`

2. Para crear un nuevo usuario en la BD usaremos CREATE, también añadiremos IDENTIFIED BY para introducirle una contraseña.
`CREATE USER 'nombre'@'localhost' INDETIFIED BY '*'`

3. Tipos de permisos:
> **ALL PRIVILEGES** --> Otorga al usuario control total en cualquier BD del sistema.
> **CREATE** --> Permite crear nuevas tablas y BD.
> **DROP** --> Permite eliminar tablas.
> **DELETE** --> Permite eliminar registros.
> **INSERT** --> Permite ingresar datos en el sistema.
> **SELECT** --> Permite leer registros de tablas.
> **UPDATE** --> Permite actualizar registros seleccionados.
> **GRANT OPTION** -->  Permite asignar permisos a otros usuarios.

4. Para asignar permisos a un usuario usaremos GRANT. (* da permisos a todas las BD).
`GRANT [permiso] ON [nombre_base_de_datos].[nombre_tabla] TO 'nombre_usuario'@'localhost`
`FLUSH PRIVILEGES`

5. Para eliminar permisos a un usuario usaremos REVOKE.
`REVOKE [permiso] ON [nombre_base_de_datos].[nombre_tabla] FROM 'nombre_usuario'@'localhost';` 

6. Y para revisar los usuarios existentes en nuestro sistema BD usamos el comando SELECT.
`SELECT user,host FROM mysql.user;`
---
# Ejercicio 4
- Vamos a crear un script sql que nos muestre las bases de datos existentes en el sistema y los usuarios con sus IP donde se pueden conectar. Para ello creamos un fichero sql y añadimos dentro:
```
SHOW DATABASES;
SELECT user,host FROM mysql.user;
```
- Para ejecutar el script pondremos en la terminal:
`mariabd -u root -p < script.sql`
 