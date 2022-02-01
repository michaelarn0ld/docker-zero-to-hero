# COPY allows us to copy files from our machine (host) to the image we build
# ADD allows us to the exact same thing except we can use files from the web
FROM centos:8

RUN echo '8-stream' > /etc/yum/vars/releasever

RUN yum -y install httpd unzip

ENV GITLAB_USER "michaelarn0ld"

# Get the file from this public repo, copy the zip file to /var/www/html/code.zip directory
# Encapsulate variables in quotes to prevent word splitting!
ADD https://gitlab.com/"$GITLAB_USER"/website/-/archive/main/website-main.zip /var/www/html/code.zip 

# We need to unzip the zipped files to make them servable!
RUN cd /var/www/html && unzip code.zip && mv website-main/* ./ && echo $GITLAB_USER > env.conf

CMD apachectl -DFOREGROUND
