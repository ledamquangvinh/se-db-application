# View in MySQL

## 1 - Basic Syntax
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
## 2 - Advance VIEW implementation

* Cannot Create a VIEW with Paramenter because VIEW is stored staic data after execute queries.

* Example: Create VIEW to count the number of orders.

```sql
CREATE VIEW v_DemTongDonDatHang AS
SELECT COUNT(MaDonDatHang) AS Total
FROM DonDatHang;
```

* Select first time is 7 orders
```sql
SELECT * FROM v_DemTongDonDatHang;
```

* After that, we create new order
```sql
INSERT INTO DonDatHang(MaDonDatHang, NgayLap, TongThanhTien, MaTaiKhoan, MaTinhTrang) 
VALUES ('240916001', NOW(), 1000, 1, 1);
```

* And select the second time, we will have 8 orders. So the view will auto update when have the request to select from view.
```sql
SELECT * FROM v_DemTongDonDatHang;
```

* Now we can use View for caching common data such as:
    * Show top 5 latest products with view ***v_ProductLatest***
    * Show top 5 best sellers with view ***v_ProductBestSeller***
    * Show top 5 hottest products with view ***v_ProductHottestt***

```sql
CREATE OR REPLACE VIEW v_ProductLatest AS
SELECT s.*
FROM SanPham s
ORDER BY NgayNhap DESC
LIMIT 5

------------------------------------------------------

CREATE OR REPLACE VIEW v_ProductBestSeller AS
SELECT s.*
FROM SanPham s
ORDER BY SoLuongBan DESC
LIMIT 5

------------------------------------------------------

CREATE OR REPLACE VIEW v_ProductHottest AS
SELECT s.*
FROM SanPham s
ORDER BY SoLuocXem DESC
LIMIT 5
```
