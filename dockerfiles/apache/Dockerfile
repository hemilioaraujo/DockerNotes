FROM ubuntu:20.04

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y apache2 && apt-get clean
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

LABEL Description="Webserver"

VOLUME /var/www/html

EXPOSE 80

# EXECUTAR O APACHE AO INICIAR O CONTAINER
ENTRYPOINT /etc/init.d/apache2 start && /bin/bash
# OU
# CMD ["apache2ctl","-D","FOREGROUND"]
