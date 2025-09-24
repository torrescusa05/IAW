# Instalación paso por paso pila LAMP
## Creado por: **José García Torrescusa**
    En este documento repasaremos el paso a paso para crear una pila LAMP. El nombre de LAMP
    proviene de las siguientes siglas:
    -   L: Linux
    -   A: Apache
    -   M: MariaDB
    -   P: PHP

    1. Actualizamos el sistema.
    -   sudo apt update && sudo apt upgrade

    2. Instalamos apache2 y mariadb. Configuramos la seguridad de mariadb.
    -   sudo apt install mariadb-server
    -   sudo apt install apache2
    -   sudo mariadb-secure-installation

    3.  Instalamos PHP.
    -   sudo apt install php-fpm php-mysql -y

    4. Habilitamos los módulos de configuración de apache2 para incorporar las nuevas bibliotecas
    instaladas de PHP. Después reiniciamos el servicio.
    -   sudo a2enmod proxy_fcgi setenvif
    -   sudo a2enconf php8.4-fpm

## Comprobar que php funcione
    Creamos un archivo de prueba en la carpeta pública de apache (var/www/html) e inscrustamos el siguiente
    código.
       <?php
           phpinfo();
       ?>

    Probamos ahora que php funcione con mariadb, creamos otro fichero en la carpeta pública con el siguiente código.
    <?php

    // Bloque para probar la conexión a la base de datos
    echo "<h1>Prueba de conexión a MariaDB</h1>";
    $servidor = "localhost";
    $usuario_db = "root";
    $contrasena_db = "tu_contraseña_segura"; // <-- ¡CAMBIA ESTO!
    $nombre_db = "mysql"; // Usamos la base de datos 'mysql' que siempre existe

    try {
        $conn = new PDO("mysql:host=$servidor;dbname=$nombre_db", $usuario_db, $contrasena_db);
        // Configurar el modo de error de PDO para que lance excepciones
        $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        echo "<p style='color:green;'>¡Conexión a MariaDB exitosa!</p>";
    } catch(PDOException $e) {
        echo "<p style='color:red;'>Error en la conexión: " . $e->getMessage() . "</p>";
    }

    // Cerrar la conexión
    $conn = null;
    ?>
