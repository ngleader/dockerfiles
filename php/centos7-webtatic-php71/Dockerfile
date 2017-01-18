FROM ngleader/docker-base:centos7-webtatic
MAINTAINER ngleader <ngleader@gmail.com>

RUN yum install -y git php71w php71w-bcmath php71w-cli php71w-common php71w-dba php71w-devel php71w-enchant php71w-fpm \
        php71w-gd php71w-imap php71w-intl php71w-ldap php71w-mbstring php71w-mcrypt php71w-mysqlnd php71w-odbc php71w-opcache \
        php71w-pdo php71w-pdo_dblib php71w-pear php71w-pecl-apcu \
        php71w-pecl-mongodb php71w-pecl-redis php71w-phpdbg php71w-process php71w-snmp \
        php71w-soap php71w-tidy php71w-xml php71w-xmlrpc && \
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
