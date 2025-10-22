# Instalación paso a paso de OnlyDocuments
## Realizado por: José García Torrescusa.
_"Onlydocuments" se refiere a la suite ofimática ONLYOFFICE Docs, que es una colección de editores en línea para crear, visualizar y colaborar en documentos de texto, hojas de cálculo y presentaciones._
---
1. Descargamos el motor de base de datos de PostgreSQL y creamos una base de datos y usuarios asociado.
`sudo apt install postgresql`
`sudo -i -u postgres psql -c "CREATE USER onlyoffice WITH PASSWORD 'onlyoffice';"`
`sudo -i -u postgres psql -c "CREATE DATABASE onlyoffice OWNER onlyoffice;";`

2. Instalamos rabbitmq que es el motor de OnlyOffice, si lo estamos instalando en una versión superior a Ubuntu 18.04 tendremos que instalar también un motor nginx.
`sudo apt install rabbitmq-server`
`sudo apt install nginx-extras`

3. Podemos cambiar el puerto por defecto del servicio de OnlyOffice, en nuestro caso mantendremos el puerto 80.
`echo onlyoffice-documentserver onlyoffice/ds-port select 80 | sudo debconf-set-selections`

4. Añadimos una clave GPG para poder acceder al fichero de descarga de OnlyOffice, una vez realizado descargamos la llave GPG y cambiamos sus permisos y lo movemos a otro directorio.
`mkdir -p -m 700 ~/.gnupg`
`curl -fsSL https://download.onlyoffice.com/GPG-KEY-ONLYOFFICE | gpg --no-default-keyring --keyring gnupg-ring:/tmp/onlyoffice.gpg --import`
`chmod 644 /tmp/onlyoffice.gpg`
`sudo chown root:root /tmp/onlyoffice.gpg`
`sudo mv /tmp/onlyoffice.gpg /usr/share/keyrings/onlyoffice.gpg`

5. Añadimos el repositorio de OnlyOffice y actualizamos paquetes.
`echo "deb [signed-by=/usr/share/keyrings/onlyoffice.gpg] https://download.onlyoffice.com/repo/debian squeeze main" | sudo tee /etc/apt/sources.list.d/onlyoffice.list`

6. Instalamos el paquete de tipo de letras y fuentes de OnlyOffice. Y el fichero de instalación de OnlyOffice.
`sudo apt-get install ttf-mscorefonts-installer`
`sudo apt-get install onlyoffice-documentserver`