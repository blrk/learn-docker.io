### Running Wordpress application using docker-compose
``` bash
mkdir compose-example-2; cd compose-example-2
```
### create a docker compose file and add the following configuration
``` bash
vi compose-sample-2.yaml
``` yaml
version: '3'

services:

  wordpress:
    image: wordpress
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: example
      WORDPRESS_DB_PASSWORD: examplePW
    volumes:
      - ./wordpress-data:/var/www/html

  mysql:
    image: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: examplerootPW
      MYSQL_DATABASE: wordpress
      MYSQL_USER: example
      MYSQL_PASSWORD: examplePW
    volumes:
      - mysql-data:/var/lib/mysql

volumes:
  mysql-data:
```
### Run the docker compose file
``` bash
$ docker-compose -f compose-sample-2.yaml up -d
Starting compose-example-2_wordpress_1 ... done
Starting compose-example-2_mysql_1     ... done
```

