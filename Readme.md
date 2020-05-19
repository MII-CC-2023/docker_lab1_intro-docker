# Guía 1: Introducción a Docker 
(https://docs.docker.com/engine/reference/commandline/cli/)

## 1. Instalación de Docker CE en Ubuntu

a) Crear una máquina virtual en GCP con Ubuntu.

b) Actualiza el índice del gestor de paquetes APT
```
$ sudo apt-get update
```
c) Instala algunos paquetes necesarios, si aún no están instalados
```
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
d) Añade la key oficial para el repositorio de Docker
```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
e) Añade el repositorio con la versión deseada
```
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
f) Instala la última versión estable de Docker
```
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```
g) Post-instalación
Añade tu usuario, que está registrado en la variable de entorno $USER.
```
$ sudo usermod -aG docker $USER
```
Es necesario volver a entrar al sistema para que los cambios tengan efecto.


Comprueba la versión de Docker instalada
```
$ docker --version
Docker version 19.03.8, build afacb8b7f0
```
Muestra la información de Docker
```
$ docker info
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 19.03.8
 Storage Driver: overlay2
  Backing Filesystem: <unknown>
  Supports d_type: true
  Native Overlay Diff: true
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc version: dc9208a3303feef5b3839f4323d9beb36df0a9dd
 init version: fec3683
 Security Options:
  apparmor
  seccomp
   Profile: default
 Kernel Version: 4.15.0-1065-aws
 Operating System: Ubuntu 18.04.4 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 1
 Total Memory: 983.7MiB
 Name: ip-172-31-33-201
 ID: BAJL:BHAG:KGR6:ER3B:WC3S:7TPT:XUHG:QE64:PTUK:PCQU:SLN2:HEQI
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
```

También podemos ver la versión tanto del cliente como del servidor
```
$ docker version
Client: Docker Engine - Community
 Version:           19.03.8
 API version:       1.40
 Go version:        go1.12.17
 Git commit:        afacb8b7f0
 Built:             Wed Mar 11 01:25:46 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.8
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.17
  Git commit:       afacb8b7f0
  Built:            Wed Mar 11 01:24:19 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

Ahora podemos ejecutar contenedores utilizando imáges desde Docker hub:
```
$ docker run -it ubuntu:18.04 bash
Unable to find image 'ubuntu:18.04' locally
18.04: Pulling from library/ubuntu
23884877105a: Pull complete 
bc38caa0f5b9: Pull complete 
2910811b6c42: Pull complete 
36505266dcc6: Pull complete 
Digest: sha256:3235326357dfb65f1781dbc4df3b834546d8bf914e82cce58e6e6b676e23ce8f
Status: Downloaded newer image for ubuntu:18.04
root@7bd71a3f1fa6:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@7bd71a3f1fa6:/#
```


## 2. Imágenes y Docker hub

a) Buscar en el Docker Hub (https://hub.docker.com/) la imagen PHP
 
Verás que dispone de varias imágenes etiquetadas con Apache
 
b) Extraer la imagen php:apache desde el Docker Hub
```
$ docker pull php:apache
apache: Pulling from library/php
27833a3ba0a5: Pull complete
2d79f6773a3c: Pull complete
f5dd9a448b82: Pull complete
95719e57e42b: Pull complete
cc75e951030f: Pull complete
78873f480bce: Pull complete
1b14116a29a2: Pull complete
95836a0750ea: Pull complete
7f419f7492e4: Pull complete
579567332cdb: Pull complete
9cc8d2923fb7: Pull complete
8dd306eba19f: Pull complete
329ff9bebb9e: Pull complete
Digest: sha256:df1b70df7eadbd94fee6432bfaba40ce54edc72fc9d4d0239780f294ae03c038
Status: Downloaded newer image for php:apache
```
c) Listar las imágenes locales
```
$ docker image ls
REPOSITORY      TAG              IMAGE ID         CREATED          SIZE
php             apache           1dffbbe4a5d3     13 days ago      378MB
```
d) Borrar una imagen
```
$ docker image rm php:apache
Untagged: php:apache
Untagged: ...
```

## 3. Contenedores.

a) Ejecutar la imagen en un contenedor (docker run)
```
$ docker run -d -it --name front-end -p 80:80 php:apache
```
Donde:

-d: (detach). Ejecución en segundo plano

-i: (interactive): Ejecución interactiva

-t: (Terminal): Ejecución en un terminal

--name nombre: asigna nombre al contenedor

-p p1:p2: Mapea el puerto p1 del host al puerto p2 del contenedor

httpd: imagen a ejecutar. Si no está disponible la obtiene del Docker hub

Comprueba que desde la IP de la máquina virtual accedes al servidor Apache del contenedor

b) Mostar listado de contenedores en ejecución
```
$ docker container ls
```
c) Parar un contenedor en ejecución
```
$ docker stop front-end
```
d) Listar de todos los contenedores
```
$ docker container ls -a
```
e) Reiniciar el contenedor
```
$ docker start front-end
```
f) Ejecutar un comando, en este caso la Shell bash, en el contenedor en ejecución
```
$ docker exec -it front-end bash
```

También, de forma alternativa, puedes ejecutar un comando sobre un contenedor al iniciarlo con
```
$ docker run -dit --name front-end -p 80:80 php:apache bash
```
En la Shell, crea un fichero index.html ejecutando:
```
  # echo 'Hola Mundo' > index.html
```
Comprueba desde el navegador los cambios 

g) Eliminar un contenedor 
```
$ docker stop front-end
$ docker container rm front-end
```

h) Asignar un volumen persistente, en este caso el directorio actual, al contenedor
```
$ docker run -d --name front-end -p 80:80 -v "$PWD":/var/www/html php:apache 
```

Incluir un fichero index.php con el contenido siguiente en el directorio actual de la máquina virtual y comprueba los resultados desde el navegador
```php
<h1>Mi WebApp</h1>
<?php
  echo "<h2>con PHP</h2>"
?>
```
Puedes eliminar un contenedor en ejecución con:
```
$ docker container rm -f front-end
```

i) Conectar (link) un contenedor con otro

Ejecutar un nuevo contenedor con mySQL; para ello, buscar en el Docker Hub la imagen MySQL para obtener más información. Como podrás ver en la documentación, ejecuta el contenedor con:
```
$ docker run -d --name database -e MYSQL_DATABASE=bd -e MYSQL_USER=web -e MYSQL_PASSWORD=web -e MYSQL_ROOT_PASSWORD=pass_root mysql:5.7
```
NOTA: En este ejemplo no estamos teniendo en cuenta la persistencia de los datos del contenedor, si deseamos hacerlo crearíamos un volumen para mapear la ruta /var/lib/mysql del contenedor.

Para comprobar los logs de un contenedor:
```
$ docker logs database 
```
Para lanzar de nuevo el contenedor PHP-Apache pero ahora conectado con la Base de datos:
```
$ docker run -d --name front-end -p 80:80 --link database -v "$PWD":/var/www/html php:apache 
```
Para instalar en el contenedor de PHP la librería para acceso a MySQL
```
$ docker exec -it front-end bash 
root@56600b9eb1c1:/var/www/html# docker-php-ext-install mysqli
root@56600b9eb1c1:/var/www/html# docker-php-ext-enable mysqli
```

Para crear una imagen a partir del contenedor:
```
$ docker commit -p front-end php:apache-mysql
```

Para subir la nueva imagen a Docker hub
```
$ docker login
$ docker tag php:apache-mysql <usuasio_docker>/php-apache-mysql-app:v1
$ docker push <usuasio_docker>/php-apache-mysql-app:v1
```
Puedes parar y borrar el contenedor front-end anterior y lanzar uno con la nueva imagen creada
```
$ docker stop front-end
$ docker rm front-end
$ docker run -d --name front-end -p 80:80 --link database -v "$PWD":/var/www/html <usuasio_docker>/php-apache-app:v1
```
Modificar el fichero index.php con el siguiente contenido
```php
<h1>Mi WebApp</h1>
<?php
  echo "<h2>con PHP</h2>"
?>
<?php
$servername = "database";
$username = "web";
$password = "web";

// Create connection
$conn = new mysqli($servername, $username, $password);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}
echo "Connected successfully";
?>
```

j) En esta guía, hemos trabajado con la red (network) por defecto (bridge), pero podríamos a ver creado nuevas redes para ubicar en ella los contenedores. Algunos comandos de interés son:

Mostrar las redes:
```
$ docker network ls 
```
Crear una red:
```
$ docker network create --driver bridge mi-red
```
Conectar un contenedor a una red:
```
$ docker network connect mi-contenedor
```
O bien, conectarlo al ejecutarlo
```
$ docker run --network=mi-red mi-imagen
```
Podemos inspeccionar las características de una red:
```
$ docker inspect mi-red
```



