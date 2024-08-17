# Export and Import data

## 1 - Using Terminal 
* Connect to MySQL Docker Container
```shell
docker exec -it db-mysql bash
```

* Navigate to directory ***/var/lib/mysql*** (docker volume mapping)
```shell
cd /var/lib/mysql
```

* Dump Database to Script SQL file with name db_product_script.sql 
```shell
mysqldump -u root -p db_product > db_product_script.sql
```
* Copy file script file ***db_product_script.sql*** from docker volume or docker container ***db-mysql-new-data*** to host (PC)

```shell
docker cp db-mysql:/var/lib/mysql/db_product_script.sql ./scripts/db_product_script.sql
```