### Overlay Networking
* The overlay network driver creates a distributed network among multiple Docker daemon hosts. 
* This network sits on top of the host-specific networks
* allowing containers connected to it to communicate securely when encryption is enabled.
### List nodes of Swarm cluster
``` bash
$ docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
rk1age2gop2mkzrnogqhootrn *   manager1            Ready               Active              Leader              19.03.11
ol09zyif4b0wohhm38g1dabi3     manager2            Ready               Active              Reachable           19.03.11
lip2uua8donrp4ett7gyc8hx8     manager3            Ready               Active              Reachable           19.03.11
w6320n5a5u6tik33q5tdbo5a5     worker1             Ready               Active                                  19.03.11
qp6v573kbd8d7rddow8gxkb0x     worker2             Ready               Active                                  19.03.11
```
### Inspect an individual node
``` bash
$ docker node inspect self --pretty
ID:                     rk1age2gop2mkzrnogqhootrn
Hostname:               manager1
Joined at:              2020-09-03 15:24:21.156372917 +0000 utc
Status:
 State:                 Ready
 Availability:          Active
 Address:               192.168.0.22
Manager Status:
 Address:               192.168.0.22:2377
 Raft Status:           Reachable
 Leader:                Yes
Platform:
 Operating System:      linux
 Architecture:          x86_64
Resources:
 CPUs:                  8
 Memory:                31.4GiB
Plugins:
 Log:           awslogs, fluentd, gcplogs, gelf, journald, json-file, local, logentries, splunk, syslog
 Network:               bridge, host, ipvlan, macvlan, null, overlay
 Volume:                local
Engine Version:         19.03.11
TLS Info:
 TrustRoot:
-----BEGIN CERTIFICATE-----
MIIBazCCARCgAwIBAgIUUY7OmawwYcZxliVFS9kGc+TqNZcwCgYIKoZIzj0EAwIw
EzERMA8GA1UEAxMIc3dhcm0tY2EwHhcNMjAwOTAzMTUxOTAwWhcNNDAwODI5MTUx
OTAwWjATMREwDwYDVQQDEwhzd2FybS1jYTBZMBMGByqGSM49AgEGCCqGSM49AwEH
A0IABDEYdQLL9Dwem0kyeUq3HlogfLqlQiyb63BKlDGkSCA8p+YDMScACjI7Tvah
3I4hr/U1ap7mLdHWsSv5PcGfsQWjQjBAMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMB
Af8EBTADAQH/MB0GA1UdDgQWBBS3wRoCrmpRuuXntRIAm4lmsqhDgjAKBggqhkjO
PQQDAgNJADBGAiEAgn8Yb5ncUAOErN5Xkx6EyQLFf8pbyhiEuBRKTrzKjScCIQCy
895i3usKv+5oDUcf57IQPFNAmgsnOAHkhWZhvl/U3w==
-----END CERTIFICATE-----

 Issuer Subject:        MBMxETAPBgNVBAMTCHN3YXJtLWNh
 Issuer Public Key:     MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEMRh1Asv0PB6bSTJ5SrceWiB8uqVCLJvrcEqUMaRIIDyn5gMxJwAKMjtO9qHcjiGv9TVqnuYt0daxK/k9wZ+xBQ==
```
### Create a network
``` bash
$ docker network create --driver overlay mydurpal
cq11r868o398xqones5koum5d
```
### lsit the network
``` bash
$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
68cabd1cc39b        bridge              bridge              local
07b7db699d15        docker_gwbridge     bridge              local
6142b607135a        host                host                local
u7karqxxp9q8        ingress             overlay             swarm
cq11r868o398        mydurpal            overlay             swarm
00b8641a709c        none                null                local
```
### Create a psql service on the overlay network
``` bash
$ docker service create --name psql --network mydurpal -e POSTGRES_PASSWORD=mypass postgres
ptmnpmdd1uax2h2g8uogvq6gq
overall progress: 1 out of 1 tasks 
1/1: running   
verify: Service converged 
```
### list the service
``` bash
$ docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
ptmnpmdd1uax        psql                replicated          1/1                 postgres:latest     
```
``` bash
$ docker service ps psql
ID                  NAME                IMAGE               NODE      DESIRED STATE       CURRENT STATE          ERROR PORTS
p1jmhv5ybysf        psql.1              postgres:latest     manager2       Running             Running 5 minutes ago  
```

