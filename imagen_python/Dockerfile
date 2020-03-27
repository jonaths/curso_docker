FROM ubuntu
RUN apt-get update
RUN apt-get install -y python
RUN echo 1.0 >> /etc/version && apt-get install -y git \
    && apt-get install -y iputils-ping
RUN mkdir /datos
WORKDIR /datos
RUN touch f1.txt
RUN mkdir /stats
WORKDIR /stats
RUN touch f2.txt
# copia index.html del directorio de contexto al directorio workdir
COPY index.html .
# copia app.log del directorio de contexto a /datos en el contenedor
# ignora el workdir porque tiene ruta absoluta
COPY app.log /datos
# agregar variables de ambiente
ENV dir=/data
# usando ARG
#ARG dir2
#RUN mkdir $dir2
#ARG user
#ENV user_docker $user
#ADD add_user.sh /datos
#RUN /datos/add_user.sh
# usando EXPOSE
RUN apt-get install -y apache2
EXPOSE 80 .
ADD entrypoint.sh /datos
# copia lo que haya en paginas al directorio de apache
ADD paginas /var/www/html
# similar al usar la opcion -v al crear el contenedor 
VOLUME ["www/var/html"]
CMD /datos/entrypoint.sh

# puedo llamar a variables seteadas aqui
RUN mkdir $dir
# ENTRYPOINT ["/bin/bash"]
