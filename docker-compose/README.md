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
docker run -dit --name my-apache-app -p 8080:80 -v "$PWD":/usr/local/apache2/htdocs/ httpd:2.4
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

