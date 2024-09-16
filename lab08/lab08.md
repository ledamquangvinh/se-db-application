# Event Schedule in MySQL

* Using Database ***db_product***


## 1 - Basic Event Syntax

Syntax to create Event Schedule

```sql
CREATE
    [DEFINER = user]
    EVENT
    [IF NOT EXISTS]
    event_name
    ON SCHEDULE schedule
    [ON COMPLETION [NOT] PRESERVE]
    [ENABLE | DISABLE | DISABLE ON {REPLICA | SLAVE}]
    [COMMENT 'string']
    DO event_body;

schedule: {
    AT timestamp [+ INTERVAL interval] ...
  | EVERY interval
    [STARTS timestamp [+ INTERVAL interval] ...]
    [ENDS timestamp [+ INTERVAL interval] ...]
}

interval:
    quantity {YEAR | QUARTER | MONTH | DAY | HOUR | MINUTE |
              WEEK | SECOND | YEAR_MONTH | DAY_HOUR | DAY_MINUTE |
              DAY_SECOND | HOUR_MINUTE | HOUR_SECOND | MINUTE_SECOND}
```

* Create a table ***tb_report*** to sumarize the total product in Database for every minute. 

```sql
CREATE TABLE tb_report(
	id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
	total INT NOT NULL,
	created_at TIMESTAMP
);
```

* Create event update report with store total productf for each minute

```sql
CREATE EVENT event_total_product_report_minute
    ON SCHEDULE EVERY 5 SECOND
    DO
        BEGIN
            DECLARE total INT;
            SELECT COUNT(id) INTO total FROM tb_products;
        
            INSERT INTO tb_report(total, created_at) VALUES (total, NOW());
  	    END;
```

* After that, we select ***tb_report*** to show with total 20 products

```sql
SELECT * FROM tb_report;
```

* And then, add one product to the tb_products
```sql
INSERT INTO tb_products(id, name, price, cat_id) VALUES (21, 'iPhone 16 PRO MAX', 2000, 4);
```

* Now, we select ***tb_products*** the log is update with new value 21 products.

```sql
SELECT * FROM tb_report;
```

* Drop Event to stop EVENT

```sql
DROP EVENT event_total_product_report_minute;
```