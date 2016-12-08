# PHP 개발을 위한 Docker 

PHP 공식 Docker가 제공되지만, 별도 php extension가 필요하면, dockerFile를 만들고 build를 해야하는 번거로움이 있어 webtatic 과 remi repo에서 제공하는 패키지를 거의 모두 설치한 Docker Image를  만들게 되었습니다. 

## Apache + PHP 구동 방법. 간단 버전

```
$ docker run -d -p 80 -v $(pwd):/var/www/html ngleader/docker-base:centos7-remi-php71
```

`-p 80` 으로 구동한 경우, container의 80 port를 host의 random port와 연결되고
`docker ps`로 나온 `0.0.0.0:xxxxx->80`를 브라우져에서 `http://127.0.0.1:xxxxx`로 접속이 가능합니다.
또는 host port를 8080으로 지정하여 `-p 8080:80` 구동이 가능합니다.

### docker images
- ngleader/docker-base:centos7-remi-php71
- ngleader/docker-base:centos6-remi-php71
- ngleader/docker-base:centos7-webtatic-php70
- ngleader/docker-base:centos6-webtatic-php70
- ngleader/docker-base:centos7-webtatic-php56
- ngleader/docker-base:centos6-webtatic-php56
- ngleader/docker-base:centos7-remi-php56
- ngleader/docker-base:centos6-remi-php56

### Config 수정
memory_limit 수정, php extension 수정, http rewrite 수정 등 php와 httpd config를 수정하여 container를 구동할 수 있습니다.

#### 필요한 버전의 dummy container 구동
#### dummy container에서 config files 복사
```
$ docker cp {container id}:/etc/php/php.ini .
$ docker cp {container id}:/etc/php.d .
$ docker cp {container id}:/etc/httpd/conf/httpd.conf .
$ docker cp {container id}:/etc/httpd/conf.d .
$ docker {container id} stop 
$ docker {container id} rm
```
* remi package인 경우 path가 다르니 `phpinfo()` 또는 `Container 안으로 접속하기` 로 확인이 필요합니다.

#### config가 반영된 새로운 container 구동
```
$ docker run -d -p 80 \
    -v $(pwd)/html:/var/www/html \
    -v $(pwd)/php.ini:/etc/php.ini \
    -v $(pwd)/php.d:/etc/php.d \
    -v $(pwd)/logs:/etc/httpd/logs \
    ngleader/docker-base:centos6-webtatic-php56
```

### Container 안으로 접속하기
`$ docker exec -it {container id} /bin/bash`

### PHP-CLI

#### php70

아래 내용으로 각 파일 저장 후 `$ chmod +x php70` 와 같이 실행 권한을 추가하여 사용
```
#!/bin/bash
docker run -it --rm -v $(pwd):/var/www/html -w /var/www/html --entrypoint=php ngleader/docker-base:centos7-webtatic-php70 $@
```

```
$ ./php70 --version
```
#### composer
```
#!/bin/bash
docker run -it --rm -v $(pwd):/var/www/html -w /var/www/html --entrypoint=composer ngleader/docker-base:centos7-webtatic-php70 $@
```

#### phpunit
```
#!/bin/bash
docker run -it --rm -v $(pwd):/var/www/html -w /var/www/html --entrypoint=phpunit ngleader/docker-base:centos7-webtatic-php70 $@
```

### MySQL 등의 Container와 함께 구동하기

#### docker 버전

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

#### docker-compose 버전

아래 내용을 docker-compose.yml 로 저장 후 `$ docker-compose up -d`

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