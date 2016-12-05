FROM ngleader/docker-base:centos6-remi
MAINTAINER ngleader <ngleader@gmail.com>

RUN yum install -y libmemcached-last-libs
RUN yum install -y git \
        php56 php56-php-cli php56-php-fpm php56-php-devel php56-php-common php56-runtime php56-build php56-php \
        php56-php-ast php56-php-bcmath php56-php-dbg php56-php-enchant php56-php-gd php56-php-imap php56-php-intl \
        php56-php-json php56-php-ldap php56-php-mbstring php56-php-mcrypt php56-php-mysqlnd php56-php-opcache php56-php-pdo \
        php56-php-pear php56-php-pecl-amqp php56-php-pecl-apcu php56-php-pecl-apcu-bc php56-php-pecl-apcu-devel \
        php56-php-pecl-crypto php56-php-pecl-gearman php56-php-pecl-geoip php56-php-pecl-http php56-php-pecl-http-devel \ 
        php56-php-pecl-igbinary php56-php-pecl-igbinary-devel php56-php-pecl-imagick php56-php-pecl-imagick-devel \
        php56-php-pecl-inotify php56-php-pecl-mailparse php56-php-pecl-memcached php56-php-pecl-mogilefs php56-php-pecl-mongodb \
        php56-php-pecl-oauth php56-php-pecl-redis php56-php-pecl-rrd php56-php-pecl-ssh2 php56-php-pecl-xdebug \
        php56-php-pecl-yaml php56-php-pecl-zip php56-php-pecl-zmq php56-php-phpiredis php56-php-process php56-php-tidy \
        php56-php-xml php56-php-xmlrpc && \
        rm -f /etc/httpd/conf.d/welcome.conf && \
        sed -i "s|;date.timezone =|date.timezone = Asia/Seoul|" /opt/remi/php56/root/etc/php.ini

EXPOSE 80
ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]

