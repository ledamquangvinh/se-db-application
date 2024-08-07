# Lab 04 - Basic Query on MySQL 

Reference: 
https://dev.mysql.com/doc/refman/8.4/en/sql-data-manipulation-statements.html
https://www.w3schools.com/sql/


## 1 - Set working database 

```sql
USE db_product;
```
## 2 - Insert data to Database 

* Insert data to ***tb_categories***

```sql
INSERT INTO tb_categories(id, name) VALUES 
(1, 'mobile'),
(2, 'tablet'),
(3, 'laptop'),
(4, 'smartwatch');
```

* Insert data to ***tb_products***
```sql
INSERT INTO tb_products(id, name, price, cat_id) VALUES
(1, 'iPhone X', 200, 1),
(2, 'iPhone XS MAX', 300, 1),
(3, 'iPhone 11 Pro', 350, 1),
(4, 'iPhone 12 Pro Max', 400, 1),
(5, 'iPhone 13 Pro Max', 450, 1),
(6, 'iPhone 14 Pro Max', 500, 1),
(7, 'iPhone 15 Pro Max', 550, 1),
(8, 'iPad gen 10',  400, 2),
(9, 'iPad Air 4', 200, 2),
(10, 'iPad Pro M2', 500, 2),
(11, 'Galaxy S24', 1200, 1),
(12, 'Galazy Z Fold', 2500, 1),
(13, 'Galaxy Flip 6', 2400, 1),
(14, 'Galaxy Tab S9', 2000, 2),
(15, 'Macbook Pro M2', 2500, 3),
(16, 'Macbook Pro M3 14 inch', 2500, 3),
(17, 'Macbook Pro M3 16 inch', 3000, 3),
(18, 'Apple Watch 9', 500, 4),
(19, 'Apple Watch Ultra 2', 800, 4),
(20, 'Galaxy Watch Ultra', 800, 4);

```

## 3 - Query Data 
* Using WHERE map data between tb_categories and tb_products by column cat_id

```sql
SELECT p.id, p.name, p.price, c.name
FROM tb_products p, tb_categories c
WHERE p.cat_id = c.id
```

* Using JOIN Command with Foreign Key Relationship

```sql
SELECT p.id, p.name, p.price, c.name
FROM tb_products p LEFT JOIN tb_categories c ON p.cat_id = c.id;
```

* Limit numbers of row result from query (skip 5 first row and get 10 next rows)
```sql
SELECT p.id, p.name, p.price, c.name
FROM tb_products p LEFT JOIN tb_categories c ON p.cat_id = c.id
LIMIT 5, 10;
```

* Get 5 first rows

```sql
SELECT p.id, p.name, p.price, c.name
FROM tb_products p LEFT JOIN tb_categories c ON p.cat_id = c.id
LIMIT 5;
```

* using ***having*** and ***group by*** command to group data

```sql
SELECT c.name COUNT(p.id) AS num_of_product
FROM tb_products p LEFT JOIN tb_categories c ON p.cat_id = c.id
GROUP BY p.cat_id;
```
* using ***having*** to set condition filter of data
```sql
SELECT c.name, COUNT(p.id) AS num_of_product
FROM tb_products p LEFT JOIN tb_categories c ON p.cat_id = c.id
GROUP BY p.cat_id
HAVING num_of_product > 5;
```

## 4 - Delete data 

* Remove all of products from ***smart watch***
```sql
DELETE FROM tb_products p
WHERE p.cat_id = (SELECT id 
                    FROM tb_categories 
                    WHERE name = 'smart watch'
                );
```

## 5 - Update 

* Rename (update name) of product which name is ***iPhone 12 Pro Max*** to ***Galaxy S25 Utra***

```sql
UPDATE tb_products
SET name = 'Galaxy S25 Ultra'
WHERE name like '%12 Pro Max';
```

## 6 - Create Store Procedure on MySQL
Reference: https://dev.mysql.com/doc/refman/8.4/en/create-procedure.html

* Syntax

* Simple store procedure to get all data of ***tb_products***

```sql
CREATE PROCEDURE `sp_get_all_products`()
BEGIN
    SELECT * FROM tb_products;
END;
```

* Call and execute store procedure
```sql
CALL PROCEDURE sp_get_all_products();
```

* Drop store procedure

```sql
DROP PROCEDURE sp_get_all_products;
```

***Cannot update or change store procedure in MySQL***

* Get information of one product by product_id

```sql
CREATE PROCEDURE sp_get_product_by_id(
  IN id INT,
)
BEGIN
  SELECT p.id, p.name INTO name, p.price INTO price, c.name INTO cat_name
  FROM tb_products p LEFT JOIN tb_categories c ON p.cat_id = c.id
  WHERE p.id = id;
END;
---
CALL sp_get_product_by_id(10);
```

* Count product have category name is "mobile"

```sql
CREATE PROCEDURE sp_count_product_by_category(
  IN cat_name VARCHAR(50),
  OUT num_product INT
)
BEGIN
  SELECT COUNT(p.id) INTO num_product
  FROM tb_products AS p LEFT JOIN tb_categories AS c ON p.cat_id = c.id
  WHERE c.name = cat_name;
END;
--- execute the procedure
CALL sp_count_product_by_category('mobile', @num_of_product);

SELECT @num_of_product;
```

* Drop store procedure

```sql
DROP IF EXIST sp_count_product_by_category;
```

## 7 - Create Function in MySQL

* Create function to count all product

```sql
-- Set DETERMINISTIC for GLOBAL 
SET GLOBAL log_bin_trust_function_creators = 1;

-- CREATE FUNCTIOn
CREATE FUNCTION count_all() RETURNS INT
BEGIN 
    DECLARE total INT;
    SET total = 0;
    SELECT COUNT(id) INTO total FROM tb_products;
    RETURN total;
END;

-- CALL FUNCTION
SELECT count_all();
```

* Drop a function
```sql
DROP FUNCTION IF EXISTS count_all();
```


