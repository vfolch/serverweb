# serverweb
Recuperación M08-UF2

Primero de todo crearemos una maquina virtual Ubuntu Desktop
El cual tendremos que entrar a la terminal y con el comando sudo apt install apache2 instalaremos
el Apache para hacer el servidor web

Para empezar tendremos que entrar a la ruta /var/www/ y crearemos el archivo index.html con sudo nano

El cual solamente lo editaremos para poner un titulo y un parrafo simplemente para probar que funcione.

Dentro de la ruta /var/www/ tendremos que hacer un sudo git clone https://github.com/pmolrua/Chrono el cual clonaremos

Ahora tendremos que instalar php y el modulo del apache para Php
Con este comando: sudo apt install php libapache2-mod-php
Y con este comando habilitaremos el modulo PHP:
sudo a2enmod php

Ahora tendremos que reiniciar el apache para aplicar los cambios con el comando: sudo service apache2 restart

Despues tendremos descargar el archivo 404.html (que lo tenemos en la practica del campus) y lo tendremos que poner en el directorio /var/www/

Ahora abriremos el archivo de configuración del Apache que es el siguiente: sudo nano /etc/apache2/sites-available/000-default.conf

Y añadir dentro del archivo una línea dentro de la sección "<VirtualHost>" que será lo siguiente: ErrorDocument 404 /404.html

Aplicaremos los cambios con sudo service apache2 restart

Despues tendremos que instalar el OpenSSL con el comando: sudo apt install openssl

Y generaremos una clave privada y un certificado autofirmado: sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/server.key -out /etc/ssl/certs/server.crt

Volveremos a abrir el archivo de configuración de Apache (/etc/apache2/sites-available/default-ssl.conf)

Y tendremos que encontrar y actualizar la configuración con la ruta correcta de los certificados, que són los siguientes:
SSLCertificateFile      /etc/ssl/certs/server.crt
SSLCertificateKeyFile   /etc/ssl/private/server.key

Guardaremos los cambios de los archivos

Y entraremos de nuevo al archivo de configuración de Apache, el 000-default.conf
Dentro de la sección <VirtualHost> tendremos que añadir lo siguiente: Redirect permanent / https://ser/ (Que es como pone en la actividad que se debe de poner)

El cual sirve para redirigir al nombre de la web

Ahora crearemos el directorio admin dentro del directorio de documentos de Apache
Haremos un sudo mkdir /var/www/html/admin
Dentro de la carpeta admin añadiremos algunas imagenes, en mi caso he añadido 2

Y crearemos una protección de acceso para el directorio admin
Crearemos el archivo dentro del directorio /var/www/html/admin/.htaccess

Dentro del archivo tendremos que añadir lo siguiente:

AuthType Basic
AuthName "Acceso restringido"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user

Guardamos los cambios y cerramos

Y crearemos un archivo .htpassword para almacenar los nombres de usuario y contraseñas
Con el siguiente comando: sudo htpasswd -c /etc/apache2/.htpasswd ser

Guardaremos y reiniciamos el servicio de apache para aplicar los cambios

Para lo ultimo pero importante, tendremos que ir al directorio /etc/ y editar el archivo de hosts para relacionar las direcciones IP de los sitios web

Añadiremos las siguientes lineas:
127.0.0.1 ser
127.0.0.1 onoser

Ahora dentro de la ruta /var/www/
Crearemos un directorio llamado onoser
Y crearemos un index.html el cual tendremos que meter contenido dentro del archivo

Añadiremos la configuración del servidor virtual 'onoser'
Abriremos el archivo de configuración de Apache para ver los sitios virtuales:
sudo nano /etc/apache2/sites-available/onoser.conf

Y añadiremos lo siguiente dentro del archivo
<VirtualHost *:443>
    ServerName onoser
    DocumentRoot /var/www/onoser
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/server.crt
    SSLCertificateKeyFile /etc/ssl/private/server.key
</VirtualHost>

Habilitaremos el nuevo servidor virtual y reiniciaremos apache:
sudo a2ensite onoser.conf
Con ese comando

Y reiniciaremos por ultimo el servicio de apache.

Tendremos dos servidores virtuales en funcionamiento en mi servidor web apache
https://ser
http://onoser
