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
List all user
```sql
SELECT USER();
```
or 
```sql
SELECT CURRENT_USER();
```

