# Actividad #4 - Docker: Almacenamiento y Redes

## Ejercicio

### 1. Referencia

* Documentación del módulo tres "Almacenamiento y redes Docker" del curso: [https://github.com/josedom24/curso_docker_ies](https://github.com/josedom24/curso_docker_ies)

### 2. Realiza al menos tres de los ejemplos mostrados en el módulo y documenta cada uno

A continuación, se presentan tres ejemplos basados en el módulo de almacenamiento y redes de Docker:

### [Ejemplo 2: Despliegue de la aplicación Temperaturas](https://github.com/josedom24/curso_docker_ies/blob/main/modulo3/temperaturas.md)

La aplicación Temperaturas consta de dos microservicios:

1. **Frontend** (imagen: `iesgn/temperaturas_frontend`)
    * Puerto: `3000/tcp`
    * Ofrece interfaz web

2. **Backend** (imagen: `iesgn/temperaturas_backend`)
    * Puerto: `5000/tcp`
    * API Restful

#### Pasos para el despliegue

1. Crear una red:

    ```bash
    docker network create red_temperaturas
    ```

2. Ejecutar el contenedor backend:

    ```bash
    docker run -d --name temperaturas-backend --network red_temperaturas iesgn/temperaturas_backend
    ```

3. Ejecutar el contenedor frontend:

    ```bash
    docker run -d -p 80:3000 --name temperaturas-frontend --network red_temperaturas iesgn/temperaturas_frontend
    ```

    ![Example 1](/docker/.imgs/Act-4/Fig1.png)

#### Observaciones

* Aplicación sin estado, no requiere almacenamiento adicional.
* No es necesario mapear el puerto del backend.
* El nombre `temperaturas-backend` es crucial para la resolución DNS.

##### Configuración alternativa

Si se cambia el nombre del contenedor backend:

1. Ejecutar backend con nombre diferente:

    ```bash
    docker run -d --name temperaturas-api --network red_temperaturas iesgn/temperaturas_backend
    ```

2. Configurar frontend con variable de entorno:

    ```bash
    docker run -d -p 80:3000 --name temperaturas-frontend -e TEMP_SERVER=temperaturas-api:5000 --network red_temperaturas iesgn/temperaturas_frontend
    ```

Esta configuración permite que el frontend se conecte al backend con el nuevo nombre.

### [Ejemplo 3: Despliegue de Wordpress + mariadb](https://github.com/josedom24/curso_docker_ies/blob/main/modulo3/wordpress.md)

Este despliegue consiste en dos contenedores:

1. **MariaDB** (imagen: `mariadb`)
   * Proporciona la base de datos para WordPress.
   * Se configura mediante variables de entorno.

2. **WordPress** (imagen: `wordpress`)
   * Proporciona el servidor web con la aplicación WordPress.
   * Configura automáticamente el archivo `wp-config.php` usando variables de entorno.

#### Pasos para el despliegue (Ej3)

1. Crear una red para conectar ambos contenedores:

   ```bash
   docker network create red_wp
   ```

2. Crear y ejecutar el contenedor de MariaDB:

   ```bash
   docker run -d --name servidor_mysql \
                  --network red_wp \
                  -v /opt/mysql_wp:/var/lib/mysql \
                  -e MYSQL_DATABASE=bd_wp \
                  -e MYSQL_USER=user_wp \
                  -e MYSQL_PASSWORD=asdasd \
                  -e MYSQL_ROOT_PASSWORD=asdasd \
                  mariadb
   ```

   * Configura la base de datos con las credenciales y parámetros definidos en las variables de entorno.

3. Crear y ejecutar el contenedor de WordPress:

   ```bash
   docker run -d --name servidor_wp \
                  --network red_wp \
                  -v /opt/wordpress:/var/www/html/wp-content \
                  -e WORDPRESS_DB_HOST=servidor_mysql \
                  -e WORDPRESS_DB_USER=user_wp \
                  -e WORDPRESS_DB_PASSWORD=asdasd \
                  -e WORDPRESS_DB_NAME=bd_wp \
                  -p 80:80 \
                  wordpress
   ```

   * Configura automáticamente la conexión con la base de datos utilizando las variables de entorno.

4. Verificar que los contenedores están funcionando:

   ```bash
   docker ps
   ```

![Example 2](/docker/.imgs/Act-4/Fig2.png)

##### Observaciones (Ej3)

* **MariaDB**:
  * Configura la base de datos según las variables de entorno proporcionadas.
  * Permite conexiones desde otros contenedores gracias a su configuración interna.
  
* **WordPress**:
  * Usa `WORDPRESS_DB_HOST` para conectarse al contenedor MariaDB mediante su nombre DNS (`servidor_mysql`).
  * Solo es necesario mapear el puerto del servidor web (`-p 80:80`) para acceder desde el exterior.

Ambos contenedores se comunican internamente a través de la red `red_wp`, lo que permite que WordPress acceda al puerto 3306 del servidor MariaDB sin necesidad de mapearlo externamente.

### [Ejemplo 4: Despliegue de tomcat + nginx](https://github.com/josedom24/curso_docker_ies/blob/main/modulo3/tomcat.md)

Este ejemplo muestra el despliegue de una aplicación Java en Tomcat con un proxy inverso Nginx usando Docker. Los pasos principales son:

1. Crear una red bridge:

   ```bash
   docker network create red_tomcat
   ```

2. Desplegar Tomcat:

   ```bash
   docker run -d --name aplicacionjava \
                --network red_tomcat \
                -v /home/vagrant/tomcat/sample.war:/usr/local/tomcat/webapps/sample.war:ro \
                tomcat:9.0
   ```

3. Configuración de Nginx:

   ```nginx
   server {
       listen       80;
       listen  [::]:80;
       server_name  localhost;

       location / {
           root   /usr/share/nginx/html;
           proxy_pass http://aplicacionjava:8080/sample/;
       }
       error_page   500 502 503 504  /50x.html;
       location = /50x.html {
           root   /usr/share/nginx/html;
       }
   }
   ```

4. Desplegar Nginx como proxy inverso:

   ```bash
   docker run -d --name proxy \
                -p 80:80 \
                --network red_tomcat \
                -v /home/vagrant/tomcat/default.conf:/etc/nginx/conf.d/default.conf:ro \
                nginx
   ```

![Example 3](/docker/.imgs/Act-4/Fig3.png)

