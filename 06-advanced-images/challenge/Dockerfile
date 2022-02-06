FROM debian

RUN \
    apt update -y && \
    apt install apache2 php php-fpm -y && \
    mkdir /run/php/ && \
    a2enmod alias proxy proxy_fcgi

COPY fpm.conf /etc/apache2/sites-available/000-default.conf
COPY info.php /var/www/html/info.php
COPY start.sh /start.sh

RUN service apache2 restart && chmod +x start.sh

CMD ./start.sh
