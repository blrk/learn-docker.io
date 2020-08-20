### Running a hello-world example
```
$ docker run hello-world
```
### Hello World Docker Image
```
$ docker images
```
### display any running container
```
$ docker ps
```
### inspect a particular Docker Image.
```
$ docker inspect bf7
```
### Running container in detached mode
```
$ docker run -it -d --name test alpine
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
df20fa9351a1: Pull complete 
Digest: sha256:185518070891758909c9f839cf4ca393ee977ac378609f700f60a771a2dfe321
Status: Downloaded newer image for alpine:latest
11cc3377c6238dc1568191a975385f7ed13e2447b08e56d4772732a2eb14faa3
```
### List the running containers
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
11cc3377c623        alpine              "/bin/sh"           12 seconds ago      Up 11 seconds                           demo
```
### Login into the container
```
$ docker attach 11c
/ # cat /etc/os-release 
NAME="Alpine Linux"
ID=alpine
VERSION_ID=3.12.0
PRETTY_NAME="Alpine Linux v3.12"
HOME_URL="https://alpinelinux.org/"
BUG_REPORT_URL="https://bugs.alpinelinux.org/"
/ # 
```
### List running container, all the container and remove a container 
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED        
     STATUS              PORTS               NAMES
[node1] (local) root@192.168.0.13 ~
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED        
     STATUS                          PORTS               NAMES
11cc3377c623        alpine              "/bin/sh"           2 minutes ago  
     Exited (0) About a minute ago                       demo
[node1] (local) root@192.168.0.13 ~
$ docker rm 11c
11c
```
### Run a command inside a container
```
$ docker run -dit --name demo1 alpine sh
72e951a90186663e3f3401ec158fae38de48ea30d43e62a81c2d771e46fb02dc

$ docker exec -it demo1 ls /
bin    etc    lib    mnt    proc   run    srv    tmp    var
dev    home   media  opt    root   sbin   sys    usr

$ docker exec -it demo1 cat /etc/os-release
NAME="Alpine Linux"
ID=alpine
VERSION_ID=3.12.0
PRETTY_NAME="Alpine Linux v3.12"
HOME_URL="https://alpinelinux.org/"
BUG_REPORT_URL="https://bugs.alpinelinux.org/"
```
### checking the status of a container
```
$ docker stats demo1
CONTAINER ID        NAME                CPU %               MEM USAGE / LIM
IT    MEM %               NET I/O             BLOCK I/O           PIDS
72e951a90186        demo1               0.00%               4.195MiB / 31.4
GiB   0.01%               0B / 0B             0B / 0B             1
```
### Stop the running container
```
$ docker stop demo1
demo1
```
### Remove all the containers
```
$ docker rm $(docker ps -aq)
72e951a90186
591ca68b2a24
```
### Remove all the images
```
$ docker rmi $(docker images -aq)
Untagged: alpine:latest
Untagged: alpine@sha256:185518070891758909c9f839cf4ca393ee977ac378609f700f60a771a2dfe321
Deleted: sha256:a24bb4013296f61e89ba57005a7b3e52274d8edd3ae2077d04395f806b63d83e
Deleted: sha256:50644c29ef5a27c9a40c393a73ece2479de78325cae7d762ef3cdc19bf42dd0a
```



