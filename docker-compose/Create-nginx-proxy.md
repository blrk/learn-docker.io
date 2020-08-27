### Create a Nginx Proxy server for Apache web server
``` bash
$ mkdir compose-sample-3; cd compose-sample-3
```
### Create a docker-compose file
``` bash
$ vi docker-compose.yaml
```
### add the following content to the docker compose file
``` yaml
version: '3'

services:
  proxy:
    image: nginx:1.13 # this will use the latest version of 1.13.x
    ports:
      - '80:80' # expose 80 on host and sent to 80 in container
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf:ro
  web:
    image: httpd  # this will use httpd:latest
```
### Create configuration for Nginx server to point apache
``` bash
$ vi nginx.conf
```
### add the following configuration
``` bash
server {

	listen 80;

	location / {

		proxy_pass         http://web;
		proxy_redirect     off;
		proxy_set_header   Host $host;
		proxy_set_header   X-Real-IP $remote_addr;
		proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header   X-Forwarded-Host $server_name;

	}
}
```
### Run the docker compose file
``` bash
$ docker-compose up -d
Creating network "compose-sample-3_default" with the default driver
Pulling proxy (nginx:1.13)...
1.13: Pulling from library/nginx
f2aa67a397c4: Pull complete
3c091c23e29d: Pull complete
4a99993b8636: Pull complete
Digest: sha256:b1d09e9718890e6ebbbd2bc319ef1611559e30ce1b6f56b2e3b479d9da51dc35
Status: Downloaded newer image for nginx:1.13
Pulling web (httpd:)...
latest: Pulling from library/httpd
bf5952930446: Already exists
3d3fecf6569b: Pull complete
b5fc3125d912: Pull complete
679d69c01e90: Pull complete
76291586768e: Pull complete
Digest: sha256:3cbdff4bc16681541885ccf1524a532afa28d2a6578ab7c2d5154a7abc182379
Status: Downloaded newer image for httpd:latest
Creating compose-sample-3_proxy_1 ... done
Creating compose-sample-3_web_1   ... done
```

[Back to Main page](https://github.com/blrk/learn-docker.io/wiki)
