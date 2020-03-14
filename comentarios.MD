# Apuntes de las diapositivas

El curso de Udemy es https://www.udemy.com/course/aprende-docker-desde-cero

## Seccion 1

### 3 Introduccion a Docker
- Encapsula todas las dependencias de un proyecto de manera transparente.
- Actualmente va en la versión 18? Checar.
    • La idea viene de los contenedores de barcos. Una caja de determinadas medidas pero no me importa lo que hay adentro.

### 4 Diferencias con máquinas virtuales
- Los contenedores comparten recursos binarios y libs con el sitema operativo host.

![Diferencias entre una maquina virtual y un contenedor](imgs/difs.jpg)

- Hay dos versiones:
  - Community (CE)
    - Stable: para producción
    - Edge: lo más moderno
  - Enterprise (EE)
    - Basic
    - Standard
    - Advance
- Los sistemas operativos de Community Edition funciona con Windows y Ubuntu. En plataformas cloud hay para AWS, Azure e IBM Cloud.

### 6. Instrucciones de instalación
- Aquí estan las instrucciones para Ubuntu: https://docs.docker.com/install/linux/docker-ce/ubuntu/
- Instale la version 19.03

### 14 Arquitectura de Docker. Imagenes, Containers y Daemons
- dockerd: es el daemon que puede ser visto desde el manedor de procesos.
- Docker client: la manera de interactuar con Docker. En este caso el cli. Se conecta al dockerd para decirle que hacer.
- Imagen: es una plantilla de un determinado contenedor. Puede tener por ejemplo Ubuntu, Python y Git.
- Container: se crean a partir de una imagen. Una imagen seria la clase por ejemplo y un contenedor las instancias de la clase.

![Elementos de Docker](imgs/conceptos.png)

## Seccion 2

### 15 Arrancar y parar docker
- Para iniciar, detener y checar el status.
```
$ systemctl start docker
$ systemctl status docker
$ systemctl stop docker
```
- Ademas de arrancarlo debemos activar docker. Esto significa que al iniciar o reiniciar el sistema docker se va a reiniciar tambien.
```
$ systemctl enable docker
```
- Tambien se puede con `service start docker` por ejemplo. Este es el comando anterior. Ahora funciona con systemctl.

### 17 Crear nuestro primer contenedor
- Comandos importantes
  - `docker -v`
  - `sudo docker info`: obtener información de memoria y de contenedores funcionando.
- Al correr  `docker run hello-world` docker busca una plantilla local que se llame asi, si no la encuentra la busca en docker hub. Entonces las descarga y crea un contenedor basado en esa plantilla.

### 18 Ver imagenes y contenedores Parte 1
- `sudo docker images` para mostrar las imagenes en local.
- Al correr un contenedor este no se elimina. Se queda con status parado.
- `sudo docker ps` muestra los contenedores. Si agregamos `sudo docker ps -a` tenemos los contenedores inactivos. Se puede tener mas de un contenedor de una misma imagen.

### 19 Ver imagenes y contenedores Parte 2
- `docker ps --help` para ayuda sobre los contenedores existentes.
- `docker ps -l` el  ultimo contenedor.
- `docker -n 3` muestra los ultimos 3 por ejemplo.

### 21 Crear un contenedor interactivo
- En modo interactivo se pone el prompt root@296e28b4d8ab. El segundo numero es el id del contenedor.
- Cuando se corre el comando run se crea un contenedor nuevo. Para reutilizar uno que ya existe hay que usar un comando como `docker start -i 296e28b4d8ab`.

### 23 Crear un contenedor en background
- `docker run -d nginx` corre un contenedor con nginx en modo dettached. Por eso es la -d.

### 27 Docker Hub Parte 1: tags y pulls
- Aqui estan las imagenes que se descargan al darle docker run.
- Al darle docker ps aparece el nombre del repositorio en la primera columna, el tag es la version del repositorio que es realmente la imagen.
- Si no le digo cual etiqueta busca la que dice latest.
- `sudo docker pull ubuntu:trusty` lo que hace es descargar la imagen pero no crea el contenedor. ubuntu:trusty quiere decir que baje la etiqueta trusty del repositorio ubuntu.

### 32 Borrar imagenes y contenedores
- `sudo docker rm 296e28b4d8ab` elimina el contendor con ese id.
- `sudo docker rm 296e28b4d8ab` elimina el contenedor con el nombre interesting_yonath.
- `sudo docker rmi 6d5fcfe5ff17` elimina una imagen con ese id.
- `sudo docker rmi -f 6d5fcfe5ff17` elimina la imagen y los contenedores que la usen tambien.

### 33 Docker Exec: ejecutar comandos contra contenedores
- `docker -run -it --name mi_ubuntu ubuntu bash` crea de manera interactiva un contenedor basado en la imagen ubuntu y el nombre mi_ubuntu y entra a la bash.
- `docker exec mi_ubuntu echo hola` corre el comando echo hola en el contenedor mi_ubuntu.
- `exec` es mas para tareas administrativas que para poner algo en produccion.
- `docker exec nginx1 /bin/sh -c 'for i in *; do echo "ficheroi -->" $i ; done'` para correr un script bash en un contendor que ya funciona.

### 35 Comandos Docker Image y Docker Container
- `docker image ls`, `docker image rm` son como subcomandos de imagenes de docker.
- `docker container ls`, `docker container rm` igual, son subcomandos de contenedores de docker.
- `docker container ls --help` para mostrar la ayuda del comando. Funciona tambien para imagenes.

### 36 Docker logs y docker kill
- Cuando se corre un comando en el modo -d no se ve en la pantalla lo que hace, a veces seria util poder hacerlo.
- `sudo docker run -d ubuntu sh -c "while true; do date; done"` corre un contenedor y un comando, para poder ver que se imprimio en la pantalla uso logs `sudo docker logs 2afc222fe419 --tail 10
`. Enmedio va el id del contenedor.  
- Puedo terminar el contenedor con `sudo docker kill 2afc222fe419`.

### 37 Docker top, docker stats
- `sudo docker top fbc1` muestra los procesos dentro de un contenedor.
- `sudo dockker stats fbc1` muestra el consumo de recursos de un contenedor.   

### 39 Docker inspect
- `docker inspect fbc1 > container.txt` proporciona información técnica (propiedades) del contenedor y lo guarda en un archivo.
- `dicker inspect ubuntu:latest > image.txt` proporciona información de la imagen.

## Seccion 3

### 41 Introduccion a los puertos en docker

- Por defecto los puertos en docker son privados.
- Debemos hacerlos publicos y mapearlos a un puerto del host donde corre el contenedor.

![Mapear puertos en Docker](imgs/conceptos.png)

### 42 Gestionar puertos para acceder al contenedor
- `sudo docker run -d -P nginx` mapea todos los puertos del contenedor al host.
- `docker run -d --name nginx_2 -p 8080:80 nginx` accedo al puerto 80 del contenedor por el puerto 8080 de la maquina fisica.

### 44 Redes en docker
- `docker network ls` muestra las redes.
  - bridge es la red que utiliza docker por default.
  - los contenedores que pertenecen a la red host no se pueden ver entre si.
  - red none es un contenedor que no tiene red.
- A las redes de tipo bridge se le van agregando los contenedores. Las IP son consecutivas dentro de docker.
- ¿Como sabemos a que red pertenece un contenedor? `docker inspect nginx_3 > nginx_2.txt`. Dentro de NetworkSettings aparece bridge, tambien el mapeo de puertos.

### 44 Inspeccionar una red
- `docker network inspect bridge` para analizar la red bridge. Se muestra la subred y el rango de ips, ademas de los contenedores asociados a esta red.
- Si yo quiero saber que contenedores estan en una red lo mejor es verlo directamente en la red en lugar de mirar contenedor por contenedor.
- `sudo docker port mongo_2` con este comando miramos los puertos por los que un contenedor escucha.

### 46. Crear una nueva red
- La red bridge que se crea automaticamente con Docker tiene algunas limitaciones.
- `docker network create --help` para mostrar la ayuda.
- `nmcli con` nos dice las conexiones que tenemos dentro del sistema operativo?

### 47. Asociar contenedores a una red
- `sudo docker run -it --name ubuntua --network red1 ubuntu` corre un contenedor al que llama ubuntua a partir de la imagen ubuntu en la red1.
- `docker network connect red2 ubuntua` conecta a la red2 el contenedor ubuntua. No lo quita de las redes a las que este asociado anteriormente.
- `docker network disconnect red2 ubuntua` lo desconecta.

### 49. Enlazar contenedores con --link
- No se recomienda usar `--link` sino contenedores personalizados.
- `sudo docker rm 'sudo docker ps -aq'` (el acento es grave, no comilla) usar **con cuidado**. Borra todos los contenedores.
- `docker run -it --rm --name b1 busybox`: el `--rm` borra el contenedor cuando termina de ejecutarse.
- `sudo docker run -it --rm --name b3 --link b1:maquina1 busybox` enlaza la maquina b3 con la b1 pero de manera unidireccional. `--link` no hace que b1 *vea* a b3. Tendria que usar la IP.  

### 51. Enlazar contenedores en redes personalizadas
- `docker run -d --name mysqlserver --rm --network red1 -e MYSQL_ROOT_PASSWORD=secret mysql` crea un contenedor con mysql y lo asigna a la red1. `MYSQL_ROOT_PASSWORD` es requerido por el contenedor como contraseña de la base de datos. No le puse el puerto porque no quiero acceder desde la maquina host, sino desde otro contenedor.
- `-e` es una variable de entorno.
- `docker exec -it mysqlserver bash` para correr el contenedor con mysql. Luego `mysql -u root -p` para abrir el servidor mysql.
- `docker run -it --name mysql_client --rm --network red1 mysql bash` corre un contenedor como cliente. Luego `mysql -h mysqlserver -u root -p` para conectarte.
- Al crear una red personalizada Docker crea un DNS, asi no tengo que teclear IPs. Asi accedi usando `-h mysqlserver`.

### Ejemplo de enlazar contenedores. Wordpress y MySQL

![Diagrama de conexiones](imgs/wp-mysql.png)

- `docker run -d --name wp --rm --network red 1 -e WORDPRESS_DN_HOST=mysql_wp -e WORDPRESS_DB_PASSWORD=secret wordpress` para crear un contenedor wordpress. 
