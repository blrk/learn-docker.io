### Working with Docker Images
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              4ab4c602aa5e        6 weeks ago         1.84kB
```
### List all images
```
docker images -a
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              4ab4c602aa5e        6 weeks ago         1.84kB
```
### List images by name and tag
### pull an Image
```
$ docker pull alpine:3.6
$ docker pull alpine:3.7
$ docker pull alpine:3.8
$ docker pull alpine:3.9
```
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              3.9                 78a2ce922f86        3 months ago        5.55MB
alpine              3.8                 c8bccc0af957        6 months ago        4.41MB
hello-world         latest              bf756fb1ae65        7 months ago        13.3kB
alpine              3.6                 43773d1dba76        17 months ago       4.03MB
alpine              3.7                 6d1ef012b567        17 months ago       4.21MB
```
```
$ docker images alpine:3.8
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              3.8                 c8bccc0af957        6 months ago        4.41MB
```
### List the full length image IDs
```
$ docker images --no-trunc 
REPOSITORY          TAG                 IMAGE ID                           
                                       CREATED             SIZE
alpine              3.9                 sha256:78a2ce922f8665f5a227dc5cd9fd
a87221acba8a7a952b9665f99bc771a29963   3 months ago        5.55MB
alpine              3.8                 sha256:c8bccc0af9571ec0d006a43acb5a
8d08c4ce42b6cc7194dd6eb167976f501ef1   6 months ago        4.41MB
hello-world         latest              sha256:bf756fb1ae65adf866bd8c456593
cd24beb6a0a061dedf42b26a993176745f6b   7 months ago        13.3kB
alpine              3.6                 sha256:43773d1dba76c4d537b494a84545
58a41729b92aa2ad0feb23521c3e58cd0440   17 months ago       4.03MB
alpine              3.7                 sha256:6d1ef012b5674ad8a127ecfa9b5e
6f5178d171b90ee462846974177fd9bdd39f   17 months ago       4.21MB
```
### Listing out images with filter
```
$ docker pull httpd
$ docker images --filter=reference=alpine
REPOSITORY          TAG                 IMAGE ID            CREATED        
     SIZE
alpine              3.9                 78a2ce922f86        3 months ago   
     5.55MB
alpine              3.8                 c8bccc0af957        6 months ago   
     4.41MB
alpine              3.6                 43773d1dba76        17 months ago  
     4.03MB
alpine              3.7                 6d1ef012b567        17 months ago  
     4.21MB
```


