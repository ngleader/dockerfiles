# dockerfiles

## Apache + PHP 구동

```
$ docker run -d -p 8080:80 -v $(pwd):/var/www/html ngleader/docker-base:centos7-remi-php71
```
### docker images
- ngleader/docker-base:centos7-remi-php71
- ngleader/docker-base:centos6-remi-php71
- ngleader/docker-base:centos7-webtatic-php70
- ngleader/docker-base:centos6-webtatic-php70
- ngleader/docker-base:centos7-webtatic-php56
- ngleader/docker-base:centos6-webtatic-php56
- ngleader/docker-base:centos7-remi-php56
- ngleader/docker-base:centos6-remi-php56

### Customize
대부분의 php extensions가 설치되어 있습니다.
아래와 같이 php와 httpd config 파일을 수정하여 container를 구동할 수 있습니다

1. 필요한 버전의 dummy container 구동
2. config files 복사
```
$ docker cp {container id}:/etc/php/php.ini .
$ docker cp {container id}:/etc/php.d .
$ docker cp {container id}:/etc/httpd/conf/httpd.conf .
$ docker cp {container id}:/etc/httpd/conf.d .
$ docker {container id} stop 
$ docker {container id} rm
```
* remi package인 경우 path가 다르니 phpinfo() 등으로 확인이 필요합니다.
3. config가 반영된 새로운 container 구동
```
$ docker run -d -p 80 \
    -v $(pwd)/html:/var/www/html \
    -v $(pwd)/php.ini:/etc/php.ini \
    -v $(pwd)/php.d:/etc/php.d \
    -v $(pwd)/logs:/etc/httpd/logs \
    ngleader/docker-base:centos6-webtatic-php56
```

### Container 안으로 
`$ docker exec -it {container id} /bin/bash`

### PHP-CLI 

아래 내용으로 각 파일 저장 후 `$ chmod +x php70` 으로 실행권한을 추가후 사용

- php70

```
#!/bin/bash
docker run -it --rm -v $(pwd):/var/www/html -w /var/www/html --entrypoint=php ngleader/docker-base:centos7-webtatic-php70 $@
```
- composer
```
#!/bin/bash
docker run -it --rm -v $(pwd):/var/www/html -w /var/www/html --entrypoint=composer ngleader/docker-base:centos7-webtatic-php70 $@
```
- phpunit
```
#!/bin/bash
docker run -it --rm -v $(pwd):/var/www/html -w /var/www/html --entrypoint=phpunit ngleader/docker-base:centos7-webtatic-php70 $@
```

### Run with MySQL etc.

#### docker

```
$ docker run -d --name mysql -p 3306 \
    -v $(pwd)/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=root \
    -e MYSQL_DATABASE=web \
    -e MYSQL_USER=web \
    -e MYSQL_PASSWORD=web\
    mysql:5.7
$ docker run -d -p 80 \
    --link mysql:mysql \
    -v $(pwd)/html:/var/www/html \
    ngleader/docker-base:centos6-webtatic-php56
```

#### docker-compose

아래 내용을 docker-compose.yml로 저장 후 `$ docker-compose up -d`

```
version: '2'

services:
  web:
    image: ngleader/docker-base:centos7-webtatic-php70
    ports:
      - "80"
    volumes:
      - ./html:/var/www/html
      - ./logs:/etc/httpd/logs
      - ./php.ini:/etc/php.ini
    restart: always
    depends_on:
      - db
      - memcached
  db:
    image: mysql:5.7
    ports:
      - "3306"
    volumes:
      - ./data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: web
      MYSQL_USER: web
      MYSQL_PASSWORD: web
  memcached:
    image: memcached:1.4
    restart: always
```

### Debugging with Xdebug and PHPStorm

위 Config 변경방법을 통해 `/etc/php.d/xdebug.ini` 에 두 줄 추가
```
xdebug.remote_enable=on
xdebug.remote_autostart=off
```

Container 구동, `# HOST IP #`는 `ifconfig` 툴 등으로 IP를, `# HOST NAME #`에는 브라우져에서 접속한 도메인 명을 입력
```
$ docker run -d -p 80 \ 
    -v $(pwd):/var/www/html \
    -v $(pwd)/php.d:/etc/php.d \
    -e XDEBUG_CONFIG="remote_host=# HOST IP #" \
    -e PHP_IDE_CONFIG="serverName=# HOST NAME #" \
    ngleader/docker-base:centos7-webtatic-php70
```

PHPStorm 에서 `Preferences > Languages & Frameworks > PHP > Servers` 에서 `+` 추가시 Host 항목에 위의 `# HOST NAME #` 입력

PHPStorm 툴바에서 전화기 모양의 `Start Listening for PHP Debuging Connections` 클릭