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

```sql
CREATE USER 'db_user'@'localhost' IDENTIFIED BY 'user_pass';
```
* Set permission (Privileges) for **db_*user*** to access
```sql
GRANT ALL PRIVILEGES ON *.* TO 'db_user'@'localhost' 
```

* List all users on MySQL

```sql
SELECT user, host FROM mysql.user
```

__db_user__ is databas user who which is use for access to database
__user_password__ is password of database user

* Log out MySQL Database (with root user)
```sql
\q
```

* Log into new User Database(db_user)
```sql
mysql -u db_user -p
```

Enter db_user password(user_password)

* Show current user information login to MySQL

```sql
SELECT USER();
```

Or

```sql
SELECT CURRENT_USER();
```

List All user

```sql
SELECT host, user FROM mysql.user;
```

## To change user password, using root user and run command
* Connect Database with ***root***

```shell
mysql -u root -p
```
Enter root password(admin123)

* Change db_user password to 'new_password'

```sql
ALTER USER 'db_user'@'localhost' IDENTIFIED BY 'New-password';
```

* Reload Grant Table
```sql
FLUSH PRIVILEDGES 
```
* When we want to run MySQL Docker Container

```shell
docker container start db-mysql
```



with __db-mysql__ is docker container name or container ID

