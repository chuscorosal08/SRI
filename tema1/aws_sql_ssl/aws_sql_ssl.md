# Instalación del servidor web Apache

Primero actualizamos la lista de paquetes disponibles en los repositorios configurados en el sistema.

```
sudo apt update
```

![](/img/intro/33.png)

```
sudo apt upgrade
```

![](/img/intro/34.png)


E instalamos Apache.

```
sudo apt-get install apache2
```

![](/img/intro/35.png)


# Autenticación MySQL

Primero instalamos PHP, MySQL y MariaDB.

```
sudo apt-get install apache2 php7.0 libapruti11-dbd-mysql -y
```

![](/img/intro/36.png)


```
sudo apt-get install mariadb-server mariadb-client -y
```

![](/img/intro/37.png)


Y activamos los servicios.

```
sudo systemctl enable apache2
```

```
sudo systemctl enable mysql
```

![](/img/intro/38.png)


Entramos en mysql.

```
sudo mysql -u root -p
```

Y creamos una base de datos.

```
create database defaultsite_db;
```

![](/img/intro/39.png)


Le damos permisos totales al admin.

```
GRANT SELECT, INSERT, UPDATE, DELETE ON defaultsite_db.* TO 'defaultsite_admin'@'localhost' IDENTIFIED BY 'usuario';
```

![](/img/intro/40.png)


```
GRANT SELECT, INSERT, UPDATE, DELETE ON defaultsite_db.* TO 'defaultsite_admin'@'localhost.localdomain' IDENTIFIED BY 'password';
```

![](/img/intro/41.png)


```
flush privileges;
```

![](/img/intro/42.png)


Entramos en la base de datos.

```
use defaultsite_db;
```

![](/img/intro/43.png)


Y creamos una tabla para los usuarios autenticados.

```
create table mysql_auth ( username varchar(191) not null, passwd varchar(191), groups varchar(191), primary key (username) );
```

![](/img/intro/44.png)


Transformamos una contraseña a hash para hacerlo más seguro.

```
htpasswd -bns siteuser siteuser
```

![](/img/intro/45.png)


E insertamos los datos en la tabla que hemos creado en la base de datos.

```
INSERT INTO `mysql_auth` (`username`, `passwd`, `groups`) VALUES('siteuser', '{SHA}tk7HEH6Wo7SKT6+3FHCgiGnJ6dA=', 'sitegroup');
```

![](/img/intro/46.png)


Habilitamos los módulos necesarios.

```
sudo a2enmod dbd
```

```
sudo a2enmod authn_dbd
```

```
sudo a2enmod socache_shmcb
```

```
sudo a2enmod authn_socache
```

![](/img/intro/47.png)


Creamos el directorio que estará protegido.

```
sudo mkdir /var/www/html/protecteddir
```

```
sudo chown -R www-data:www-data /var/www/html/protecteddir
```

![](/img/intro/48.png)


Y modificamos la configuración de Apache de nuestro dominio.

```
sudo nano /etc/apache2/sites-available/000-default.conf
```

![](/img/intro/49.png)


Introducimos lo siguiente.

```
DBDriver mysql
DBDParams "dbname=defaultsite_db user=defaultsite_admin pass=usuario"
 
DBDMin 4 
DBDKeep 8 
DBDMax 20 
DBDExptime 300
 
<Directory "/var/www/html/protecteddir"> 
# mod_authn_core and mod_auth_basic configuration 
# for mod_authn_dbd 
AuthType Basic 
AuthName "My Server"
 
# To cache credentials, put socache ahead of dbd here 
AuthBasicProvider socache dbd
 
# Also required for caching: tell the cache to cache dbd lookups! 
AuthnCacheProvideFor dbd 
AuthnCacheContext my-server
 
# mod_authz_core configuration 
Require valid-user
 
# mod_authn_dbd SQL query to authenticate a user 
AuthDBDUserPWQuery "SELECT passwd FROM mysql_auth WHERE username = %s"
```

![](/img/intro/50.png)


Y reiniciamos Apache.

```
sudo systemctl restart apache2
```

![](/img/intro/51.png)


Ahora cuando entremos al directorio por acceso web, nos pedirá el usuario y contraseña.

![](/img/intro/52.png)

![](/img/intro/53.png)

![](/img/intro/54.png)


# Certificados SSL

Abrimos los puertos http y https.

```
sudo uf allow "Apache Full"
```

![](/img/intro/55.png)


Activamos el módulo necesario.

```
sudo a2enmod ssl
```

![](/img/intro/56.png)


Y reiniciamos Apache

```
sudo systemctl restart apache2
```

![](/img/intro/57.png)


Creamos el certificado.

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
```

![](/img/intro/58.png)


Importante en Common Name introducir nuestra IP o nombre de dominio configurado.

```
Country Name (2 letter code) [XX]:ES
State or Province Name (full name) []:Huelva
Locality Name (eg, city) [Default City]:Huelva
Organization Name (eg, company) [Default Company Ltd]:LAMARISMA
Organizational Unit Name (eg, section) []:ASIR
Common Name (eg, your name or your server's hostname) []:tu_ip
Email Address []:webmaster@example.com
```

![](/img/intro/59.png)

Y modificamos la configuración de Apache de nuestro dominio.

```
sudo nano /etc/apache2/sites-available/000-default.conf
```

![](/img/intro/60.png)


Introducimos lo siguiente en un VirtualHost en el puerto 443.

```
<VirtualHost *:443>
   ServerName tu_ip
   DocumentRoot /var/www/html
```

```
   SSLEngine on
   SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
   SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
</VirtualHost>
```

![](/img/intro/61.png)

![](/img/intro/62.png)


Reiniciamos Apache.

```
sudo apache2ctl configtest
```

```
sudo systemctl reload apache2
```

![](/img/intro/63.png)


Y ya tenemos el certificado SSL autofirmado.

![](/img/intro/64.png)

![](/img/intro/65.png)

![](/img/intro/66.png)

![](/img/intro/67.png)
