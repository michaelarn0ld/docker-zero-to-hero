FROM centos:8

RUN echo '8-stream' > /etc/yum/vars/releasever

RUN yum -y install httpd unzip

ENV GITLAB_USER "michaelarn0ld"

## Dockerfile hits this comand and changes all commands below to execute here
WORKDIR /var/www/html/

ADD https://gitlab.com/"$GITLAB_USER"/website/-/archive/main/website-main.zip ./code.zip 

RUN unzip code.zip && mv website-main/* ./ && echo $GITLAB_USER > env.conf

CMD apachectl -DFOREGROUND
