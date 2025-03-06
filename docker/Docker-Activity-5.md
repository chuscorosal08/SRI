# Actividad #5 - Docker Compose

## Ejercicio: Implementación de 3 Escenarios con Docker Compose

### 1. Referencia

* Documentación del módulo cuatro "Creando escenarios multicontenedor con Docker Compose" del curso: [https://github.com/josedom24/curso_docker_ies](https://github.com/josedom24/curso_docker_ies)

### 2. Realización de tres ejemplos del módulo

#### Ejemplo 1: Despliegue de la aplicación Temperaturas

1. Crear el fichero `docker-compose.yml`:

    ```yaml
    version: '3.1'
    services:
    frontend:
        container_name: temperaturas-frontend
        image: iesgn/temperaturas_frontend
        restart: always
        ports:
        - 8081:3000
        environment:
        TEMP_SERVER: temperaturas-backend:5000
        depends_on:
        - backend
    backend:
        container_name: temperaturas-backend
        image: iesgn/temperaturas_backend
        restart: always
    ```

    ![docker-compose.yml](/Docker/.imgs/Act-5/Fig1.png)

2. Iniciar los contenedores:

    ```bash
    docker compose up -d
    ```

3. Acceder a la aplicación en `http://localhost:8080`

4. Detener y eliminar los contenedores:

    ```bash
    docker compose down
    ```

![Example 1](/Docker/.imgs/Act-5/Fig2.png)

#### Ejemplo 2: Despliegue de Wordpress + mariadb

1. Crear el fichero `docker-compose.yml`:

    ```yaml
    version: '3.1'
    services:
    wordpress:
        image: wordpress
        restart: always
        environment:
        WORDPRESS_DB_HOST: db
        WORDPRESS_DB_USER: user_wp
        WORDPRESS_DB_PASSWORD: asdasd
        WORDPRESS_DB_NAME: bd_wp
        ports:
        - 8080:80
        volumes:
        - wordpress_data:/var/www/html
    db:
        image: mariadb
        restart: always
        environment:
        MYSQL_DATABASE: bd_wp
        MYSQL_USER: user_wp
        MYSQL_PASSWORD: asdasd
        MYSQL_ROOT_PASSWORD: asdasd
        volumes:
        - mariadb_data:/var/lib/mysql
    volumes:
        wordpress_data:
        mariadb_data:
    ```

    ![docker-compose.yml](/Docker/.imgs/Act-5/Fig3.png)

2. Iniciar los contenedores:

    ```bash
    docker compose up -d
    ```

3. Acceder a WordPress en `http://localhost:8080`

4. Detener y eliminar los contenedores (manteniendo los volúmenes):

    ```bash
    docker compose down
    ```

![Example 2](/Docker/.imgs/Act-5/Fig4.png)

#### Ejemplo 3: Despliegue de tomcat + nginx

1. Crear el fichero `docker-compose.yml`:

    ```yaml
    version: '3.1'
    services:
    aplicacionjava:
        container_name: tomcat
        image: tomcat:9.0
        restart: always
        volumes:
        - ./sample.war:/usr/local/tomcat/webapps/sample.war:ro
    proxy:
        container_name: nginx
        image: nginx
        ports:
        - 80:80
        volumes:
        - ./default.conf:/etc/nginx/conf.d/default.conf:ro
    ```

    ![docker-compose.yml](/Docker/.imgs/Act-5/Fig5.png)

2. Crear el archivo de configuración `default.conf` para Nginx:

    ```nginx
    server {
        listen       80;
        listen  [::]:80;
        server_name  localhost;

        location / {
            proxy_pass http://tomcat:8080/sample/;
        }
    }
    ```

3. Descargar el archivo `sample.war`:

    ```bash
    wget https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/sample.war
    ```

4. Iniciar los contenedores:

    ```bash
    docker compose up -d
    ```

5. Acceder a la aplicación en `http://localhost`

6. Detener y eliminar los contenedores:

    ```bash
    docker compose down
    ```

![Example 3](/Docker/.imgs/Act-5/Fig6.png)

