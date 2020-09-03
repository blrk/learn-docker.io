### What is Docker SWarm?
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

$ docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
yu8fag60sadz        mystifying_jones    replicated          1/1                 alpine:latest       
hoztk2miplj8        relaxed_vaughan     replicated          0/1                 alphine:latest      
```
