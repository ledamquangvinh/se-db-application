# Lab 01 - Setup MySQL with Docker 

Reference: https://www.datacamp.com/tutorial/set-up-and-configure-mysql-in-docker

# Step by Step

## Step 01

* Download MySQL Docker

```shell
docker pull mysql:latest
```

* List docker images on pc
```shell
docker images
```

## Step 02

* Running and Managing MySQL Container

```shell
docker run -d --name db-mysql -p 3307:3306 -e MYSQL_ROOT_PASSWORD=admin123  mysql
```

Command Description
* __run__ creates a new container or starts an existing one
* __-d__: this tag make the container run the background.
* __--name <CONTAINER_NAME>__: Setup Container Name
* __-p <HOST_PORT>:<DOCKER_PORT>__: define the port mapping from Docker Port to Host(PC) Port. In this case is docker port 3306 (default port of mysql) mapped to host port 3306 (real port in PC)
* __-e [ENV_VARIABLE=value]__: the -e tag creates an enviroment variable. In this case is MYSQL_ROOT_PASSWORD that is root password of MySQL DB.
* __<IMAGE_NAME>__ : is the name of the image in this case ***mysql***

## Step 03

Access to MySQL Container

```shell
docker exec -it db-mysql bash 
```

* __exec__: is the command to access to container
* __-it__: 
  
  * -i: Keep STDIN open even if not attached
  * -t: Allocate pseudo-TTY

*__CONTAINER_NAME__ : is the name of container which is accessed. In this case is ***db-mysql***
* __bash__: is type of terminal 

## Step 04
Using bash command to connect mysql-command

* Connect to client as ROOT user as MYSQL
```shell
mysql -u root -p
```

Enter password (admin123) after enter above command

Now, we can use mysql-command (query) to access database

## Step 05
Configuration the MySQL Container
* We can edit configure of MySQL by access to the directories in MySQL Docker Container:
> /etc/mysql/

> /etc/mysql/conf.d

> /etc/mysql/mysql.conf.d

After that, remove current MySQL docker container and create new with command
  * Remove current MySQL Docker Container

```shell
docker container -rm db-mysql -f
```

  * Create host directory configure for MySQL Configure

```shell
mkdir ./mysql-conf
touch ./mysql-conf/my.cnf
```

* Enter the config to apply for MySQL
```shell
vim ./mysql-conf/my.cnf
[mysqld]
max_connections=200
```

* Create new MySQL Docker Container with configure directory mounted
```shell
  ~~name db-mysql \
  -e MYSQL_ROOT_PASSWORD=admin123 \
  -p 3307:3306
  -v ./mysql-conf:/etc/mysql/conf.d \
  -d mysql
```
## Step 06
Advance command docker to preserve the data store in MySQL Docker Container

* Create a volume

```shell
docker volume create db-mysql-data
```
* Create new MySQL Container with the volumn mounted

```shell
docker run \
  --name db-mysql
  -e MYSQL_ROOT_PASSWORD=admin123\
  -p 3307:3306\
  -v db-mysql-data:/var/lib/mysql \
  -d mysql
```

## FINALLY
 ```shell
 docker run \
    --name db-mysql \
    -e MYSQL_ROOT_PASSWORD=admin123 \
    -v ./mysql-conf:/etc/mysql/conf.d\
    -v db-mysql-new-data:/var/lib/mysql \
    -d mysql
 ```
* This command will create new volume db-mysql-new-data and map to MySQL Docker Container