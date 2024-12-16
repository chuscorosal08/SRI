# Trabajando con scripts

Creamos un script para cada uno de los siguientes problemas:

+ Crea un script que añada un puerto de escucha en el fichero de configuración de Apache. El puerto se recibirá como parámetro en la llamada y se comprobará que no esté ya presente en el fichero de configuración.

Primero creamos un archivo ejecutable para escribir el script.

![](/img/intro/17.png)

Y dentro de él escribimos lo siguiente:

```
#!/bin/bash

if [ $# -eq 0 ]; then
  echo "ERROR. No se han pasado parámetros para el número de puerto de escucha"
elif grep -q "Listen $1" /etc/apache2/ports.conf; then
  echo "ERROR. El puerto ya existe"
else
  cp /etc/apache2/ports.conf /etc/apache2/ports.conf.bak
  echo "Listen $1" >> /etc/apache2/ports.conf
  echo "Puerto $1 añadido al fichero de escucha de puertos"
fi
```

![](/img/intro/18.png)

Hacemos las comprobaciones.

![](/img/intro/19.png)

Y comprobamos que de verdad se ha añadido al fichero.

![](/img/intro/20.png)

+ Crea un script que añada un nombre de dominio y una ip al fichero hosts. Debemos comprobar que no existe dicho dominio en el fichero hosts

Primero creamos un archivo ejecutable para escribir el script y dentro de él escribimos lo siguiente:

```
#!/bin/bash

if [ $# -lt 2 ]; then
  echo "ERROR. No se han pasado suficientes parámetros"
elif grep -q "$2    $1" /etc/hosts; then
  echo "ERROR. La IP en conjunto a ese nombre de dominio existe"
else
  cp /etc/hosts /etc/hosts.bak
  echo "$2    $1" >> /etc/hosts
  echo "La IP $2 y el nombre de dominio $1 han sido añadidos al fichero de hosts"
fi
```

![](/img/intro/21.png)

Hacemos las comprobaciones.

![](/img/intro/22.png)

Y comprobamos que de verdad se ha añadido al fichero.

![](/img/intro/23.png)

Reiniciamos el servicio.

![](/img/intro/24.png)
![](/img/intro/25.png)

Y vemos que ahora al introducir ese nombre de dominio, nos lleva a la IP que hemos introducido.

![](/img/intro/26.png)

+ Crea un script que nos permita crear una página web con un título, una cabecera y un mensaje

Primero creamos un archivo ejecutable para escribir el script y dentro de él escribimos lo siguiente:

```
#!/bin/bash

if [ $# -eq 0 ]; then
  echo "ERROR. No se han pasado parámetros para el nombre de la página"
elif ls /var/www/mi_dominio | grep -q "$1"; then
  echo "La página ya existe"
else
  echo "Escriba el título"
  read tit
  echo "Escriba la cabecera"
  read cab
  echo "Escriba el mensaje"
  read men
  echo "<!DOCTYPE html>" >> /var/www/mi_dominio/$1.html
  echo "<html>" >> /var/www/mi_dominio/$1.html
  echo "<head>" >> /var/www/mi_dominio/$1.html
  echo "<title>$tit</title>" >> /var/www/mi_dominio/$1.html
  echo "</head>" >> /var/www/mi_dominio/$1.html
  echo "<body>" >> /var/www/mi_dominio/$1.html
  echo "<h1>$cab</h1>" >> /var/www/mi_dominio/$1.html
  echo "<p>$men</p>" >> /var/www/mi_dominio/$1.html
  echo "</body>" >> /var/www/mi_dominio/$1.html
  echo "</html>" >> /var/www/mi_dominio/$1.html
fi
```

![](/img/intro/27.png)

Hacemos las comprobaciones.

![](/img/intro/28.png)

Reiniciamos el servicio y comprobamos que de verdad se ha añadido al fichero.

![](/img/intro/29.png)
![](/img/intro/30.png)
![](/img/intro/31.png)

Y vemos que ahora al introducir el recurso en la URL, nos lleva a este.

![](/img/intro/32.png)
