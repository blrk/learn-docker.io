### What is Docker Swarm?
* A Docker Swarm is a group of either physical or virtual machines that are running the Docker application and that have been configured to join together in a cluster

### Do you have a local setup then do the following steps to start the docker swarm mode
``` bash
docker swarm init 
docker node ls
```
### No local setup do the following setup 
* Open https://labs.play-with-docker.com/
* Click on settings icon
* choose the option you want 

### docker service command 
``` bash
$ docker service --help

Usage:  docker service COMMAND

Manage services

Commands:
  create      Create a new service
  inspect     Display detailed information on one or more services
  logs        Fetch the logs of a service or task
  ls          List services
  ps          List the tasks of one or more services
  rm          Remove one or more services
  rollback    Revert changes to a service's configuration
  scale       Scale one or multiple replicated services
  update      Update a service

Run 'docker service COMMAND --help' for more information on a command.
```
### Check the setup
``` bash
$ docker service create alpine ping 8.8.8.8
yu8fag60sadzwwawdayiblty9
overall progress: 1 out of 1 tasks 
1/1: running   
verify: Service converged 
```
### list the service
``` bash
$ docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
yu8fag60sadz        mystifying_jones    replicated          1/1                 alpine:latest       
hoztk2miplj8        relaxed_vaughan     replicated          0/1                 alphine:latest      
```
### Check the status
``` bash
$ docker service ps mystifying_jones
ID                  NAME                 IMAGE               NODE          
      DESIRED STATE       CURRENT STATE           ERROR               PORTS
sm8bcuccyl98        mystifying_jones.1   alpine:latest       worker2       
      Running             Running 4 minutes ago 
```
### List the container
``` bash
$ docker container ls
```
### Update the docker service
* create a nginx service
``` bash
$ docker service create nginx 
h2dhndtdr3zufn76izbv04a8a
overall progress: 1 out of 1 tasks 
1/1: running   
verify: Service converged 

$ docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS      
h2dhndtdr3zu        pedantic_napier     replicated          1/1  
```
* Update the docker service
``` bash
$ docker service update pedantic_napier --replicas 3
pedantic_napier
overall progress: 3 out of 3 tasks 
1/3: running   
2/3: running   
3/3: running   
verify: Service converged 
```
``` bash
$ docker service ps pedantic_napier
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
jb2ajsgcz738        pedantic_napier.1   nginx:latest        worker2             Running             Running 8 minutes ago                       
3aycezwwhfl3        pedantic_napier.2   nginx:latest        manager3            Running             Running 3 minutes ago                       
rqk1mqaccw3f        pedantic_napier.3   nginx:latest        manager1            Running             Running 3 minutes ago    ```
* remove one container and see what happends
``` bash
$ docker service stop jb2ajsgcz738
```
* check the status of the service
``` bash
$ docker service ps pedantic_napier 
ID                  NAME                    IMAGE               NODE                DESIRED STATE       CURRENT STATE            ERROR                         PORTS
jb2ajsgcz738        pedantic_napier.1       nginx:latest        worker2             Running             Running 30 minutes ago                                 
3aycezwwhfl3        pedantic_napier.2       nginx:latest        manager3            Running             Running 26 minutes ago                                 
orvx93zdlgeb        pedantic_napier.3       nginx:latest        manager1            Running             Running 5 minutes ago                                  
rqk1mqaccw3f         \_ pedantic_napier.3   nginx:latest        manager1            Shutdown            Failed 5 minutes ago     "task: non-zero exit 
```
* Note : Failed container recreated automatically 
### Delete a node that run one of the container
* In the online playground delete node manager 3
* then run the following commands
``` bash
$ docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS      
h2dhndtdr3zu        pedantic_napier     replicated          1/1  

$ docker service ps h2d
```
* Note that the failed container recreated in another node

[Back to Main page](https://github.com/blrk/learn-docker.io/wiki)
