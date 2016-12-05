FROM ngleader/docker-base:centos7-remi
MAINTAINER ngleader <ngleader@gmail.com>

RUN yum install -y git \
        php71 php71-php-cli php71-php-fpm php71-php-devel php71-php-common php71-runtime php71-build php71-php \
        php71-php-ast php71-php-bcmath php71-php-dbg php71-php-enchant php71-php-gd php71-php-imap php71-php-intl \
        php71-php-json php71-php-ldap php71-php-mbstring php71-php-mcrypt php71-php-mysqlnd php71-php-opcache php71-php-pdo \
        php71-php-pear php71-php-pecl-amqp php71-php-pecl-apcu php71-php-pecl-apcu-bc php71-php-pecl-apcu-devel \
        php71-php-pecl-crypto php71-php-pecl-gearman php71-php-pecl-geoip php71-php-pecl-http php71-php-pecl-http-devel \ 
        php71-php-pecl-igbinary php71-php-pecl-igbinary-devel php71-php-pecl-imagick php71-php-pecl-imagick-devel \
        php71-php-pecl-inotify php71-php-pecl-mailparse php71-php-pecl-memcached php71-php-pecl-mogilefs php71-php-pecl-mongodb \
        php71-php-pecl-oauth php71-php-pecl-redis php71-php-pecl-rrd php71-php-pecl-ssh2 php71-php-pecl-xdebug \
        php71-php-pecl-yaml php71-php-pecl-zip php71-php-pecl-zmq php71-php-phpiredis php71-php-process php71-php-tidy \
        php71-php-xml php71-php-xmlrpc && \
        rm -f /etc/httpd/conf.d/welcome.conf && \
        sed -i "s|;date.timezone =|date.timezone = Asia/Seoul|" /etc/opt/remi/php71/php.ini

# Install composer
RUN curl https://getcomposer.org/installer | php -- && mv composer.phar /usr/local/bin/composer && chmod +x /usr/local/bin/composer

EXPOSE 80
ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]

