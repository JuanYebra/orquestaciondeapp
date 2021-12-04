#Establece la img base
FROM php:8.0.11-apache

#Descarga los paquetes disponibles y se actualizan en la ultima version.
RUN apt update && apt upgrade -y

#Especificamos la zona horaria e instalamos tzdata y supervisor
RUN DEBIAN_FRONTEND="noninteractive" TZ="America/New_York" apt install -y tzdata supervisor

#Configura root de documentos
#Crea el directorio
RUN mkdir /var/www/CRUD
#Cambia el propietario del directorio
RUN chown -R $USER:$USER /var/www/CRUD

#Crea un nuevo virtualhost en el servidor web
COPY apache.conf.j2 /etc/apache2/sites-available/CRUD.conf

#Habilita el virtualhost
RUN a2ensite CRUD.conf

#copiamos app
RUN mkdir /var/www/html/CRUD
COPY ./CRUD /var/www/html/CRUD

#Copia el archivo 000-default.conf para que cargue la pagina de la app en lugar de apache
COPY 000-default.conf /etc/apache2/sites-enabled/000-default.conf

#Copia el archivo supervisor.conf
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

#Expone el puerto
EXPOSE 80

#Pasa valores por defecto al contenedor
CMD ["/usr/bin/supervisord"]