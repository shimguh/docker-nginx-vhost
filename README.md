## 개요
lb, serv-a, serv-b 에 Dockerfile 추가하여 build, run 하여 네트워크를 통해 묶어보기

## 구성
- lb - Dockerfile, config - default.conf
- serv-a - index.html, Dokcerfile
- serv-b - index.html, Dokcerfile

## 테스트 방법
build
```
$ sudo docker build -t lb:0.1.0 -f Dockerfile .
$ sudo docker build -t ng-s-1:0.1.0 -f Dockerfile .
$ sudo docker build -t ng-s-2:0.1.0 -f Dockerfile .
```

run
```
$ sudo docker run -d --name lb-1 -p 9001:80 lb:0.1.0
$ sudo docker run -d --name s1-1 ng-s-1:0.1.0
$ sudo docker run -d --name s2-1 ng-s-2:0.1.0
```

network
```
$ sudo docker network create ablb
$ sudo docker network connect ablb lb-1
$ sudo docker network connect ablb s1-1
$ sudo docker network connect ablb s2-1
```
확인
```
$ sudo docker inspect ablb
```

## 결과
![화면 캡처 2024-02-14 172624](https://github.com/shimguh/docker-nginx-vhost/assets/80744883/145755d6-d81f-4a63-942c-d48e9125c58e)
![화면 캡처 2024-02-14 172609](https://github.com/shimguh/docker-nginx-vhost/assets/80744883/cbbead1f-0590-444e-951b-241f1905503e)
