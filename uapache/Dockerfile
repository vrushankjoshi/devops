FROM centos:latest

RUN yum -y install wget

RUN yum -y install apache2 httpd httpd-devel pcre-devel openssl-devel expat-devel apr apr-devel apr-util apr-util-devel gcc gcc-c++ make autoconf libtool


##########INSTALL PCRE PACKAGE ################################
RUN mkdir -p /usr/local/pcre/
WORKDIR /usr/local/pcre/

COPY ./pcre2-10.31.tar.gz /usr/local/pcre/pcre2-10.31.tar.gz

RUN tar xvfz pcre2-10.31.tar.gz

WORKDIR /usr/local/pcre/pcre2-10.31/
RUN ls -lrt /usr/local/pcre/pcre2-10.31
RUN ./configure --prefix=/usr/local/pcre


##########INSTALL mod_jk.so ################################


RUN mkdir -p /opt/mod_jk/

WORKDIR /opt/mod_jk

COPY ./tomcat-connectors-1.2.42-src.tar.gz /opt/mod_jk/tomcat-connectors-1.2.42-src.tar.gz

RUN tar -xvf /opt/mod_jk/tomcat-connectors-1.2.42-src.tar.gz

WORKDIR /opt/mod_jk/tomcat-connectors-1.2.42-src/native/

RUN mkdir -p /etc/httpd/

#RUN ./configure --prefix=/etc/httpd/ --sbindir=/sbin/ --enable-ssl --enable-so
RUN ./configure --with-apxs=/usr/bin/apxs

RUN make

RUN libtool --finish /usr/lib64/httpd/modules

RUN make install

# Copy load balancing configurations files
COPY mod_jk.conf /etc/httpd/conf.d/
COPY workers.properties /etc/httpd/conf.d/

EXPOSE 80

# Simple startup script to avoid some issues observed with container restart 
ADD run-httpd.sh /run-httpd.sh
RUN chmod -v +x /run-httpd.sh


RUN ls -lrt /etc/init.d/
#RUN ls -lrt /usr/local/apache2/

RUN systemctl enable httpd

CMD ["httpd start"]