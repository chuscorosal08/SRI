# Actividad #6 - Docker: Creación de Imágenes Docker

## Ejercicio

### 1. Referencia

* Documentación del módulo cinco "Creación de imágenes Docker" del curso: [https://github.com/josedom24/curso_docker_ies](https://github.com/josedom24/curso_docker_ies)

### 2. Realiza al menos tres de los ejemplos mostrados en el módulo y documenta cada uno

A continuación, se presentan tres ejemplos de creación de imágenes Docker, basados en el módulo proporcionado:

#### [Ejemplo 1: Construcción de imágenes con una página estática](https://github.com/josedom24/curso_docker_ies/blob/main/modulo5/ejemplo1.md)

1. **Crear directorio y el archivo `index.html`:**

    ```bash
    mkdir static_web
    cd static_web
    nano index.html
    ```

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>Página Estática</title>
    </head>
    <body>
        <h1>Hola, mundo!</h1>
    </body>
    </html>
    ```

2. **Crear el `Dockerfile`:**

    ```bash
    nano Dockerfile
    ```

    ```docker
    FROM nginx:latest
    COPY index.html /usr/share/nginx/html
    ```

3. **Construir la imagen:**

    ```bash
    docker build -t static_web_image .
    ```

4. **Ejecutar el contenedor:**

    ```bash
    docker run -d -p 8080:80 static_web_image
    ```

5. **Verificar en el navegador en `http://localhost:8080`**

6. **Detener y eliminar el contenedor:**

    ```bash
    docker stop <ID_DEL_CONTENEDOR>
    docker rm <ID_DEL_CONTENEDOR>
    ```

![Example 1](/Docker/.imgs/Act-6/Fig1.png)

#### [Ejemplo 2: Construcción de imágenes con una aplicación PHP](https://github.com/josedom24/curso_docker_ies/blob/main/modulo5/ejemplo2.md)

1. **Crear directorio y el archivo `index.php`:**

    ```bash
    mkdir php_app
    cd php_app
    nano index.php
    ```

    ```php
    <?php
    echo "Hola, mundo desde PHP!";
    ?>
    ```

2. **Crear el `Dockerfile`:**

    ```bash
    nano Dockerfile
    ```

    ```docker
    FROM php:7.4-apache
    COPY index.php /var/www/html/
    ```

3. **Construir la imagen:**

    ```bash
    docker build -t php_app_image .
    ```

4. **Ejecutar el contenedor:**

    ```bash
    docker run -d -p 8080:80 php_app_image
    ```

5. **Verificar en el navegador en `http://localhost:8080`**

6. **Detener y eliminar el contenedor:**

    ```bash
    docker stop <ID_DEL_CONTENEDOR>
    docker rm <ID_DEL_CONTENEDOR>
    ```

![Example 2](/Docker/.imgs/Act-6/Fig2.png)
![Example 2](/Docker/.imgs/Act-6/Fig3.png)

#### [Ejemplo 3: Construcción de imágenes con una aplicación Python](https://github.com/josedom24/curso_docker_ies/blob/main/modulo5/ejemplo3.md)

1. **Crear directorio y los archivos `app.py` y `requirements.txt`:**

    ```bash
    mkdir python_app
    cd python_app
    nano app.py
    ```

    ```python
    from flask import Flask
    app = Flask(__name__)

    @app.route("/")
    def hello():
        return "Hola, mundo desde Python!"

    if __name__ == "__main__":
        app.run(debug=True,host='0.0.0.0')
    ```

    ```bash
    nano requirements.txt
    ```

    ```text
    Flask
    ```

2. **Crear el `Dockerfile`:**

    ```bash
    nano Dockerfile
    ```

    ```docker
    FROM python:3.8-slim-buster
    WORKDIR /app
    COPY requirements.txt .
    RUN pip install -r requirements.txt
    COPY app.py .
    EXPOSE 5000
    CMD ["python", "app.py"]
    ```

3. **Construir la imagen:**

    ```bash
    docker build -t python_app_image .
    ```

4. **Ejecutar el contenedor:**

    ```bash
    docker run -d -p 8080:5000 python_app_image
    ```

5. **Verificar en el navegador en `http://localhost:8080`**

6. **Detener y eliminar el contenedor:**

    ```bash
    docker stop <ID_DEL_CONTENEDOR>
    docker rm <ID_DEL_CONTENEDOR>
    ```

![Example 3](/Docker/.imgs/Act-6/Fig4.png)
![Example 3](/Docker/.imgs/Act-6/Fig5.png)

