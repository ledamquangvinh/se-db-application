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

* Edit VIEW to get shop information of product

```sql
CREATE OR REPLACE VIEW HangSanXuat_Lamaze AS
SELECT s.MaSanPham, s.TenSanPham, s.GiaSanPham, h.TenHangSanXuat, l.TenLoaiSanPham
FROM SanPham s INNER JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat INNER JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham
WHERE h.TenHangSanXuat = 'Lamaze';
------------------------------------------------------------
SELECT * FROM HangSanXuat_Lamaze;
```

* Drop a VIEW 

```sql
DROP VIEW HangSanXuat_Lamaze; 
```