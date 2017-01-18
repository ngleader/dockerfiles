FROM ngleader/docker-base:centos7-webtatic
MAINTAINER ngleader <ngleader@gmail.com>

RUN yum install -y git php56w php56w-bcmath php56w-cli php56w-common php56w-dba php56w-devel php56w-enchant php56w-fpm \
        php56w-gd php56w-imap php56w-intl php56w-ldap php56w-mbstring php56w-mcrypt php56w-mysqlnd php56w-odbc php56w-opcache \
        php56w-pdo php56w-pdo_dblib php56w-pear php56w-pecl-apcu php56w-pecl-apcu-devel php56w-pecl-imagick \
        php56w-pecl-imagick-devel php56w-pecl-mongodb php56w-pecl-redis php56w-pecl-xdebug php56w-process php56w-snmp \
        php56w-soap php56w-tidy php56w-xml php56w-xmlrpc && \
        rm -f /etc/httpd/conf.d/welcome.conf && \
        sed -i "s|;date.timezone =|date.timezone = Asia/Seoul|" /etc/php.ini && \
        sed -i "s|expose_php = On|expose_php = Off|" /etc/php.ini && \
        sed -i "s|variables_order =|;variables_order =|" /etc/php.ini

# Install composer
RUN curl https://getcomposer.org/installer | php -- && mv composer.phar /usr/local/bin/composer && chmod +x /usr/local/bin/composer
# Install phpunit
RUN curl https://phar.phpunit.de/phpunit.phar -L -o /usr/local/bin/phpunit && chmod +x /usr/local/bin/phpunit

EXPOSE 80
ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]

