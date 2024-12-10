# Guia de intalacion y configuracion generica para poder aplicar en diferentes usos o recursos

# Instalación del servidor web Apache

Primero actualizamos la lista de paquetes disponibles en los repositorios configurados en el sistema.

```
sudo apt update
```
<img src="../Trabajo final 1 trimestre/img/1.png">
<img src="../Trabajo final 1 trimestre/img/2.png">

```
sudo apt upgrade
```

<img src="../Trabajo final 1 trimestre/img/3.png">
<img src="../Trabajo final 1 trimestre/img/4.png">
E instalamos el servicio de Apache.

```
sudo apt install apache2
```

<img src="../Trabajo final 1 trimestre/img/5.png">
<img src="../Trabajo final 1 trimestre/img/6.png">
Vamos a comprobar nuestra dirección IP para ver si se ha instalado correctamente (también podemos hacerlo con localhost).

```
sudo apt install net-tools
```

<img src="../Trabajo final 1 trimestre/img/7.png">
<img src="../Trabajo final 1 trimestre/img/8.png">

```
ifconfig
```

<img src="../Trabajo final 1 trimestre/img/9.png">
<img src="../Trabajo final 1 trimestre/img/10.png">

Y vemos que funciona correctamente el servicio de servidor.

<img src="../Trabajo final 1 trimestre/img/11.png">

Ahora vamos a añadir al fichero hosts el nombre de dominio que tendremos para cada uno de ellos.

<img src="../Trabajo final 1 trimestre/img/12.png">
<img src="../Trabajo final 1 trimestre/img/13.png">

Comprobamos y reiniciamos Apache.

```
apachectl configtest
```
<img src="../Trabajo final 1 trimestre/img/14.png">

```
sudo service apache2 restart
```
<img src="../Trabajo final 1 trimestre/img/15.png">



Y podemos observar que el DNS funciona. He modificado el index.html del dominio para que aparezca la siguiente página.

<img src="../Trabajo final 1 trimestre/img/16.png">
<img src="../Trabajo final 1 trimestre/img/17.png">

# Activar los módulos necesarios para ejecutar php y acceder a mysql

Instalamos el servicio de mysql.

```
sudo apt install mysql-server
```

<img src="../Trabajo final 1 trimestre/img/18.png">
<img src="../Trabajo final 1 trimestre/img/19.png">

```
sudo mysql_secure_installation
```

<img src="../Trabajo final 1 trimestre/img/20.png">
<img src="../Trabajo final 1 trimestre/img/21.png">
<img src="../Trabajo final 1 trimestre/img/22.png">
<img src="../Trabajo final 1 trimestre/img/23.png">
<img src="../Trabajo final 1 trimestre/img/24.png">
<img src="../Trabajo final 1 trimestre/img/25.png">

Comprobamos que funciona.

```
sudo mysql
```

<img src="../Trabajo final 1 trimestre/img/26.png">
<img src="../Trabajo final 1 trimestre/img/27.png">

```
exit
```

<img src="../Trabajo final 1 trimestre/img/28.png">
Y a continuación instalamos php.

```
sudo apt install php libapache2-mod-php php-mysql
```

<img src="../Trabajo final 1 trimestre/img/29.png">
<img src="../Trabajo final 1 trimestre/img/30.png">

Y comprobamos que está instalado revisando su versión.

```
php -v
```

<img src="../Trabajo final 1 trimestre/img/31.png">
<img src="../Trabajo final 1 trimestre/img/32.png">

# Instala y configura wordpress

Primero vamos a crear la estructura de directorios de cada dominio.

```
sudo mkdir /var/www/XXX
```

<img src="../Trabajo final 1 trimestre/img/33.png">
<img src="../Trabajo final 1 trimestre/img/34.png">

```
sudo chown -R $USER:$USER /var/www/XXX
```

<img src="../Trabajo final 1 trimestre/img/35.png">

Y también su fichero de configuración.

```
sudo nano /etc/apache2/sites-available/XXX.conf
```

<img src="../Trabajo final 1 trimestre/img/36.png">


```
<VirtualHost *:80>
  ServerName centro.intranet
  ServerAlias www.centro.intranet
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/XXX
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

<img src="../Trabajo final 1 trimestre/img/37.png">

Y de igual forma para el otro.

<img src="../Trabajo final 1 trimestre/img/38.png">
<img src="../Trabajo final 1 trimestre/img/39.png">

Activamos los sitios.

```
sudo a2ensite XXX
```

<img src="../Trabajo final 1 trimestre/img/40.png">


Comprobamos Apache y reiniciamos.

```
apachectl configtest
```

<img src="../Trabajo final 1 trimestre/img/41.png">

```
sudo service apache2 restart
```

<img src="../Trabajo final 1 trimestre/img/42.png">

```
sudo systemctl reload apache2
```

<img src="../Trabajo final 1 trimestre/img/43.png">
<img src="../Trabajo final 1 trimestre/img/44.png">

Instalamos MariaDB.

```
sudo apt-get install mariadb-client mariadb-server
```

<img src="../Trabajo final 1 trimestre/img/45.png">
<img src="../Trabajo final 1 trimestre/img/46.png">

Y también descargamos el directorio de WordPress.

```
sudo wget https://wordpress.org/latest.tar.gz
```

<img src="../Trabajo final 1 trimestre/img/47.png">
<img src="../Trabajo final 1 trimestre/img/48.png">

Lo descomprimimos.

```
tar -xzf latest.tar.gz
```

<img src="../Trabajo final 1 trimestre/img/49.png">


Y podemos ver la carpeta en el directorio principal.

```
ls
```

<img src="../Trabajo final 1 trimestre/img/50.png">

Vamos a moverlo a nuestro directorio.

```
sudo mv wordpress/* /var/www/XXX/
```

<img src="../Trabajo final 1 trimestre/img/51.png">

```
ls /var/www/XXX
```

<img src="../Trabajo final 1 trimestre/img/52.png">

Y ahora a hacer una copia del fichero de configuración para configurarlo más tarde.

```
sudo cp /var/www/XXX/wp-config-sample.php /var/www/XXX/wp-config.php
```

<img src="../Trabajo final 1 trimestre/img/53.png">

Entramos en MariaDB.

```
sudo mariadb
```

<img src="../Trabajo final 1 trimestre/img/54.png">
<img src="../Trabajo final 1 trimestre/img/55.png">

Y creamos una base de datos para WordPress.

```
create database wordpress;
```

```
create user 'usuario' identified by  '1234';
```

```
grant all privileges on wordpress.* to 'usuario' identified by '1234';
```

```
quit;
```

<img src="../Trabajo final 1 trimestre/img/56.png">

Configuramos WordPress.

```
sudo nano /var/www/XXX/wp-config.php
```

<img src="../Trabajo final 1 trimestre/img/57.png">

```
define( 'DB_NAME', 'wordpress' );

define( 'DB_USER', 'usuario' );

define( 'DB_PASSWORD', '1234' );
```

<img src="../Trabajo final 1 trimestre/img/58.png">

Y ahora al acceder a la página veremos el siguiente asistente de instalación.

<img src="../Trabajo final 1 trimestre/img/59.png">
<img src="../Trabajo final 1 trimestre/img/60.png">
<img src="../Trabajo final 1 trimestre/img/61.png">


Una vez instalado iniciamos sesión.

<img src="../Trabajo final 1 trimestre/img/62.png">

Y vemos la página de configuración de administrador.

<img src="../Trabajo final 1 trimestre/img/63.png">

Pero al entrar en la página principal de nuestro dominio veremos la página principal de WordPress.

<img src="../Trabajo final 1 trimestre/img/64.png">

# Activar el módulo wsgi

Activamos el módulo wsgi para permitir la ejecución de aplicaciones Python.

```
sudo apt-get install libapache2-mod-wsgi-py3
```

<img src="../Trabajo final 1 trimestre/img/65.png">
<img src="../Trabajo final 1 trimestre/img/66.png">

# Crea y despliega una pequeña aplicación python

Primero vamos a crear el directorio para la aplicación y los logs.

```
cd /var/www/XXX
```

```
mkdir mypythonapp
```

```
mkdir public_html
```

```
mkdir logs
```

<img src="../Trabajo final 1 trimestre/img/67.png">

Y creamos la aplicación Python

```
echo '# -*- coding: utf-8 -*-' > mypythonapp/controller.py
```

<img src="../Trabajo final 1 trimestre/img/68.png">

Y escribimos el siguiente código dentro del archivo.

```
def application(environ, start_response):
  output = b'<p>Bienvenido a mi <b>PythonApp</b>!!!</p>'
  start_responde('200 OK', [('Content-Type', 'text/html; charset=utf-8')])
  return [output]
```

<img src="../Trabajo final 1 trimestre/img/69.png">
<img src="../Trabajo final 1 trimestre/img/70.png">

Y en la configuración de nuestro dominio XXX agregamos el siguiente código.

```
DocumentRoot /var/www/XXX/public_html
WSGIScriptAlias / /var/www/XXX/mypythonapp/controller.py
ErrorLog /var/www/XXX/logs/error.log
CustomLog /var/www/XXX/logs/access.log combined

<Directory />
  Options FollowSymLinks
  AllowOverride All
</Directory>
```

<img src="../Trabajo final 1 trimestre/img/71.png">
<img src="../Trabajo final 1 trimestre/img/72.png">

Y ya podemos acceder.

<img src="../Trabajo final 1 trimestre/img/73.png">

# Protegemos el acceso a la aplicación python mediante autenticación

Creamos dentro de nuestro directorio otro llamado passwd, y creamos las credenciales.

```
mkdir /var/www/XXX/passwd
```

```
htpasswd -c /var/www/XXX/passwd/passwords usuario
```

<img src="../Trabajo final 1 trimestre/img/74.png">

Ahora dentro del fichero de configuración del servidor agregamos el siguiente código.

```
<Directory /var/www/XXX/mypythonapp>
  AuthType Basic
  AuthName "Restricted Files"
  AuthUserFile /var/www/XXX/passwd/passwords
  Require user usuario
</Directory>
```

<img src="../Trabajo final 1 trimestre/img/75.png">

Y ahora nos pide las credenciales al intentar acceder.

<img src="../Trabajo final 1 trimestre/img/76.png">

Una vez introducidas tenemos acceso de nuevo.

<img src="../Trabajo final 1 trimestre/img/77.png">

# Instala y configura awstat.

Instalamos el servicio.

```
sudo apt-get install awstats
```

<img src="../Trabajo final 1 trimestre/img/78.png">

Y activamos el módulo cgi y reiniciamos apache

```
sudo a2enmod cgi
```

```
sudo systemctl restart apache2
```

<img src="../Trabajo final 1 trimestre/img/79.png">

Modificamos el archivo de configuración de awstats.

```
sudo nano /etc/awstats/awstats.conf
```

<img src="../Trabajo final 1 trimestre/img/80.png">

Y hacemos los siguientes cambios.

```
SiteDomain="XXX"
```

```
HostAliases="XXX localhost 127.0.0.1"
```

<img src="../Trabajo final 1 trimestre/img/81.png">

```
AllowToUpdateStatsFromBrowser=1
```

<img src="../Trabajo final 1 trimestre/img/82.png">

Generamos las estadísticas iniciales.

```
sudo /usr/lib/cgi-bin/awstats.pl -config=XXX -update
```

<img src="../Trabajo final 1 trimestre/img/83.png">
<img src="../Trabajo final 1 trimestre/img/84.png">

Y configuramos Apache para awstats.

```
sudo cp -r /usr/lib/cgi-bin /var/www/XXX
```

```
sudo chown -R www-data:www-data /var/www/XXX/cgi-bin
```

```
sudo chmod -R 755 /var/www/XXX/cgi-bin
```

<img src="../Trabajo final 1 trimestre/img/85.png">

Vamos a modificar la configuración de awstats en Apache (creamos el archivo).

```
sudo nano /etc/apache2/conf-available/awstats.conf
```

<img src="../Trabajo final 1 trimestre/img/86.png">

Y agregamos lo siguiente.

```
Alias /awstatsclasses "/usr/share/awstats/lib"
Alias /awstats-icon "/usr/share/awstats/icon/"
Alias /awstatscss "/usr/share/doc/awstats/examples/css"
ScriptAlias /awstats/ /usr/lib/cgi-bin/
Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
```

<img src="../Trabajo final 1 trimestre/img/87.png">

Habilitamos la configuración y reiniciamos Apache.

```
sudo a2enconf awstats
```

```
sudo systemctl reload apache2
```

<img src="../Trabajo final 1 trimestre/img/88.png">
<img src="../Trabajo final 1 trimestre/img/89.png">

Y si accedemos a XXX/awstats/awstats.pl podemos ver las estadísticas de visita de nuestro dominio.

<img src="../Trabajo final 1 trimestre/img/90.png">

# Instalación del servidor web nginx

En mi caso yo he elegido nginx para instalar el segundo servidor.

```
sudo apt install nginx
```

<img src="../Trabajo final 1 trimestre/img/91.png">
<img src="../Trabajo final 1 trimestre/img/92.png">

Modificamos su archivo de configuración.

```
sudo nano /etc/nginx/nginx.conf
```

<img src="../Trabajo final 1 trimestre/img/93.png">

De forma que el http escuche en el puerto 8080.

```
listen 8080;
listen [::]:8080;
```

<img src="../Trabajo final 1 trimestre/img/94.png">

Creamos un nuevo fichero de configuración.

```
sudo nano /etc/nginx/sites-available/default
```

<img src="../Trabajo final 1 trimestre/img/95.png">

Y agregamos lo siguiente:

```
listen 8080 default_server;
listen [::]:8080 default_server;
```

<img src="../Trabajo final 1 trimestre/img/96.png">

Comprobamos la configuración y reiniciamos nginx.

```
sudo nginx -t
```

```
sudo systemctl restart nginx
```

<img src="../Trabajo final 1 trimestre/img/97.png">

Creamos el directorio donde estarán nuestros archivos.

```
sudo mkdir /var/www/nginx
```

<img src="../Trabajo final 1 trimestre/img/98.png">

Y dentro creamos un index.html

```
sudo nano /var/www/nginx/index.html
```

<img src="../Trabajo final 1 trimestre/img/99.png">

De la siguiente forma.

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

<img src="../Trabajo final 1 trimestre/img/100.png">

Y en el fichero de configuración de antes cambiamos el root a este nuevo directorio.

```
root /var/www/nginx;
```

<img src="../Trabajo final 1 trimestre/img/101.png">

Y agregamos el orden de index y el server_name.

```
index index.html index.htm index.nginx-debian.html;

server_name XXX;
```
<img src="../Trabajo final 1 trimestre/img/102.png">


Agregamos el nombre del dominio al fichero hosts.

```
sudo nano /etc/hosts
```
<img src="../Trabajo final 1 trimestre/img/103.png">


```
127.0.0.1      XXX
```

<img src="../Trabajo final 1 trimestre/img/104.png">

Reiniciamos el servidor.

```
sudo systemctl restart nginx
```

<img src="../Trabajo final 1 trimestre/img/105.png">

Y ya podemos acceder con el nombre del dominio y el puerto 8080.

<img src="../Trabajo final 1 trimestre/img/106.png">

Ahora instalamos phpmyadmin.

```
sudo apt install phpmyadmin
```

<img src="../Trabajo final 1 trimestre/img/107.png">
<img src="../Trabajo final 1 trimestre/img/108.png">

Creamos un enlace simbólico.

```
sudo ln -s /usr/share/phpmyadmin /var/www/nginx/phpmyadmin
```

<img src="../Trabajo final 1 trimestre/img/109.png">

Y modificamos los permisos.

```
sudo chown -R www-data:www-data /usr/share/phpmyadmin
```

```
sudo chmod 755 /usr/share/phpmyadmin
```

<img src="../Trabajo final 1 trimestre/img/110.png">

En el fichero de configuración de nuestro sitio agregamos lo siguiente.

```
location / {
  try_files $uri $uri/ =404;
}

location /phpmyadmin {
  root /var/www/nginx;
  index index.php index.html;

  location ~ ^/phpmyadmin/(doc|sql|setup)/ {
    deny all;
  }

  location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/run/php/php8.1-fpm.sock;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
  }
}
```

<img src="../Trabajo final 1 trimestre/img/111.png">

Instalamos php-fpm.

```
sudo apt install php8.1-fpm
```

<img src="../Trabajo final 1 trimestre/img/112.png">
<img src="../Trabajo final 1 trimestre/img/113.png">

Y ahora podemos acceder a phpmyadmin en nuestro dominio con XXX:8080/phpmyadmin.

<img src="../Trabajo final 1 trimestre/img/114.png">

Vamos a crear un usuario para entrar.

```
sudo mysql -y root -p
```

```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'usuario';
```

```
exit;
```

<img src="../Trabajo final 1 trimestre/img/115.png">

Y ya tenemos phpmyadmin listo y configurado.

<img src="../Trabajo final 1 trimestre/img/116.png">
