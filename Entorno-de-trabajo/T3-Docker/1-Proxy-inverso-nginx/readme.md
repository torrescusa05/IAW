# Instalación paso a paso de un servidor proxy inverso con Nginx
## Realizado por: José García Torrescusa.
_Prueba_
---
## Preparación de entorno
1. Usaremos una instancia de AWS EC2, como requisitos básicos necesitamos una AMI con Ubuntu Server y con 1GB de RAM. Añadimos un grupo de seguridad y su clave privada.

2. Nos conectamos por SSH a nuestra máquina e instalamos Docker.
```
sudo apt update;
sudo apt install docker.io -y;
sudo systemctl enable docker;
sudo systemctl start docker;
sudo usermod -aG docker ${USER};
su - ${USER};
id -nG;
sudo usermod -aG docker "username";
```
3. Ejecutamos un contenedor de prueba de Apache2, para comprobar la conectividad de nuestra instancia. Si no podemos acceder al sitio web, seguramente tengamos que abrir un puerto en el grupo de seguridad.
```docker run -d --name apache -p 8080:80 httpd```
---
## Instalación nginx
1. Una vez preparado nuestro entorno vamos a instalar nginx y certbot, para la generación de un certicado ssl con let's encrypt!.
```
sudo apt install nginx -y;
sudo snap install core;
sudo snap refresh core;
sudo snap install --classic certbot;
```
2. Ahora configuraremos nuestro servidor nginx para que realice la función de un proxy inverso. Para ello tendremos que crear un fichero en el directorio [/etc/nginx/sites-available/apache_proxy] el cual tiene que tener como propietario el usuario root y permisos establecidos en 644.
`sudo nano /etc/nginx/sites-available/apache_proxy`
```        
server {
    listen 80;
    server_name "DNS de AMAZON o DDNS Establecido por tí";

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
3. Borramos la configuración por defecto y activamos el fichero creado anteriormente. Le tendremos que asignar un enlace simbólico.
```
sudo rm /etc/nginx/sites-enabled/default;
sudo ln -s /etc/nginx/sites-available/apache_proxy /etc/nginx/sites-enabled/;
sudo nginx -t; --> Comprobar sintaxis de los ficheros
sudo systemctl restart nginx;
```
4. Añadimos nuestro dominio noip a Let's Encrypt mediante certbot
`sudo certbot --nginx -d gtj.hopto.org`

5. Por último accedemos desde el navegador a nuestro sitio web, con el protocolo https://.
