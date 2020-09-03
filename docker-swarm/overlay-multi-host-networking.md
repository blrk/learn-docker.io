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
$ docker network create --driver overlay mydrupal
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
cq11r868o398        mydrupal            overlay             swarm
00b8641a709c        none                null                local
```
### Create a psql service on the overlay network
``` bash
$ docker service create --name psql --network mydrupal -e POSTGRES_PASSWORD=mypass postgres
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
### Check the logs
``` bash
$ docker service logs psql
psql.1.p1jmhv5ybysf@manager2    | The files belonging to this database system will be owned by user "postgres".
psql.1.p1jmhv5ybysf@manager2    | This user must also own the server process.
psql.1.p1jmhv5ybysf@manager2    | 
psql.1.p1jmhv5ybysf@manager2    | The database cluster will be initialized with locale "en_US.utf8".
psql.1.p1jmhv5ybysf@manager2    | The default database encoding has accordingly been set to "UTF8".
psql.1.p1jmhv5ybysf@manager2    | The default text search configuration will be set to "english".
psql.1.p1jmhv5ybysf@manager2    | 
psql.1.p1jmhv5ybysf@manager2    | Data page checksums are disabled.
psql.1.p1jmhv5ybysf@manager2    | 
psql.1.p1jmhv5ybysf@manager2    | fixing permissions on existing directory /var/lib/postgresql/data ... ok
psql.1.p1jmhv5ybysf@manager2    | creating subdirectories ... ok
psql.1.p1jmhv5ybysf@manager2    | selecting dynamic shared memory implementation ... posix
psql.1.p1jmhv5ybysf@manager2    | selecting default max_connections ... 100
psql.1.p1jmhv5ybysf@manager2    | selecting default shared_buffers ... 128MB
psql.1.p1jmhv5ybysf@manager2    | selecting default time zone ... Etc/UTC
psql.1.p1jmhv5ybysf@manager2    | creating configuration files ... ok
psql.1.p1jmhv5ybysf@manager2    | running bootstrap script ... ok
psql.1.p1jmhv5ybysf@manager2    | performing post-bootstrap initialization ... ok
psql.1.p1jmhv5ybysf@manager2    | syncing data to disk ... ok
psql.1.p1jmhv5ybysf@manager2    | 
psql.1.p1jmhv5ybysf@manager2    | 
psql.1.p1jmhv5ybysf@manager2    | Success. You can now start the database server using:
psql.1.p1jmhv5ybysf@manager2    | 
psql.1.p1jmhv5ybysf@manager2    |     pg_ctl -D /var/lib/postgresql/data -l logfile start
psql.1.p1jmhv5ybysf@manager2    | 
psql.1.p1jmhv5ybysf@manager2    | initdb: warning: enabling "trust" authentication for local connections
psql.1.p1jmhv5ybysf@manager2    | You can change this by editing pg_hba.conf or using the option -A, or
psql.1.p1jmhv5ybysf@manager2    | --auth-local and --auth-host, the next time you run initdb.
psql.1.p1jmhv5ybysf@manager2    | waiting for server to start....2020-09-03 15:57:51.692 UTC [46] LOG:  starting PostgreSQL 12.4 (Debian 12.4-1.pgdg100+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 8.3.0-6) 8.3.0, 64-bit
psql.1.p1jmhv5ybysf@manager2    | 2020-09-03 15:57:51.695 UTC [46] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
psql.1.p1jmhv5ybysf@manager2    | 2020-09-03 15:57:51.712 UTC [47] LOG:  database system was shut down at 2020-09-03 15:57:51 UTC
psql.1.p1jmhv5ybysf@manager2    | 2020-09-03 15:57:51.718 UTC [46] LOG:  database system is ready to accept connections
psql.1.p1jmhv5ybysf@manager2    |  done
psql.1.p1jmhv5ybysf@manager2    | server started
psql.1.p1jmhv5ybysf@manager2    | 
psql.1.p1jmhv5ybysf@manager2    | /usr/local/bin/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*
psql.1.p1jmhv5ybysf@manager2    | 
psql.1.p1jmhv5ybysf@manager2    | 2020-09-03 15:57:51.778 UTC [46] LOG:  received fast shutdown request
psql.1.p1jmhv5ybysf@manager2    | 2020-09-03 15:57:51.778 UTC [46] LOG:  aborting any active transactions
psql.1.p1jmhv5ybysf@manager2    | waiting for server to shut down....2020-09-03 15:57:51.782 UTC [46] LOG:  background worker "logical replication launcher" (PID 53) exited with exit code 1
psql.1.p1jmhv5ybysf@manager2    | 2020-09-03 15:57:51.782 UTC [48] LOG:  shutting down
psql.1.p1jmhv5ybysf@manager2    | 2020-09-03 15:57:51.795 UTC [46] LOG:  database system is shut down
psql.1.p1jmhv5ybysf@manager2    |  done
psql.1.p1jmhv5ybysf@manager2    | server stopped
psql.1.p1jmhv5ybysf@manager2    | 
psql.1.p1jmhv5ybysf@manager2    | PostgreSQL init process complete; ready for start up.
psql.1.p1jmhv5ybysf@manager2    | 
psql.1.p1jmhv5ybysf@manager2    | 2020-09-03 15:57:51.896 UTC [1] LOG:  starting PostgreSQL 12.4 (Debian 12.4-1.pgdg100+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 8.3.0-6) 8.3.0, 64-bit
psql.1.p1jmhv5ybysf@manager2    | 2020-09-03 15:57:51.897 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
psql.1.p1jmhv5ybysf@manager2    | 2020-09-03 15:57:51.898 UTC [1] LOG:  listening on IPv6 address "::", port 5432
psql.1.p1jmhv5ybysf@manager2    | 2020-09-03 15:57:51.901 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
psql.1.p1jmhv5ybysf@manager2    | 2020-09-03 15:57:51.918 UTC [55] LOG:  database system was shut down at 2020-09-03 15:57:51 UTC
psql.1.p1jmhv5ybysf@manager2    | 2020-09-03 15:57:51.922 UTC [1] LOG:  database system is ready to accept connections
```
### Starting durpal
``` bash
$ docker service create --name drupal --network mydrupal -p 80:80 drupal
6k4yzlhmaxvgpbeuxalx7hjpx
overall progress: 1 out of 1 tasks 
1/1: running   
verify: Service converged 
```
