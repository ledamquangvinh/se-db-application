# Lab03 - Create and Access database on MySQL Docker Container



## Step 1:
Install Client GUI Tool to access Database 

* MySQL WorkBench: 
* Azure Data Studio: https://learn.microsoft.com/en-us/azure-data-studio/download-azure-data-studio?view=sql-server-ver16&tabs=win-install%2Cwin-user-install%2Credhat-install%2Cwindows-uninstall%2Credhat-uninstall#download-azure-data-studio

  * Install Extension: 
    * MySQL
    * SQL Database Project

## Step 2:

Connect to Database with Terminal

### Step 2.1:

Start MySQL Docker Container

```shell
docker container start db-mysql
```
Check status of MySQL Docker Container

```shell
docker ps
```

### Step 2.2:
Access to MySQL Docker Container

```shell
docke exec -it db-mysql bash
```

### Step 2.3:
Connect to Database on MySQL Docker Container

```shell
mysql -u db_user -p
```

## Step 3:
Create Database with command

```sql
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name
    [create_option] ...

create_option: [DEFAULT] {
    CHARACTER SET [=] charset_name
  | COLLATE [=] collation_name
  | ENCRYPTION [=] {'Y' | 'N'}
}
```

and now, we create databse ***db-product*** with command

```sql
CREATE DATABASE IF NOT EXISTS db_product
CHARACTER SET utf16
COLLATE utf16_unicode_ci;
```

Use database
```sql
use db_product;
```

Show current database:
```sql
SELECT DATABASE();
```

and drop database with command:
```sql
DROP DATABASE db_product;
```
Show all databases:
```sql
SHOW DATABASES;
```

