# Lab 1: Introducción a Docker 
(https://docs.docker.com/engine/reference/commandline/cli/)


Más información para instalación en otras plataformas: https://docs.docker.com/engine/install/ 

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
    gnupg \
    lsb-release
```
d) Añade la key oficial para el repositorio de Docker
```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
e) Añade el repositorio con la versión deseada
```
$ echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
f) Instala la última versión estable de Docker
```
$ sudo apt-get update

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
Docker version 20.10.14, build a224086
```
Muestra la información de Docker
```
$ docker info
Client:
 Context:    default
 Debug Mode: false
 Plugins:
  app: Docker App (Docker Inc., v0.9.1-beta3)
  buildx: Build with BuildKit (Docker Inc., v0.5.1-docker)
  scan: Docker Scan (Docker Inc., v0.7.0)

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 20.10.14
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 05f951a3781f4f2c1911b05e61c160e9c30eaa8e
 runc version: 12644e614e25b05da6fd08a38ffa0cfe1903fdec
 init version: de40ad0
 Security Options:
  apparmor
  seccomp
   Profile: default
 Kernel Version: 5.4.0-1045-aws
 Operating System: Ubuntu 18.04.5 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 1
 Total Memory: 978.6MiB
 Name: ip-172-31-20-222
 ID: RPUM:VTFA:XVZO:P5YH:PXKO:ZQYD:3T6L:CMUT:UEBW:CL37:CMOJ:STJI
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false

```

También podemos ver la versión tanto del cliente como del servidor
```
$ docker version
Client: Docker Engine - Community
 Version:           20.10.14
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        370c289
 Built:             Fri Apr  9 22:46:01 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.14
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       8728dd2
  Built:            Fri Apr  9 22:44:13 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.4
  GitCommit:        05f951a3781f4f2c1911b05e61c160e9c30eaa8e
 runc:
  Version:          1.0.0-rc93
  GitCommit:        12644e614e25b05da6fd08a38ffa0cfe1903fdec
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

```

Ayuda comando docker:

```
$ docker 
Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default
                           "C:\\Users\\alvarez\\.docker")
  -c, --context string     Name of the context to use to connect to the
                           daemon (overrides DOCKER_HOST env var and
                           default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level
                           ("debug"|"info"|"warn"|"error"|"fatal")
                           (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default
                           "C:\\Users\\alvarez\\.docker\\ca.pem")
      --tlscert string     Path to TLS certificate file (default
                           "C:\\Users\\alvarez\\.docker\\cert.pem")
      --tlskey string      Path to TLS key file (default
                           "C:\\Users\\alvarez\\.docker\\key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  app*        Docker App (Docker Inc., v0.9.1-beta3)
  builder     Manage builds
  buildx*     Build with BuildKit (Docker Inc., v0.5.1-docker)
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  image       Manage images
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  scan*       Docker Scan (Docker Inc., v0.6.0)
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.

To get more help with docker, check out our guides at https://docs.docker.com/go/guides/

```

Ahora podemos ejecutar contenedores utilizando imáges desde Docker hub:
```
$ docker container run -it ubuntu:18.04 bash
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
$ docker image pull php:apache
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
$ docker container run -d -it --name front-end -p 80:80 php:apache
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
$ docker container stop front-end
```
d) Listar de todos los contenedores
```
$ docker container ls -a
```
e) Reiniciar el contenedor
```
$ docker container start front-end
```
f) Ejecutar un comando, en este caso la Shell bash, en el contenedor en ejecución
```
$ docker container exec -it front-end bash
```

También, de forma alternativa, puedes ejecutar un comando sobre un contenedor al iniciarlo con
```
$ docker container run -dit --name front-end -p 80:80 php:apache bash
```
En la Shell, crea un fichero index.html ejecutando:
```
  # echo 'Hola Mundo' > index.html
```
Comprueba desde el navegador los cambios 

g) Eliminar un contenedor 
```
$ docker container stop front-end
$ docker container rm front-end
```

h) Asignar un volumen persistente, en este caso el directorio actual, al contenedor
```
$ docker container run -d --name front-end -p 80:80 -v "$PWD":/var/www/html php:apache 
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
$ docker container logs database 
```
Para lanzar de nuevo el contenedor PHP-Apache pero ahora conectado con la Base de datos:
```
$ docker run -d --name front-end -p 80:80 --link database -v "$PWD":/var/www/html php:apache 
```
Para instalar en el contenedor de PHP la librería para acceso a MySQL
```
$ docker container exec -it front-end bash 
root@56600b9eb1c1:/var/www/html# docker-php-ext-install mysqli
root@56600b9eb1c1:/var/www/html# docker-php-ext-enable mysqli
```

Para crear una imagen a partir del contenedor:
```
$ docker container commit -p front-end php:apache-mysql
```

Para subir la nueva imagen a Docker hub
```
$ docker login
$ docker image tag php:apache-mysql <usuasio_docker>/php-apache-mysql-app:v1
$ docker image push <usuasio_docker>/php-apache-mysql-app:v1
```
Puedes parar y borrar el contenedor front-end anterior y lanzar uno con la nueva imagen creada
```
$ docker container stop front-end
$ docker container rm front-end
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



