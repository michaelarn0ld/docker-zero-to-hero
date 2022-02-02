FROM centos:8

# LABEL gives us metadata; not used to actually build technical aspects of image
# No functionality
LABEL maintainer=Michael
LABEL vendor=companyB

RUN echo '8-stream' > /etc/yum/vars/releasever

RUN yum -y install httpd unzip

ENV GITLAB_USER "michaelarn0ld"

WORKDIR /var/www/html/

ADD https://gitlab.com/"$GITLAB_USER"/website/-/archive/main/website-main.zip ./code.zip 

RUN unzip code.zip && mv website-main/* ./ && echo $GITLAB_USER > env.conf

CMD apachectl -DFOREGROUND
