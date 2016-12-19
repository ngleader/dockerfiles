FROM ngleader/docker-base:centos7-webtatic
MAINTAINER ngleader <ngleader@gmail.com>

RUN yum install -y git php70w php70w-bcmath php70w-cli php70w-common php70w-dba php70w-devel php70w-enchant php70w-fpm \
        php70w-gd php70w-imap php70w-intl php70w-ldap php70w-mbstring php70w-mcrypt php70w-mysqlnd php70w-odbc php70w-opcache \
        php70w-pdo php70w-pdo_dblib php70w-pear php70w-pecl-apcu php70w-pecl-apcu-devel php70w-pecl-imagick \
        php70w-pecl-imagick-devel php70w-pecl-mongodb php70w-pecl-redis php70w-pecl-xdebug php70w-process php70w-snmp \
        php70w-soap php70w-tidy php70w-xml php70w-xmlrpc && \
        rm -f /etc/httpd/conf.d/welcome.conf && \
        sed -i "s|;date.timezone =|date.timezone = Asia/Seoul|" /etc/php.ini

RUN yum install -y gcc gcc-c++ make re2c

# install php-rdkafka
RUN cd ~ && mkdir src && cd ~/src && \
    git clone https://github.com/edenhill/librdkafka.git && cd librdkafka && \
    ./configure && make && make install
RUN cd ~/src && \
    git clone https://github.com/arnaud-lb/php-rdkafka.git && cd php-rdkafka && \
    phpize && ./configure && make all -j 5 && make install && \
    echo "extension=rdkafka.so" > /etc/php.d/rdkafka.ini

# php-ev
RUN cd ~/src && \
	git clone https://bitbucket.org/osmanov/pecl-ev.git && cd pecl-ev && \
    phpize && ./configure && make && make install && \
    echo "extension=ev.so" > /etc/php.d/ev.ini

# Install composer
RUN curl https://getcomposer.org/installer | php -- && mv composer.phar /usr/local/bin/composer && chmod +x /usr/local/bin/composer
# Install phpunit
RUN curl https://phar.phpunit.de/phpunit.phar -L -o /usr/local/bin/phpunit && chmod +x /usr/local/bin/phpunit

EXPOSE 80
ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]

