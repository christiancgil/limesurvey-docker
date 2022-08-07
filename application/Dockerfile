FROM php:apache

# Instalaci�n librerias requeridas instalaci�n
RUN apt-get update && \
	apt-get install -y vim wget unzip default-mysql-client

# Instalaci�n cliente mysql
RUN docker-php-ext-install mysqli pdo pdo_mysql

# Asignar permisos carpeta de descarga medios instalaci�n
WORKDIR /tmp
RUN chmod 775 -R /tmp/

# Descarga medios de instalaci�n en carpeta de trabajo PHP
RUN wget -c https://download.limesurvey.org/latest-stable-release/limesurvey5.3.29+220801.zip && \
	ls -la && \
	unzip limesurvey5.3.29+220801.zip && \
	mv limesurvey /var/www/html/

# Asignar permisos carpeta limesurvey
WORKDIR /var/www/html/limesurvey/
RUN chown www-data:www-data -R /var/www/html/limesurvey/
RUN chmod 775 -R /var/www/html/limesurvey/

# Creaci�n de carpeta y asignaci�n de permisos gesti�n php sessions
RUN mkdir /usr/local/etc/php/writable && \
	chmod 777 -R /usr/local/etc/php/writable/

# Actualizaci�n archivo config php con carpeta de trabajo php sessions
RUN mv /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini && \
	echo "session.save_path = \"/usr/local/etc/php/writable"\" >> /usr/local/etc/php/php.ini

# Copiar archivo configuraci�n pre-editado
COPY limesurvey.conf /etc/apache2/sites-available/

EXPOSE 80