# View in MySQL

* In SQL VIEW is a virtue table base on the result set of an SQL statement. VIEW contais rows and colums, just like a real table. The field in the VIEW are fields from one or more real tables in database.

* Create View Syntax

```sql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE conditions;
```

* Using VIEW to show the full infomation of product 

```sql
CREATE VIEW HangSanXuat_Lamaze AS
SELECT s.*, TenHangSanXuat
FROM SanPham s LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
WHERE h.TenHangSanXuat = 'Lamaze';
------------------------------------------------------------
SELECT * FROM HangSanXuat_Lamaze;
```
