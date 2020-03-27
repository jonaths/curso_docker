# descargar una version de ubuntu
FROM ubuntu:12.04
# actualizar el sistema
RUN apt-get update
# instalar nginx (es importante e. -y)
RUN apt-get install -y nginx
# creamos un archivo index.html en el directorio por defecto de nginx
# RUN echo 'Mi primer Dockerfile' > /usr/share/nginx/www/index.html
# ponemos el contenido de una carpeta en la imagen
# ADD original /usr/share/nginx/www/
# usar volume
VOLUME /usr/share/nginx/www/
ADD original /usr/share/nginx/www/
# arrancamos nginx a traves de ENTRYPOINT para que no pueda ser modificado
# al crear el contenedor
ENTRYPOINT ["/usr/sbin/nginx", "-g", "daemon off;"]
# exponemos el puerto 80
EXPOSE 80
