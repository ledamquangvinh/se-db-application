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
```

-- execute store procedure --
```sql
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

