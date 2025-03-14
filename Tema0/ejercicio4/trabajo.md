# Usando cUrl

cUrl es una abreviatura de “Client URL”. Los comandos de cURL están diseñados para funcionar como una 
forma de verificar la conectividad a las URL y como una gran herramienta para transferir datos.

1. Obtén la página principal de un servidor web.

```
curl https://www.example.com/
```

![](/img/intro/6.png)

2. Obtén una página web de un servidor usando el puerto 443.

```
curl http://www.example.com:443/
```

![](/img/intro/7.png)

3. Obtén la página principal de un servidor FTP.

```
curl ftp://ftp.rediris.com
```

![](/img/intro/8.png)

4. Obtén una página web y guárdala en un archivo local con un nombre específico.

```
curl -o thatpage.html http://www.example.com/
```

![](/img/intro/9.png)

![](/img/intro/10.png)

5. Obtén un archivo de mensaje de un servidor FTP.

```
curl ftp://ftp.rediris.com/welcome.msg
```

![](/img/intro/11.png)
