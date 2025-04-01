# Bài thực hành số 1 - Tạo CSDL quản lý trung tâm ngoại ngữ

## 1. Tạo lược đồ cơ sở dữ liệu với các ràng buộc

```sql
-- Tạo bảng CHUONGTRINH
CREATE TABLE CHUONGTRINH (
    MACT VARCHAR(10) PRIMARY KEY,
    TENCT NVARCHAR(100) NOT NULL
);

-- Tạo bảng KHOAHOC
CREATE TABLE KHOAHOC (
    MAKH VARCHAR(10) PRIMARY KEY,
    TENKH NVARCHAR(100) NOT NULL,
    NGAYBD DATE NOT NULL,
    NGAYKT DATE NOT NULL,
    CONSTRAINT CHK_NGAYKT CHECK (NGAYKT > NGAYBD)
);

-- Tạo bảng LOAILOP
CREATE TABLE LOAILOP (
    MALOAI VARCHAR(10) PRIMARY KEY,
    MACT VARCHAR(10) NOT NULL,
    TENLOAI NVARCHAR(50) NOT NULL,
    CONSTRAINT FK_LOAILOP_CHUONGTRINH FOREIGN KEY (MACT) REFERENCES CHUONGTRINH(MACT)
);

-- Tạo bảng MONHOC
CREATE TABLE MONHOC (
    MAMH VARCHAR(10) PRIMARY KEY,
    TENMH NVARCHAR(100) NOT NULL
);

-- Tạo bảng HOCVIEN
CREATE TABLE HOCVIEN (
    MAHV VARCHAR(10) PRIMARY KEY,
    TENHV NVARCHAR(100) NOT NULL,
    GIOITINH BIT NOT NULL, -- 0: Nam, 1: Nữ
    NGAYSINH DATE NOT NULL,
    SDT VARCHAR(15),
    DIACHI NVARCHAR(200),
    CONSTRAINT CHK_GIOITINH CHECK (GIOITINH IN (0, 1))
);

-- Tạo bảng LOP
CREATE TABLE LOP (
    MALOP VARCHAR(10) PRIMARY KEY,
    MALOAI VARCHAR(10) NOT NULL,
    TENLOP NVARCHAR(50) NOT NULL,
    SISO INT NOT NULL,
    MAKH VARCHAR(10) NOT NULL,
    CONSTRAINT FK_LOP_LOAILOP FOREIGN KEY (MALOAI) REFERENCES LOAILOP(MALOAI),
    CONSTRAINT FK_LOP_KHOAHOC FOREIGN KEY (MAKH) REFERENCES KHOAHOC(MAKH),
    CONSTRAINT CHK_SISO CHECK (SISO > 12)
);

-- Tạo bảng PHIEUTHU
CREATE TABLE PHIEUTHU (
    SOPT VARCHAR(10) PRIMARY KEY,
    MAHV VARCHAR(10) NOT NULL,
    MALOP VARCHAR(10) NOT NULL,
    NGAYLAPPHIEU DATE NOT NULL,
    THANHTIEN DECIMAL(12,2) NOT NULL,
    CONSTRAINT FK_PHIEUTHU_HOCVIEN FOREIGN KEY (MAHV) REFERENCES HOCVIEN(MAHV),
    CONSTRAINT FK_PHIEUTHU_LOP FOREIGN KEY (MALOP) REFERENCES LOP(MALOP),
    CONSTRAINT CHK_THANHTIEN CHECK (THANHTIEN > 0)
);

-- Tạo bảng DIEM
CREATE TABLE DIEM (
    MAMH VARCHAR(10),
    MAHV VARCHAR(10),
    MALOP VARCHAR(10),
    DIEM DECIMAL(4,2) NOT NULL,
    PRIMARY KEY (MAMH, MAHV, MALOP),
    CONSTRAINT FK_DIEM_MONHOC FOREIGN KEY (MAMH) REFERENCES MONHOC(MAMH),
    CONSTRAINT FK_DIEM_HOCVIEN FOREIGN KEY (MAHV) REFERENCES HOCVIEN(MAHV),
    CONSTRAINT FK_DIEM_LOP FOREIGN KEY (MALOP) REFERENCES LOP(MALOP),
    CONSTRAINT CHK_DIEM CHECK (DIEM >= 0 AND DIEM <= 10)
);
```

## 2. Thêm dữ liệu vào các bảng

```sql
-- Thêm dữ liệu vào bảng CHUONGTRINH
INSERT INTO CHUONGTRINH VALUES ('CT001', N'Tiếng Anh giao tiếp');
INSERT INTO CHUONGTRINH VALUES ('CT002', N'Tiếng Anh chuyên ngành');

-- Thêm dữ liệu vào bảng KHOAHOC
INSERT INTO KHOAHOC VALUES ('K001', N'Khóa hè 2023', '2023-06-01', '2023-08-31');
INSERT INTO KHOAHOC VALUES ('K002', N'Khóa thu 2023', '2023-09-01', '2023-12-31');

-- Thêm dữ liệu vào bảng LOAILOP
INSERT INTO LOAILOP VALUES ('LL001', 'CT001', N'Lớp cơ bản');
INSERT INTO LOAILOP VALUES ('LL002', 'CT002', N'Lớp nâng cao');

-- Thêm dữ liệu vào bảng MONHOC
INSERT INTO MONHOC VALUES ('MH001', N'Ngữ pháp cơ bản');
INSERT INTO MONHOC VALUES ('MH002', N'Nghe hiểu');

-- Thêm dữ liệu vào bảng HOCVIEN
INSERT INTO HOCVIEN VALUES ('HV001', N'Nguyễn Văn An', 0, '2000-05-15', '0912345678', N'123 Lê Lợi, Q1, TP.HCM');
INSERT INTO HOCVIEN VALUES ('HV002', N'Trần Thị Bình', 1, '2001-08-20', '0987654321', N'456 Nguyễn Huệ, Q1, TP.HCM');
INSERT INTO HOCVIEN VALUES ('HV0012', N'Lê Văn Cường', 0, '1999-03-10', '0905123456', N'789 Lý Tự Trọng, Q1, TP.HCM');

-- Thêm dữ liệu vào bảng LOP
INSERT INTO LOP VALUES ('L001', 'LL001', N'Lớp 1', 15, 'K001');
INSERT INTO LOP VALUES ('L002', 'LL001', N'Lớp 2', 20, 'K001');
INSERT INTO LOP VALUES ('L003', 'LL002', N'Lớp 3', 18, 'K002');

-- Thêm dữ liệu vào bảng PHIEUTHU
INSERT INTO PHIEUTHU VALUES ('PT00001', 'HV001', 'L001', '2023-06-05', 1350000);
INSERT INTO PHIEUTHU VALUES ('PT00002', 'HV002', 'L002', '2023-06-10', 1350000);

-- Thêm dữ liệu vào bảng DIEM
INSERT INTO DIEM VALUES ('MH001', 'HV001', 'L001', 8.5);
INSERT INTO DIEM VALUES ('MH002', 'HV001', 'L001', 7.5);
INSERT INTO DIEM VALUES ('MH001', 'HV002', 'L002', 9.0);
```

## 3. Thêm dòng dữ liệu vào PHIEUTHU

```sql
INSERT INTO PHIEUTHU VALUES ('PT00008','HV0012','L001','06-02-2021',1350000);
```

**Giải thích**: Dòng này có thể thêm vào được nếu:
1. Học viên 'HV0012' tồn tại trong bảng HOCVIEN
2. Lớp 'L001' tồn tại trong bảng LOP
3. Không có phiếu thu nào trùng khóa chính 'PT00008'

Nếu các điều kiện trên đều thỏa mãn thì dòng dữ liệu sẽ được thêm vào.

## 4. Thêm dòng dữ liệu vào LOP

```sql
INSERT INTO LOP VALUES ('L004','LL002','Lớp 4',10,'K001');
```

**Giải thích**: Dòng này KHÔNG thể thêm vào được vì vi phạm ràng buộc CHECK (SISO > 12). Giá trị SISO = 10 không thỏa mãn điều kiện này.

## 5. Xóa khóa học có mã 'K001'

```sql
DELETE FROM KHOAHOC WHERE MAKH = 'K001';
```

**Giải thích**: Khóa học này KHÔNG thể xóa được vì có các bản ghi trong bảng LOP tham chiếu đến nó (thông qua khóa ngoại MAKH). Điều này vi phạm ràng buộc toàn vẹn tham chiếu.

## 6. Xóa khóa học có mã 'K002'

```sql
DELETE FROM KHOAHOC WHERE MAKH = 'K002';
```

**Giải thích**: Khóa học này có thể xóa được nếu không có bản ghi nào trong bảng LOP tham chiếu đến nó (MAKH = 'K002'). Nếu có bất kỳ lớp nào thuộc khóa học này, thao tác xóa sẽ không thành công.

## 7. Giảm giá trị cột thành tiền của phiếu thu 000001 xuống 10%

```sql
UPDATE PHIEUTHU 
SET THANHTIEN = THANHTIEN * 0.9 
WHERE SOPT = 'PT00001';
```

## 8. Thêm cột hocphi vào bảng LOP và cập nhật giá trị

```sql
-- Thêm cột hocphi
ALTER TABLE LOP ADD HOCPHI DECIMAL(12,2);

-- Cập nhật giá trị hocphi
UPDATE LOP 
SET HOCPHI = CASE 
    WHEN MALOAI = 'LL001' THEN 1350000
    WHEN MALOAI = 'LL002' THEN 1650000
END;
```

## 9. Tạo bảng HOCVIEN_NAM

```sql
CREATE TABLE HOCVIEN_NAM (
    MAHV VARCHAR(10) PRIMARY KEY,
    TENHV NVARCHAR(100) NOT NULL,
    SDT VARCHAR(15),
    NGAYSINH DATE NOT NULL,
    DIACHI NVARCHAR(200),
    CONSTRAINT FK_HOCVIEN_NAM_HOCVIEN FOREIGN KEY (MAHV) REFERENCES HOCVIEN(MAHV)
);
```

## 10. Lấy dữ liệu từ HOCVIEN thêm vào HOCVIEN_NAM

```sql
INSERT INTO HOCVIEN_NAM (MAHV, TENHV, SDT, NGAYSINH, DIACHI)
SELECT MAHV, TENHV, SDT, NGAYSINH, DIACHI
FROM HOCVIEN
WHERE GIOITINH = 0;
```

## 11. Xóa bảng KHOAHOC

```sql
DROP TABLE KHOAHOC;
```

**Giải thích**: Bảng này KHÔNG thể xóa được vì có các bảng khác (LOP) tham chiếu đến nó thông qua khóa ngoại. Cần phải xóa các bảng phụ thuộc trước hoặc xóa các ràng buộc khóa ngoại trước khi xóa bảng KHOAHOC.

## 12. Xóa bảng HOCVIEN_NAM

```sql
DROP TABLE HOCVIEN_NAM;
```

**Giải thích**: Bảng này có thể xóa được vì không có bảng nào tham chiếu đến nó.

## 13. Đổi kiểu dữ liệu của cột tenMH

```sql
ALTER TABLE MONHOC ALTER COLUMN TENMH NVARCHAR(100);
```
