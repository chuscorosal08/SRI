# Actividad #3 - Docker: Gestión de Imágenes y Contenedores

## Ejercicio

### 1. Lee los siguientes artículos

* [Pull docker images & run docker containers](http://www.servermom.org/pull-docker-images-run-docker-containers/3225/)
* [Borrar imágenes y contenedores Docker](https://www.tecmint.com/remove-docker-images-containers-and-volumes/)
* [Dar nombre a contenedores docker](https://www.tecmint.com/name-docker-containers/)

### Recomendación

* Se recomienda hacer previamente el ejercicio: [https://training.play-with-docker.com/ops-s1-hello/](https://training.play-with-docker.com/ops-s1-hello/)

### Pasos

1. **Descarga la imagen de ubuntu:**

    ```bash
    docker pull ubuntu
    ```

2. **Descarga la imagen de hello-world:**

    ```bash
    docker pull hello-world
    ```

3. **Descarga la imagen nginx:**

    ```bash
    docker pull nginx
    ```

4. **Muestra un listado de todas las imágenes:**

    ```bash
    docker images
    ```

    ![Steps 1-4](/Docker/.imgs/Act-3/Fig1.png)

5. **Ejecuta un contenedor hello-world y dale nombre "myhello1":**

    ```bash
    docker run --name myhello1 hello-world
    ```
  
    ![Step 5](/Docker/.imgs/Act-3/Fig2.png)

6. **Ejecuta un contenedor hello-world y dale nombre "myhello2":**

    ```bash
    docker run --name myhello2 hello-world
    ```

    ![Step 6](/Docker/.imgs/Act-3/Fig3.png)

7. **Ejecuta un contenedor hello-world y dale nombre "myhello3":**

    ```bash
    docker run --name myhello3 hello-world
    ```

    ![Step 7](/Docker/.imgs/Act-3/Fig4.png)

8. **Muestra los contenedores que se están ejecutando:**

    ```bash
    docker ps
    ```

9. **Para el contenedor "myhello1":**

    ```bash
    docker stop myhello1
    ```

10. **Para el contenedor "myhello2":**

    ```bash
    docker stop myhello2
    ```

11. **Borra el contenedor "myhello1":**

    ```bash
    docker rm myhello1
    ```

12. **Muestra los contenedores que se están ejecutando:**

    ```bash
    docker ps -a
    ```

    ![Steps 8-12](/Docker/.imgs/Act-3/Fig5.png)

13. **Borra todos los contenedores:**

    ```bash
    docker stop $(docker ps -aq)
    docker rm $(docker ps -aq)
    ```

    ![Step 13](/Docker/.imgs/Act-3/Fig6.png)

