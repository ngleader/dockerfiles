# 한 port에 여러 web server container 연결하기

여러 web server container를 운영시 유용한 방법으로 reverse proxy로 nginx-proxy container를 두고, 각 web server container를 연결하는 방식이다.

## nginx-proxy container 구동
host의 80 port를 사용하기 위해 root 권한으로 `sudo` 구동하고, docker 데몬의 unix socket인 `/var/run/docker.sock`를 연결한다.
```
$ sudo docker run -d -p 80:80 \
        -v /var/run/docker.sock:/tmp/docker.sock:ro \
        jwilder/nginx-proxy
```

## web server container 구동

nginx-proxy와는 환경변수 `VIRTUAL_HOST`를 통해 연결되며, nginx-proxy가 이를 확인하여 설정을 변경한다.

아래 예제는 xip.io DNS서비스를 통한 예제로 `{ip}.xip.io` 도메인 및 하위 도메인 질의시 도메인내 `{ip}`를 ip로 응답을 주는 서비스 이다. 

- project1.127.0.0.1.xip.io
```
$ docker run -d -p 80 --name php70 \
        -e VIRTUAL_HOST=project1.127.0.0.1.xip.io,*.project1.127.0.0.1.xip.io \
        -v $(pwd)/project1:/var/www/html
        ngleader/docker-base:centos7-webtatic-php70
```

- project2.127.0.0.1.xip.io
```
$ docker run -d -p 80 --name php56 \
        -e VIRTUAL_HOST=project1.127.0.0.1.xip.io,*.project2.127.0.0.1.xip.io \
        -v $(pwd)/project2:/var/www/html
        ngleader/docker-base:centos7-webtatic-php56
```

이제 브라우져로 http://project1.127.0.0.1.xip.io 에 접속하면 php70 container로
http://project2.127.0.0.1.xip.io 에 접속하면 php56 container로 서비스 되는 것을 확인 할 수 있다.
그리고 wildcard `*` 도 등록했으니 http://www.project1.127.0.0.1.xip.io 등으로도 접속이 가능하다