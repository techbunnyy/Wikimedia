# Use an official CentOS 8 image as a parent image
FROM centos:8

#start Pre-Config
RUN cd /etc/yum.repos.d/
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

RUN yum -y install java

CMD /bin/bash

# Install necessary packages
RUN dnf -y update && dnf -y install httpd php php-mysqlnd php-gd php-xml php-json mod_ssl php-intl php-mbstring php-apcu git unzip wget tar && dnf clean all

# Download and install MediaWiki
WORKDIR /var/www/html
RUN wget https://releases.wikimedia.org/mediawiki/1.40/mediawiki-1.40.1.tar.gz && \
    tar -xzf mediawiki-1.40.1.tar.gz --strip-components=1 && \
    rm mediawiki-1.40.1.tar.gz
# add the directoryIndex for mediawiki as it uses .PHP index file 
RUN sed -i -e 's|    DirectoryIndex index.html|    DirectoryIndex index.html index.html.var index.php|' /etc/httpd/conf/httpd.conf
# Comment out the line for SSL certificate , SSL will be required when in Production env
RUN sed -i -e 's|SSLCertificateFile /etc/pki/tls/certs/localhost.crt|#SSLCertificateFile /etc/pki/tls/certs/localhost.crt|' /etc/httpd/conf.d/ssl.conf
RUN sed -i -e 's|SSLCertificateKeyFile /etc/pki/tls/private/localhost.key|#SSLCertificateKeyFile /etc/pki/tls/private/localhost.key|' /etc/httpd/conf.d/ssl.conf
RUN sed -i -e 's|ServerName|#ServerName|' /etc/httpd/conf/httpd.conf
# Expose Apache web server port
EXPOSE 80

# Start the Apache web server
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]

