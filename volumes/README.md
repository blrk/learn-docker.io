# Editing Create and manage docker volumes using CLI
### Creating a volume
```
$ docker volume create mydata
mydata
```
### Listing Docker Volumes
```
$ docker volume ls
DRIVER              VOLUME NAME
local               mydata
```
### Inspecting docker volume
```
$ docker inspect mydata 
[
    {
        "CreatedAt": "2020-08-17T02:09:43Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/mydata/_data",
        "Name": "mydata",
        "Options": {},
        "Scope": "local"
    }
]
```
### Removing a docker volume
```
$ docker volume rm mydata 
mydata
[node1] (local) root@192.168.0.28 ~
$ docker volume ls
DRIVER       
```

