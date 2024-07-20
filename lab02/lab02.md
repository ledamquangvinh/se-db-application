# Lab02 - Access MySQL with command

## Step 01 
Start ***db-mysql*** Docker Container

```shell
docker container start db-mysql
```
Or use alias docker command

```shell
docker start db-mysql
```

## Step 02

* Access to MySQL Docker Container

```shell
  docker exec -it db-mysql bash
```


## Step 03
* Log into MySQL Database

```shell
mysql -u root -p
```
Enter root password(admin123) after enter above command

* Create MySQL Database User

```shell
CREATE USER 'db_user'@'localhost' IDENTIFIED BY 'user_pass';
```
* Set permission (Privileges) for **db_*user*** to access
```shell
GRANT ALL PRIVILEGES ON *.* TO 'db_user'@'localhost' 
```

* List all users on MySQL

```shell
SELECT user, host FROM mysql.user
```

__db_user__ is databas user who which is use for access to database
__user_password__ is password of database user

* Log out MySQL Database (with root user)
```shell
\q
```

* Log into new User Database(db_user)
```shell
mysql -u db_user -p
```

Enter db_user password(user_password)

* Show current user information login to MySQL

```shell
SELECT USER();
```

Or

```shell
SELECT CURRENT_USER();
```

* When we want to run MySQL Docker Container

```shell
docker container start db-mysql
```

with __db-mysql__ is docker container name or container ID

