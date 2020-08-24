### Create Directory Structure
``` bash
$ mkdir compose-example-2; cd compose-example-2
```
### Create Dockerfile for Webapp
* Create a Dockerfile in webapp directory to create a customized image for your application including Apache web server.
``` bash
mkdir webapp
echo "<h1>Welcome to RK's home page</h1>" > webapp/index.html
```
# Create Dockerfile for Webapp
* CMD 
``` bash
vi  webapp/Dockerfile
```
* add the following content
* FROM creates a layer from the ubuntu:18.04 Docker image.
* COPY adds files from your Docker client’s current directory.
* RUN builds your application with make.
* CMD specifies what command to run within the container.
``` bash
FROM ubuntu:18.04 

RUN apt-get update \
   && apt-get install -y apache2

COPY index.html /var/www/html/
WORKDIR /var/www/html
CMD ["apachectl", "-D", "FOREGROUND"]
EXPOSE 80
```
