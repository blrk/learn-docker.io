# Editing Docker Image How to create your own Image
### Run a container
```
$ docker run -itd alpine sh
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
df20fa9351a1: Pull complete 
Digest: sha256:185518070891758909c9f839cf4ca393ee977ac378609f700f60a771a2df
e321
Status: Downloaded newer image for alpine:latest
534cbeb5fb14e87d0843748e34ea27a1ef529222413d00ac0fe3ace9f5049582
```
### List the running container
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED        
      STATUS              PORTS               NAMES
534cbeb5fb14        alpine              "sh"                About a minute 
ago   Up About a minute                       sleepy_mestorf
```
### connect to the container
```
$ docker attach 53
/ # cat /etc/os-release 
NAME="Alpine Linux"
ID=alpine
VERSION_ID=3.12.0
PRETTY_NAME="Alpine Linux v3.12"
HOME_URL="https://alpinelinux.org/"
BUG_REPORT_URL="https://bugs.alpinelinux.org/"
```
```
/ # vi /etc/os-release 
/ # cat /etc/os-release 
NAME="Alpine Linux"
ID=alpine
VERSION_ID=3.12.0
PRETTY_NAME="Alpine Linux v3.12"
HOME_URL="https://alpinelinux.org/"
BUG_REPORT_URL="https://bugs.alpinelinux.org/"
#blrk added a line
```
### Update 
```
/ # apk update
fetch http://dl-cdn.alpinelinux.org/alpine/v3.12/main/x86_64/APKINDEX.tar.g
z
fetch http://dl-cdn.alpinelinux.org/alpine/v3.12/community/x86_64/APKINDEX.
tar.gz
v3.12.0-240-g35a7b2e977 [http://dl-cdn.alpinelinux.org/alpine/v3.12/main]
v3.12.0-241-g30378ef162 [http://dl-cdn.alpinelinux.org/alpine/v3.12/communi
ty]
OK: 12749 distinct packages available
```
### install git
```
/ # apk add git
(1/6) Installing ca-certificates (20191127-r4)
(2/6) Installing nghttp2-libs (1.41.0-r0)
(3/6) Installing libcurl (7.69.1-r0)
(4/6) Installing expat (2.2.9-r1)
(5/6) Installing pcre2 (10.35-r0)
(6/6) Installing git (2.26.2-r0)
Executing busybox-1.31.1-r16.trigger
Executing ca-certificates-20191127-r4.trigger
OK: 22 MiB in 20 packages
```
### commit the changes and exit (Press --> ctrl +P+Q)
```
Executing busybox-1.31.1-r16.trigger
Executing ca-certificates-20191127-r4.trigger
OK: 22 MiB in 20 packages
/ # read escape sequence
```
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED        
     STATUS              PORTS               NAMES
534cbeb5fb14        alpine              "sh"                12 minutes ago 
     Up 12 minutes                           sleepy_mestorf
```
### commit the changes
```
$ docker commit -m "updated and installed git" 534 blrk/alpine-git
sha256:5af38fa34234e7ab39be5d25fb410ba653f98fa589040698ee139fbf2fea9723
```
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED        
     SIZE
blrk/alpine-git     latest              5af38fa34234        10 seconds ago 
     23.9MB
httpd               latest              a6ea92c35c43        11 days ago    
     166MB
ubuntu              latest              1e4467b07108        3 weeks ago    
     73.9MB
alpine              latest              a24bb4013296        2 months ago        5.57MB
alpine              3.9                 78a2ce922f86        3 months ago        5.55MB
alpine              3.8                 c8bccc0af957        6 months ago        4.41MB
hello-world         latest              bf756fb1ae65        7 months ago        13.3kB
alpine              3.6                 43773d1dba76        17 months ago       4.03MB
alpine              3.7                 6d1ef012b567        17 months ago 
```
### Tag an image
```
docker tag blrk/alpine-git:latest blrk/alpine-git:1.0
```
```
docker images
REPOSITORY          TAG                 IMAGE ID            CREATED        
     SIZE
blrk/alpine-git     1.0                 5af38fa34234        3 minutes ago  
     23.9MB
blrk/alpine-git     latest              5af38fa34234        3 minutes ago  
     23.9MB
httpd               latest              a6ea92c35c43        11 days ago    
     166MB
ubuntu              latest              1e4467b07108        3 weeks ago    
     73.9MB
alpine              latest              a24bb4013296        2 months ago   
     5.57MB
alpine              3.9                 78a2ce922f86        3 months ago        5.55MB
alpine              3.8                 c8bccc0af957        6 months ago        4.41MB
hello-world         latest              bf756fb1ae65        7 months ago        13.3kB
alpine              3.6                 43773d1dba76        17 months ago       4.03MB
alpine              3.7                 6d1ef012b567        17 months ago       4.21MB
```
### pushing an image to dockerhub
### login to docker hub
```
$ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you d
on't have a Docker ID, head over to https://hub.docker.com to create one.
Username: blrk
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.j
son.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-sto
re

Login Succeeded
```
```
$ docker push blrk/alpine-git:1.0 
The push refers to repository [docker.io/blrk/alpine-git]
d278b7546df3: Pushed 
50644c29ef5a: Mounted from library/alpine 
1.0: digest: sha256:15c0b546ad1b4d9e6f756b41ec28991d71ca524a2d61efa6207d746
cf211505b size: 740
```
