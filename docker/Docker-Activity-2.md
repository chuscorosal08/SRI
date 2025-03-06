# Actividad #2 - Uso Básico de Docker

## Ejercicio

### 1. Lee los siguientes artículos

* [https://docs.docker.com/get-started/](https://docs.docker.com/get-started/)
* [https://docs.docker.com/get-started/part2/](https://docs.docker.com/get-started/part2/)

### 2. Lleva a cabo la práctica descrita en el primer artículo

1. **Ejecuta la imagen "hello-world"**

    ```bash
    docker run hello-world
    ```

    ![Running Hello World Image (From Activity 1)](/Docker/.imgs/Act-1/Fig3.png)

    Este comando descarga la imagen `hello-world` (si no está presente localmente) y la ejecuta en un contenedor. Deberías ver un mensaje de saludo de Docker.

2. **Muestra las imágenes Docker instaladas**

    ```bash
    docker images
    ```

    ![Docker Images](/Docker/.imgs/Act-2/Fig1.png)

    Este comando lista todas las imágenes Docker almacenadas localmente, incluyendo `hello-world`.

3. **Muestra los contenedores Docker**

    ```bash
    docker ps -a
    ```

    ![Docker ps -a](/Docker/.imgs/Act-2/Fig2.png)

    Este comando lista todos los contenedores Docker, tanto los que están en ejecución como los que están detenidos. Verás el contenedor `hello-world` que ejecutaste previamente.

### 3. Lleva a cabo la práctica descrita en el segundo artículo

1. **Edita el fichero Dockerfile**

    * Crea un directorio para tu proyecto y un archivo llamado `Dockerfile` dentro de él.
    * Edita el `Dockerfile` con instrucciones para construir tu propia imagen.

        Ejemplo de Dockerfile:

        ```docker
        # syntax=docker/dockerfile:1

        FROM node:lts-alpine
        WORKDIR /app
        COPY . .
        RUN yarn install --production
        CMD ["node", "src/index.js"]
        EXPOSE 3000
        ```

    ![Dockerfile](/Docker/.imgs/Act-2/Fig3.png)

2. **Construye el contenedor**

    ```bash
    docker build -t getting-started .
    ```

    ![Docker build](/Docker/.imgs/Act-2/Fig4.png)

3. **Ejecútalo**

    ```bash
    docker run -d -p 127.0.0.1:3000:3000 getting-started
    ```

    ![Docker run](/Docker/.imgs/Act-2/Fig5.png)

    * `-d` ejecuta el contenedor en segundo plano.
    * `-p 3000:3000` mapea el puerto 80 del contenedor al puerto 80 del host.

4. **Crea una cuenta en [hub.docker.com](http://hub.docker.com/)**

    * Si no tienes una cuenta, regístrate en [Docker Hub](http://hub.docker.com/).

5. **Publícalo**

    * Conecta con Docker Hub:

        ```bash
        docker login
        ```

    ![Docker run](/Docker/.imgs/Act-2/Fig6.png)

    * Etiqueta tu imagen:

        ```bash
        docker tag tu_usuario/nombre_imagen tu_usuario/nombre_imagen:version
        ```

        Sustituye `version` con un número de versión (ej: `1.0`).

    * Sube la imagen:

        ```bash
        docker push tu_usuario/nombre_imagen:version
        ```

## Nota

Para publicar una imagen, debes conectar previamente con Docker Hub, tal como se muestra en el siguiente artículo:

* [https://www.thegeekdiary.com/how-to-build-and-push-docker-image-to-the-docker-hub-repository/](https://www.thegeekdiary.com/how-to-build-and-push-docker-image-to-the-docker-hub-repository/)

