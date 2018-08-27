FROM httpd:2.4.27-alpine

RUN apk update && apk add mc apache2 apache2-proxy

RUN apk add apache2-mod-jk
RUN a2enmod jk

COPY ./httpd.conf /usr/local/apache2/conf/httpd.conf
COPY ./httpd-vhosts.conf /usr/local/apache2/conf/extra/httpd-vhosts.conf
COPY ./app1.conf /usr/local/apache2/conf/extra/app1.conf
COPY ./proxy-html.conf /usr/local/apache2/conf/extra/proxy-html.conf

