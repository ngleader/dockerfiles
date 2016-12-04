FROM ngleader/docker-base:centos6-epel
MAINTAINER ngleader <ngleader@gmail.com>

RUN rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm 
RUN yum install -y php70w php70w-bcmath php70w-cli php70w-common php70w-dba php70w-devel php70w-enchant php70w-fpm \
        php70w-gd php70w-imap php70w-intl php70w-ldap php70w-mbstring php70w-mcrypt php70w-mysqlnd php70w-odbc php70w-opcache \
        php70w-pdo php70w-pdo_dblib php70w-pear php70w-pecl-apcu php70w-pecl-apcu-devel php70w-pecl-imagick \
        php70w-pecl-imagick-devel php70w-pecl-mongodb php70w-pecl-redis php70w-pecl-xdebug php70w-process php70w-snmp \
        php70w-soap php70w-tidy php70w-xml php70w-xmlrpc && \
        rm -f /etc/httpd/conf.d/welcome.conf && \
        sed -i "s|;date.timezone =|date.timezone = Asia/Seoul|" /etc/php.ini

EXPOSE 80
ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]

