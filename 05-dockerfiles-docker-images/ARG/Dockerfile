FROM centos:8

# Default value for user argument if nothing is passed
# when using docker build make sure to pass --build-args user=whatever
ARG user="application"
ARG key="defualt"

RUN echo '8-stream' > /etc/yum/vars/releasever

RUN yum -y install httpd unzip

ENV GITLAB_USER "michaelarn0ld"
ENV KEY "$key"

WORKDIR /var/www/html/

ADD https://gitlab.com/"$GITLAB_USER"/website/-/archive/main/website-main.zip ./code.zip 

RUN unzip code.zip && mv website-main/* ./ && echo "$GITLAB_USER" > env.conf

# Add a user and change owner of relevant files so that user owns them
RUN useradd "$user" && chown "$user":"$user" /var/www/html -R

# Become the user
USER "$user"

# Do stuff
# RUN some commands...foo bar
RUN rm -rf *

## Go back to root directory
WORKDIR /

# Switch user back to root so we have priveleges to execute line 27
USER root

CMD apachectl -DFOREGROUND
