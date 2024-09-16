# Advance Stored Proceure

## 1 - Config database for Website BabyShop

* Import SQL script file ***db_babyshop.sql*** to Create Database ***db_babyshop***

## 2 - Advance Store Procedure 

* Get All products newest

```sql
CREATE PROCEDURE sp_layDanhSachSanPham()
Begin
    SELECT s.*, l.TenLoaiSanPham, h.TenHangSanXuat 
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    ORDER BY s.MaLoaiSanPham;
END;
-- execute store procedure --
Call sp_layDanhSachSanPham;
```

* Get 5 newest product
``` sql
CREATE PROCEDURE sp_lay_05_SanPham_MoiNhat()
Begin
    SELECT s.*, l.TenLoaiSanPham, h.TenHangSanXuat 
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    ORDER BY s.NgayNhap DESC
    LIMIT 5;
END;
```

* Get 5 bestsell product
```sql
CREATE PROCEDURE sp_lay_05_SanPham_BanNhieuNhat()
Begin
    SELECT s.*, l.TenLoaiSanPham, h.TenHangSanXuat 
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    ORDER BY s.SoLuongBan DESC
    LIMIT 5;
END;

Call sp_lay_05_SanPham_BanNhieuNhat;
```

* Get 5 hottest products
```sql
CREATE PROCEDURE sp_lay_05_SanPham_QuanTam()
Begin
    SELECT s.*, l.TenLoaiSanPham, h.TenHangSanXuat 
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    ORDER BY s.SoLuocXem DESC
    LIMIT 5;
END;

Call sp_lay_05_SanPham_BanNhieuNhat;
```

* Get product by ID
```sql
CREATE PROCEDURE sp_layThongTinSanPham(
    MaSanPham INT
)
Begin
    SELECT  s.*, l.TenLoaiSanPham, h.TenHangSanXuat 
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    WHERE s.MaLoaiSanPham = MaLoaiSanPham AND BiXoa = 0;
END;

Call sp_layThongTinSanPham(1);
```

```sql
CREATE PROCEDURE sp_layThongTinSanPham(
    MaSanPham INT
)
Begin
    SELECT  s.*, l.TenLoaiSanPham, h.TenHangSanXuat 
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    WHERE s.MaLoaiSanPham = MaSanPham AND s.BiXoa = 0;
END;

Call sp_layThongTinSanPham(1);
```

```sql
CREATE PROCEDURE sp_layThongTinSanPhamTheoHang(
    In MaHangSanXuat INT
)
Begin
    SELECT  s.*, l.TenLoaiSanPham, h.TenHangSanXuat 
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    WHERE s.MaHangSanXuat = MaHangSanXuat AND s.BiXoa = 0;
END;

Call sp_layThongTinSanPham(1);
```

* Get account which don't have transaction
```sql
CREATE PROCEDURE sp_layDanhSachCacTaiKhoanChuaThucHienGiaoDich()
Begin
    SET @ds_taikhoan_cogiaodich := (SELECT t.MaTaiKhoan FROM TaiKhoan t INNER JOIN DonDatHang d ON t.MaTaiKhoan = d.MaTaiKhoan WHERE t.BiXoa = 0 GROUP By MaTaiKhoan);
    SELECT * FROM TaiKhoan WHERE MaTaiKhoan NOT IN (@ds_taikhoan_cogiaodich);
END;

Call sp_layDanhSachCacTaiKhoanChuaThucHienGiaoDich;
```

## 3 - Trigger

Syntax of Trigger

```sql
CREATE TRIGGER trigger_name trigger_time trigger_event
    ON table_name
    FOR EACH ROW
BEGIN

END
```

* ***trigger_name*** is name of the trigger
* ***trigger_time*** is the time of the trigger
    * ***BEFORE*** set execute trigger before data change
    * ***AFTER*** set execute trigger after data change
* ***trigger_event*** is the follow of the event to change data such as INSERT, UPDATE, DELETE
* keyword ***OLD*** is to reference data before changed
* keyword ***NEW*** is to reference data after changed

First Example: create a trigger to insert ***NgayLap, TongThanhTien, MaTinhTrang*** before insert data to table.

```sql
DROP TRIGGER IF EXISTS `trg_after_insert_user_dondathang`;
CREATE TRIGGER `trg_after_insert_user_dondathang` BEFORE INSERT ON `DonDatHang`
FOR EACH ROW
BEGIN
    SET NEW.NgayLap = NOW();
    SET NEW.TongThanhTien = 0;
    SET NEW.MaTinhTrang = 1;
END;

------------------------------------------------------------

SELECT * FROM DonDatHang;

INSERT INTO DonDatHang(MaDonDatHang, MaTaiKhoan) VALUES ('240911002', 1);
```

Second Example: when insert product information to ***ChiTietDonHang***, using Trigger to update ***TongThanhTien*** with ***TongThanhTien*** += ***GiaSanPham*** * ***SoLuong***

```sql
DROP TRIGGER IF EXISTS `trg_after_inser_chitietdondathang`;
CREATE TRIGGER `trg_after_insert_chitietdondathang` AFTER INSERT ON `ChiTietDonDatHang`
FOR EACH ROW
BEGIN
    UPDATE DonDatHang
    SET TongThanhTien = TongThanhTien + (NEW.GiaBan * NEW.SoLuong)
    WHERE MaDonDatHang = NEW.MaDonDatHang;
END;

------------------------------------------------------------
SELECT * FROM DonDatHang;

INSERT INTO ChiTietDonDatHang(MaChiTietDonDatHang, MaDonDatHang, MaSanPham, SoLuong, GiaBan) VALUES ('24091100101', '240911001', 9, 2, 380000);

SELECT * FROM DonDatHang;

SELECT * FROM ChiTietDonDatHang;
```

> Note: the INSERT Event has only NEW data.

Third Example: when update ***SoLuong*** on ***ChiTietDonHang***, using Trigger to update ***TongThanhTien*** with ***TongThanhTien*** += ***GiaSanPham*** * ***SoLuong***

```sql
DROP TRIGGER IF EXISTS `trg_after_update_chitietdondathang`;
CREATE TRIGGER `trg_after_update_chitietdondathang` AFTER UPDATE ON `ChiTietDonDatHang`
FOR EACH ROW
BEGIN
    UPDATE DonDatHang
    SET TongThanhTien = TongThanhTien + (NEW.GiaBan * (NEW.SoLuong - OLD.SoLuong))
    WHERE MaDonDatHang = NEW.MaDonDatHang;
END;

-----------------------------------------------------------------------------------------
SELECT * FROM DonDatHang;

UPDATE ChiTietDonDatHang
SET SoLuong = 5
WHERE MaChiTietDonDatHang = '24091100101';

SELECT * FROM DonDatHang;

SELECT * FROM ChiTietDonDatHang;
```

FOURTH Example: when Delete ***ChiTietDonDatHang*** on ***ChiTietDonHang***, using Trigger to update ***TongThanhTien*** with ***TongThanhTien*** += ***GiaSanPham*** * ***SoLuong***

```sql
DROP TRIGGER IF EXISTS `trg_after_delete_chitietdondathang`;
CREATE TRIGGER `trg_after_delete_chitietdondathang` AFTER DELETE ON `ChiTietDonDatHang`
FOR EACH ROW
BEGIN
    UPDATE DonDatHang
    SET TongThanhTien = TongThanhTien - (OLD.GiaBan * OLD.SoLuong)
    WHERE MaDonDatHang = OLD.MaDonDatHang;
END;

-----------------------------------------------------------------------------------------
SELECT * FROM DonDatHang;

DELETE FROM ChiTietDonDatHang
WHERE MaChiTietDonDatHang = '24091100101';

SELECT * FROM DonDatHang;

SELECT * FROM ChiTietDonDatHang;
```

> Note: the Delete Event has only OLD data.