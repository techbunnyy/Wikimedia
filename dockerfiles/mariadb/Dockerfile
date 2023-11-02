# Using CentOS 8 image
FROM centos:8

#Repos are not added by default in the image , hence adding repos as follows
#start Pre-Config
RUN cd /etc/yum.repos.d/
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

RUN yum -y install java

CMD /bin/bash

# Need to install MariaDB first
RUN yum -y update && yum -y install mariadb-server

# Staring MariaDB services
CMD ["/usr/sbin/mariadb", "-D", "FOREGROUND"]

EXPOSE 3306
