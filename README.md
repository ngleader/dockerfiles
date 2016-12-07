# dockerfiles

## Apache + PHP

### PHP 7.1
#### centos7-remi-php71
```
docker run -d -p 80 -v $(pwd):/var/www/html ngleader/docker-base:centos7-remi-php71
```
#### centos6-remi-php71
```
docker run -d -p 80 -v $(pwd):/var/www/html ngleader/docker-base:centos6-remi-php71
```

### PHP 7.0
#### centos7-webtatic-php70
```
docker run -d -p 80 -v $(pwd):/var/www/html ngleader/docker-base:centos7-webtatic-php70
```
#### centos6-webtatic-php70
```
docker run -d -p 80 -v $(pwd):/var/www/html ngleader/docker-base:centos7-webtatic-php70
```

### PHP 5.6
#### centos7-webtatic-php56
```
docker run -d -p 80 -v $(pwd):/var/www/html ngleader/docker-base:centos7-webtatic-php56
```
#### centos6-webtatic-php56
```
docker run -d -p 80 -v $(pwd):/var/www/html ngleader/docker-base:centos6-webtatic-php56
```
#### centos7-remi-php56
```
docker run -d -p 80 -v $(pwd):/var/www/html ngleader/docker-base:centos7-remi-php56
```
#### centos6-remi-php56
```
docker run -d -p 80 -v $(pwd):/var/www/html ngleader/docker-base:centos6-remi-php56
```

### Customize
대부분의 php extensions가 설치되어 있습니다.
아래와 같이 php와 httpd config 파일을 수정하여 container를 구동할 수 있습니다

1. 필요한 버전의 dummy container 구동
2. config files 복사
```
# remi package인 경우 path가 다르니 phpinfo() 등으로 확인이 필요합니다.
docker cp {container id}:/etc/php/php.ini .
docker cp {container id}:/etc/php.d .
docker cp {container id}:/etc/httpd/conf/httpd.conf .
docker cp {container id}:/etc/httpd/conf.d .
docker {container id} stop
docker {container id} rm
```
3. config가 반영된 새로운 container 구동
```
docker run -d -p 80 \
    -v $(pwd)/html:/var/www/html \
    -v $(pwd)/php.ini:/etc/php.ini \
    -v $(pwd)/php.d:/etc/php.d \
    -v $(pwd)/logs:/etc/httpd/logs \
    ngleader/docker-base:centos6-remi-php56
```

### Container 안으로 
`docker exec -t -i {container id} /bin/bash`


### Run with MySQL etc.

`docker-compose.yml` 저장 후, `$ docker-compose up -d`
 
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