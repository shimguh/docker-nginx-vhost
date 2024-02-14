# docker nginx vhost
![304601299-878eaf6a-18bc-4467-8b3f-5086de8ff3a1](https://github.com/shimguh/docker-nginx-vhost/assets/80744883/d22b2261-1ce0-4a63-a4d3-338963b16d1d)

# load-balancing
- https://www.nginx.com/resources/glossary/load-balancing/
  
## step 1
docker rm, rmi
```
$ sudo docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
$ sudo docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

## step 2
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

$sudo docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                   NAMES
3746c2e8582d   lb:0.1.0       "/docker-entrypoint.…"   49 minutes ago   Up 46 minutes   0.0.0.0:9001->80/tcp, :::9001->80/tcp   lb-1
9be090877987   ng-s-2:0.1.0   "/docker-entrypoint.…"   53 minutes ago   Up 53 minutes   80/tcp                                  s2-1
a210842225ff   ng-s-1:0.1.0   "/docker-entrypoint.…"   53 minutes ago   Up 53 minutes   80/tcp                                  s1-1
```

## setp 3
network
```
$ sudo docker network ls
$ sudo docker network create ablb
$ sudo docker network connect ablb lb-1
$ sudo docker network connect ablb s1-1
$ sudo docker network connect ablb s2-1
```
확인
```
$ sudo docker inspect ablb
```

## step 1, 2, 3 by Dockerfile
```
$ tree
.
├── README.md
├── lb
│   ├── Dockerfile
│   └── config
│       └── default.conf
├── ngninx
│   └── Dockerfile
├── serv-a
│   ├── Dockerfile
│   └── index.html
└── serv-b
    ├── Dockerfile
    └── index.html
```

- test
```
  $ curl http://localhost:9001
<h1>A</h1>
$ curl http://localhost:9001
<h1>B</h1>
$ curl http://localhost:9001
<h1>A</h1>
```

## ref
- https://hub.docker.com/
