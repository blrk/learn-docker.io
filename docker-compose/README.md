# Docker-compose

``` bash
[node1] (local) root@192.168.0.23 ~
$ mkdir compose-example-1
[node1] (local) root@192.168.0.23 ~
$ cd compose-example-1/
[node1] (local) root@192.168.0.23 ~/compose-example-1
```
### My first compose file 
``` bash
vi docker-compose.yaml
```
### docker-compose.yaml file
``` yaml
version: '3' 
  
# same as 
# docker run -dit --name my-apache-app -p 8080:80 -v "$PWD":/usr/local/apache2/htdocs/ httpd:latest

services:
  httpd:
    image: httpd:latest
    volumes:
      - .:/usr/local/apache2/htdocs/
    ports:
      - '8080:80'
```
### Running my first docker compose file
``` bash
$ docker-compose up 
Creating network "test1_default" with the default driver
Pulling httpd (httpd:latest)...latest: Pulling from library/httpdbf5952930446: Pull complete3d3fecf6569b: Pull completeb5fc3125d912: Pull complete679d69c01e90: Pull complete
76291586768e: Pull complete
Digest: sha256:3cbdff4bc16681541885ccf1524a532afa28d2a6578ab7c2d5154a7abc182379
Status: Downloaded newer image for httpd:latest
Creating test1_httpd_1 ... done
Attaching to test1_httpd_1
httpd_1  | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.19.0.2. Set the 'ServerName' directive globally to suppress this message
httpd_1  | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.19.0.2. Set the 'ServerName' directive globally to suppress this message
httpd_1  | [Mon Aug 24 04:19:32.161521 2020] [mpm_event:notice] [pid 1:tid 140199202600064] AH00489: Apache/2.4.46 (Unix) configured -- resuming normal operations
httpd_1  | [Mon Aug 24 04:19:32.161642 2020] [core:notice] [pid 1:tid 140199202600064] AH00094: Command line: 'httpd -D FOREGROUND'
httpd_1  | 172.18.0.1 - - [24/Aug/2020:04:19:35 +0000] "GET / HTTP/1.1" 200 225
httpd_1  | 172.18.0.1 - - [24/Aug/2020:04:19:36 +0000] "GET /favicon.ico HTTP/1.1" 404 196
httpd_1  | 172.18.0.1 - - [24/Aug/2020:04:19:46 +0000] "GET /docker-compose.yaml HTTP/1.1" 200 251
```
