FROM centos:8

RUN echo '8-stream' > /etc/yum/vars/releasever; \
          yum -y install httpd php

COPY html-ssl/ /var/www/html

RUN echo "<?php phpinfo(); ?>" > /var/www/html/file.php

CMD ["apachectl", "-DFOREGROUND"]
