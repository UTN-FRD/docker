# Instalación de Docker Desktop
Instalar docker desktop:
   https://www.docker.com/products/docker-desktop/

# Correr el primer contenedor
Abrir PowerShell y ejecutar el siguiente comando:

```
> docker run hello-world
```

En la terminal se mostrará el siguiente mensaje, el cual explico en partes:

Lo primero que muestra es que no encuentra la imagen 'hello-world:latest' localmente, esto es porque no se ha descargado en el sistema operativo. En este caso, lo que hace docker es buscar la imagen en el repositorio de Docker Hub, que es una plataforma de contenedores que aloja millones de imágenes de contenedores.

```
Unable to find image 'hello-world:latest' locally
```

Luego muestra que esta descargando la imagen de Docker Hub, en este caso es una imagen muy pequeña que solo contiene un mensaje de bienvenida.

```
latest: Pulling from library/hello-world
c1ec31eb5944: Pull complete
Digest: sha256:d211f485f2dd1dee407a80973c8f129f00d54604d2c90732e8e320e5038a0348
Status: Downloaded newer image for hello-world:latest
```

Luego muestra un mensaje desde el Docker:
```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

Lo que sucedió es que docker creó un contenedor a partir de la imagen "hello-world" y lo ejecutó. Este contenedor se creó con el comando "docker run hello-world". 
El contenedor, después de ejecutarse, se detuvo automáticamente, ya que la imagen "hello-world" no contiene ningún proceso que se pueda mantener activo indefinidamente.

# Explorar ambiente local
Podemos ver los contenedores que están corriendo con el siguiente comando:
```
> docker ps
```
Lo cual nos mostrará un output como el siguiente:
```
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

Allí vemos que no hay contenedores corriendo. Para ver los contenedores que se han corrido, se puede usar el siguiente comando:
```
> docker ps -a
```
Lo cual nos mostrará un output como el siguiente:
```
CONTAINER ID   IMAGE                                    COMMAND                  CREATED          STATUS                      PORTS                               NAMES
892705b1531b   hello-world                              "/hello"                 16 seconds ago   Exited (0) 15 seconds ago                                       competent_shtern
```

# Levantar Linux en un contenedor
Ahora vamos a correr un contenedor con un Ubuntu preinstalado, la cual se puede usar como sistema operativo en un contenedor. Para esto usaremos el comando:
```
> docker run -it ubuntu bash
```

Esto nos mostrará un output como el siguiente:

```
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
ff65ddf9395b: Pull complete
Digest: sha256:d4f6f70979d0758d7a6f81e34a61195677f4f4fa576eaf808b79f17499fd93d1
Status: Downloaded newer image for ubuntu:latest
```

Con el parámetro -it se indica que el contenedor se ejecute en modo interactivo, y con el parámetro bash se indica que se ejecute el comando bash, que es el shell por defecto en Ubuntu.

Esto dejará el prompt del sistema operativo Ubuntu listo para usar. Aquí estamos dentro del contenedor, y podemos ejecutar comandos del sistema operativo Ubuntu. 

Por ejemplo, podemos listar los archivos en el directorio actual con el comando:

```
root@da0464e67235:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

Podemos salir del contenedor con el comando:
```
root@da0464e67235:/# exit
```
o simplemente presionando `Ctrl+D`

Si volvemos a correr el comando `docker run -it ubuntu bash`, veremos que se crea un nuevo contenedor, ya que el anterior se detuvo, pero en este caso no se volverá a descargar la imagen de Ubuntu, ya que la misma ya se encuentra en el sistema operativo local.

```
> docker run -it ubuntu bash
root@65fac46cfc84:/#
```

Abrimos una nueva terminal y ejecutamos el comando `docker ps`, lo cual nos mostrará un output como el siguiente:

```
> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES
65fac46cfc84   ubuntu    "bash"    55 seconds ago   Up 54 seconds             sharp_carson
```

Si ejecutamos el comando `docker ps -a` vamos a ver los contenedores que se han corrido.

```
> docker ps -a
CONTAINER ID   IMAGE                                    COMMAND                  CREATED             STATUS                         PORTS                               NAMES
65fac46cfc84   ubuntu                                   "bash"                   5 minutes ago       Up 5 minutes                                                       sharp_carson
da0464e67235   ubuntu                                   "bash"                   10 minutes ago      Exited (0) 6 minutes ago                                           boring_grothendieck
892705b1531b   hello-world                              "/hello"                 About an hour ago   Exited (0) About an hour ago                                       competent_shtern
```

# Imágenes locales
Para ver las imágenes que tenemos descargadas, podemos usar el comando:

```
> docker images
REPOSITORY                               TAG          IMAGE ID       CREATED         SIZE
ubuntu                                   latest       59ab366372d5   10 minutes ago 78.1MB
hello-world                              latest       d2c94e258dcb   1 day ago      13.3kB
```

# Docker hub
Explorar los contenedores disponibles en Docker Hub: https://hub.docker.com/search 

Por ejemplo, para correr un contenedor con Nginx, y levantar el servicio en el puerto 80 de nuestra máquina, podemos usar el comando:
```
> docker run -p 80:80 nginx
```

Esto descargará la imagen de Nginx desde Docker Hub, y la ejecutará en un contenedor. El parámetro -p 80:80 indica que el contenedor escuchará en el puerto 80, y nuestro sistema operativo lo mapeará al puerto 80 de la máquina host.

También se puede usar el parámetro -d para que el contenedor se ejecute en segundo plano.
El parámetro -name se puede usar para asignarle un nombre al contenedor.

La salida de comando anterior nos mostrará un output como el siguiente:
```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/10/18 01:16:55 [notice] 1#1: using the "epoll" event method
2024/10/18 01:16:55 [notice] 1#1: nginx/1.23.4
2024/10/18 01:16:55 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6)
2024/10/18 01:16:55 [notice] 1#1: OS: Linux 5.10.16.3-microsoft-standard-WSL2
2024/10/18 01:16:55 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2024/10/18 01:16:55 [notice] 1#1: start worker processes
2024/10/18 01:16:55 [notice] 1#1: start worker process 29
2024/10/18 01:16:55 [notice] 1#1: start worker process 30
2024/10/18 01:16:55 [notice] 1#1: start worker process 31
2024/10/18 01:16:55 [notice] 1#1: start worker process 32
2024/10/18 01:16:55 [notice] 1#1: start worker process 33
2024/10/18 01:16:55 [notice] 1#1: start worker process 34
2024/10/18 01:16:55 [notice] 1#1: start worker process 35
2024/10/18 01:16:55 [notice] 1#1: start worker process 36
```

Para detener un contenedor, se puede usar el comando `docker stop <id_del_contenedor>` o `docker kill <id_del_contenedor>`.

Para eliminar un contenedor, se puede usar el comando `docker rm <id_del_contenedor>`.

Para eliminar una imagen, se puede usar el comando `docker rmi <id_de_la_imagen>`.  

