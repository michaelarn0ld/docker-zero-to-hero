FROM centos:8

RUN echo '8-stream' > /etc/yum/vars/releasever

RUN yum -y install httpd

# When you create an instance of this image, be sure to expose a port
# docker build --tag image_name:version .
# docker run -d -p 9090:80 image_name:version
CMD apachectl -DFOREGROUND
