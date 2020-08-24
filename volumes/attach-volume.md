# Editing Creating and attaching a Volume with container
### create a volume
```
$ docker volume create app-data
app-data
```
### attach a volume 
``` bash
$ docker run -it --name demo -v app-data:/mnt alpine /bin/sh
/ # cd /mnt/
/mnt # touch hello.txt
/mnt # touch mydile.txt
/mnt # ls
hello.txt   mydile.txt
/mnt # 
```
```
exit
```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
c9fe13a401c8        alpine              "/bin/sh"           6 minutes ago       Exited (0) 3 minutes ago                       demo
```
```
$ docker volume ls
DRIVER              VOLUME NAME
local               app-data
```
```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[node1] (local) root@192.168.0.28 ~
$ docker rmi $(docker images -q)
Untagged: httpd:latest
Untagged: httpd@sha256:3cbdff4bc16681541885ccf1524a532afa28d2a6578ab7c2d5154a7abc182379
Deleted: sha256:a6ea92c35c43206ac8a508b2be7d6d6b5ecf5f40e7a9042a35669bcbcb2da201
Deleted: sha256:074e0e3314f787dd955937eb5b17694b1f7fc64631f404223a62e2a4a1292fb6
Deleted: sha256:b05020dd1c0b21291751d69304826e21518a0fa056229bb5c5646a2f80aa2ce5
Deleted: sha256:0724735f53919994876490bd1e9633792f9533a646f2f5cd6a2c607e4c86909a
Deleted: sha256:378cb5ce0d682b9abc91a9cf70ffc7b0ab650d282d90c60439af8158c944041d
Deleted: sha256:d0f104dc0a1f9c744b65b23b3fd4d4d3236b4656e67f776fe13f8ad8423b955c
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:5d1d5407f353843ecf8b16524bc5565aa332e9e6a1297c73a92d3e754b8a636d
Deleted: sha256:1e4467b07108685c38297025797890f0492c4ec509212e2e4b4822d367fe6bc8
Deleted: sha256:7515ee845913c8df9826c988341a09e0240e291c66bdc436a067e070d7910a1f
Deleted: sha256:50ebe6a0675f1ed7ca499a2ec7d8cc993d495dd66ca1035c218ec5efcb6fbb8c
Deleted: sha256:2515e0ecfb82d58c004c4b53fcf9230d9eca8d0f5f823c20172be01eec587ccb
Deleted: sha256:ce30112909569cead47eac188789d0cf95924b166405aa4b71fb500d6e4ae08d
Untagged: alpine:3.9
Untagged: alpine@sha256:414e0518bb9228d35e4cd5165567fb91d26c6a214e9c95899e1e056fcd349011
Deleted: sha256:78a2ce922f8665f5a227dc5cd9fda87221acba8a7a952b9665f99bc771a29963
Deleted: sha256:89ae5c4ee501a09c879f5b58474003539ab3bb978a553af2a4a6a7de248b5740
Untagged: alpine:3.8
Untagged: alpine@sha256:2bb501e6173d9d006e56de5bce2720eb06396803300fe1687b58a7ff32bf4c14
Deleted: sha256:c8bccc0af9571ec0d006a43acb5a8d08c4ce42b6cc7194dd6eb167976f501ef1
Deleted: sha256:7444ea29e45e927abea1f923bf24cac20deaddea603c4bb1c7f2f5819773d453
Untagged: hello-world:latest
Untagged: hello-world@sha256:7f0a9f93b4aa3022c3a4c147a449bf11e0941a1fd0bf4a8e6c9408b2600777c5
Deleted: sha256:bf756fb1ae65adf866bd8c456593cd24beb6a0a061dedf42b26a993176745f6b
Deleted: sha256:9c27e219663c25e0f28493790cc0b88bc973ba3b1686355f221c38a36978ac63
Untagged: alpine:3.6
Untagged: alpine@sha256:66790a2b79e1ea3e1dabac43990c54aca5d1ddf268d9a5a0285e4167c8b24475
Deleted: sha256:43773d1dba76c4d537b494a8454558a41729b92aa2ad0feb23521c3e58cd0440
Deleted: sha256:721384ec99e56bc06202a738722bcb4b8254b9bbd71c43ab7ad0d9e773ced7ac
Untagged: alpine:3.7
Untagged: alpine@sha256:8421d9a84432575381bfabd248f1eb56f3aa21d9d7cd2511583c68c9b7511d10
Deleted: sha256:6d1ef012b5674ad8a127ecfa9b5e6f5178d171b90ee462846974177fd9bdd39f
Deleted: sha256:3fc64803ca2de7279269048fe2b8b3c73d4536448c87c32375b2639ac168a48b
Error response from daemon: conflict: unable to delete 5af38fa34234 (must be forced) - image is referenced in multiple repositories
Error response from daemon: conflict: unable to delete 5af38fa34234 (must be forced) - image is referenced in multiple repositories
Error response from daemon: conflict: unable to delete a24bb4013296 (cannot be forced) - image has dependent child images
```
```
$ docker volume rm $(docker volume ls -q)
app-data
[node1] (local) root@192.168.0.28 ~
$ docker volume ls
DRIVER              VOLUME NAME
```





