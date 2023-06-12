FROM debian

RUN apt-get update && apt-get install -y apache2
#RUN chown www-data:www-data /var/lock && chown www-data:www-data /var/run && chown www-data:www-data /var/log

ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="/www-data"
ENV APACHE_RUN_GROUP="/www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

#COPY index.html /var/www/html/
# Add adicionar os arquivos tar descompactados
ADD index.html /var/www/html/

LABEL description="Webserver"
LABEL version="1.0.0"

USER root

WORKDIR /var/www/html/

VOLUME [ "/var/www/html/" ]
EXPOSE 80

#Caso não tenha ENTRYPOINT/CMD o principal é o bash
#Principal processo do container
ENTRYPOINT [ "/usr/sbin/apachectl" ]

#Passa parametros para o ENTRYPOINT
CMD [ "-D", "FOREGROUND" ]