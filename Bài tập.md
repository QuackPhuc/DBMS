# 1
CHUYENDI(**MACD**, TENCD, NGAYBD, MATHUQUY, TONGNGANSACH, LINK, MABIMAT)
F1: LINK, MABIMAT -> MACD, TENCD, NGAYBD, MATHUQUY, TONGNGANSACH

THANHVIEN(**MATV**, TENTV, MACD)
F2:MATV -> TENTV

THANHTOAN(**MATT**, MAMUCCHI, NOIDUNG, SOTIEN, MATV)
F3: MATT -> MAMUCCHI, NOIDUNGM SOTIEN, MATV

MUCCHI(**MAMUCCHI**, TENMUCCHI)
F4: MAMUCCHI -> TENMUCCHI

## 1 - TRUY VẤN
#### a) Liệt kê các thành viên của các chuyến đi không có thủ quỷ. Output: MACD, TENCD, MATV, TENTV (relational algebra)
$$\begin{align*} 
&C1: \\
&\pi_{_{MACD, TENCD, MATV, TENTV}} \sigma_{_{MATHUQUY = null}}(CHUYENDI \Join THANHVIEN) \\ \\ \\
&C2: \\
&R1 \leftarrow CHUYENDI \Join THANHVIEN \\
&\pi_{_{MACD, TENCD, MATV, TENTV}}(R1 - (R1 \Join_{_{R1.MATHUQUY = THANHVIEN.MATV}} THANHVIEN))\\
\end{align*}$$
#### b) Tính tổng tiền cho từng thành viên đã thanh toán cho mục chi "Ăn uống" trong chuyến đi có mã là: CD001. Output: MATV, TENTV, TONGTIEN (relational algebra)
$$
_{MATV, TENTV}ℱ_{SUM(SOTIEN)}\sigma_{MACD = 'CD001' \wedge TENMUCCHI = 'Ăn \ uống'}(THANHVIEN \Join THANHTOAN \Join MUCCHI)
$$
#### c) Tìm các mục chi cần thiết trong tất cả các chuyến đi sau năm 2018 (Mục chi cần thiết là mục được chi trong tất cả các chuyến đi sau 2018). Thông tin kết xuất bao gồm: MAMUCCHI, TENMUCCHI (SQL)
```SQL
# Mục chi cần thiết là mục mà tất cả các chuyến đi sau 2018 đều có.
# Truy vấn trở thành "Tìm mục chi mà tất cả các chuyến đi sau 2018 đều có"
# Mục chi mà KHÔNG CÓ chuyến đi nào sau 2018 KHÔNG CÓ mục chi đó

SELECT MC.*
FROM MUCCHI MC
WHERE NOT EXISTS 
( SELECT *
  FROM CHUYENDI CD
  WHERE YEAR(CD.NGAYBD) > 2018 AND NOT EXISTS 
  ( SELECT *
    FROM THANHVIEN TV
    JOIN THANHTOAN TT ON TV.MATV = TT.MATV
    WHERE TT.MAMUCCHI = MC.MAMUCCHI AND TT.MACD = CD.MACD
  )
)
```
## 2
Hãy phát biểu bối cảnh, nội dung, và bảng tầm ảnh hưởng cho ràng buộc toàn vẹn sau (1.0 điểm): 
> “Tổng các khoản thanh toán cho 1 chuyến đi phải nhỏ hơn hoặc bằng tổng ngân sách của chuyến đi đó.”

Context: CHUYENDI, THANHTOAN, THANHVIEN
Content:
- Normal Language: Tổng các khoản thanh toán cho 1 chuyến đi phải nhỏ hơn hoặc bằng tổng ngân sách của chuyến đi đó.
- Formal Language: $$\begin{align*}&(\forall t)(CHUYENDI(t) \ \wedge \\ &((\forall s)(THANHVIEN(s) \wedge s.MACD = t.MACD) \ \wedge \\ &((\forall u)(THANHTOAN(u) \wedge u.MATV = s.MATV) \ \wedge \\ &t.TONGNGANSACH \geq sum(u.SOTIEN))\end{align*}$$
Influence Table:

|           | insert | delete | modify          |
| --------- | ------ | ------ | --------------- |
| CHUYENDI  | +      | -      | +(TONGNGANSACH) |
| THANHVIEN | -      | -      | +(MACD)         |
| TT        | +      | -      | +(MATV, SOTIEN) |

## 3
$R(A,C,B,D,E,F,G,H), PK = {AC}, F = \{A \rightarrow BD; C \rightarrow FG; AC \rightarrow E; G \rightarrow H\}$
#### a) Chỉ ra những chỗ trùng lắp trên lược đồ
Phụ thuộc 1 phần:
```python
A -> B
A -> D
C -> F
C -> G
```
Phụ thuộc bắc cầu:
```python
G -> H
```
#### b) Lược đồ trên dạng chuẩn mấy. Giải thích.
Lược đồ trên dạng 1NF, vì còn có phụ thuộc 1 phần
#### c) Chuẩn hóa về BCNF
$$\begin{align*} 
&R_1(A, B, D) \ - \ PK_1(A)\\
&R_2(C, F, G) \ - \ PK_2(C)\\ 
&R_3(A, C, E) \ - \ PK_3(A, C) \\
&R_4(G, H) \ - \ PK_4(G)
\end{align*}$$

# 2
NHANVIEN(MANV, TENNV, PHAI, MAPB)
ĐEAN(MAĐA, TENĐA, CHIPHIĐA, MAPB)
CONGVIEC(MACV, TENCV, CHIPHICV)
ĐEAN_CV (MAĐA, MACV, THOIGIAN_QĐ)
THAMGIA (MANV, MAĐA, MACV, THOIGIAN)

## 1 - TRUY VẤN
#### a) Cho biết mã và tên của những đề án có ít nhất là hai công việc và ít nhất hai nhân viên tham gia (relational algebra)
$$\begin{align*}
&r_1(MADA, TENDA, CV\_C, NV\_C) \leftarrow _{_{MADA, TENDA}} ℱ_{_{COUNT(MACV), COUNT(MANV)}}(THAMGIA \Join DEAN) \\
&KQ \leftarrow \pi_{_{MADA, TENDA}}\sigma_{_{CV\_C \geq 2 \  \wedge \ NV\_C \geq 2}}(r_1)
\end{align*}$$
#### b) Danh sách mã và tên nhân viên đã tham gia các đề án do chính phòng ban mà nhân viên trực thuộc phụ trách (TRC)
$$\begin{align*}
&\{ \ t.MANV, t.TENNV \ \ | \  \ (NHANVIEN(t) \ \wedge \\
&(\exists s)THAMGIA(s) \ \wedge \ t.MANV = s.MANV \ \wedge \\
&(\exists u)DEAN(u) \ \wedge \ s.MADV = u.MADA \ \wedge \ \\
&u.MAPB = t.MAPB\ \}
\end{align*}$$
#### c) Cho biết mã và tên của những công việc thuộc ít nhất 2 đề án khác nhau (SQL)
```SQL
SELECT CV.MACV, CV.TENCV
FROM CONGVIEC CV
JOIN DEAN_CV DA ON CV.MACV = DA.MACV
GROUP BY CV.MACV, CV.TENV
HAVING COUNT(DISTINCT DA.MADA) >= 2
```

## 2
Hãy phát biểu bối cảnh, nội dung, và bảng tầm ảnh hưởng cho ràng buộc toàn vẹn sau (1 điểm): 
>“Chi phí thực hiện một đề án bằng tổng chi phí thực hiện các công việc thuộc đề án đó”

Context: DEAN, CONGVIEC, DEANCV
Content:
$$\begin{align*}
&(\forall t)(DEAN(t) \ \wedge \\
&(\forall s)DEAN\_CV(s) \ \wedge \ t.MADA = s.MADA \ \wedge \\
&(\forall u)CONGVIEC(u) \ \wedge \ s.MACV = u.MACV \ \wedge \\
&t.CHIPHIDA = sum(u.CHIPHICV))
\end{align*}$$
Influence Table

|          | insert | delete | modify        |
| -------- | ------ | ------ | ------------- |
| DEAN     | +      | +      | +(CHIPHIDEAN) |
| CONGVIEC | -      | +      | +(CHIPHICV)   |
| DEAN_CV  | +      | +      | +(MADA, MACV) |

## 3
$$\begin{align*}
&LICHBIEU(MONHOC, SACH\_THAM\_KHAO, BOMON, GIANGVIEN) \\
&PK ={(MONHOC, SACH\_THAM\_KHAO)} \\
&F = \{MONHOC \rightarrow GIANGVIEN; GIANGVIEN \rightarrow BOMON\}
\end{align*}$$
#### a) Lược đồ LICHBIEU đạt chuẩn mấy. Giải thích.
Phụ thuộc 1 phần
```python
MONHOC -> GIANGVIEN
MONHOC -> BOMON # bắc cầu qua GIANGVIEN
```
Phụ thuộc bắc cầu
```python
GIANGVIEN -> BOMON
```
- LICHBIEU đạt chuẩn 1NF, vì còn phụ thuộc bắc 1 phần.
#### b) Chuẩn hóa về BCNF
$$\begin{align*}
&R_1(MONHOC, GIANGVIEN) \\
&R_2(GIANGVIEN, BOMON) \\
&R_3(MONHOC, SACH\_THAM\_KHAO)
\end{align*}$$

# 3
**GOI_THAU**(*MA_GT*, TEN_GT, NGAYMO, NGAYDONG, MA_SP)
**SANPHAM_THAU**(*MA_SP*, TEN_SP, THONGSO_KT, LOAI_SP, GIA_DUKIEN)
**NHATHAU**(*MA_NT*, TEN_NT, DCHI_NT, DTHOAI_NT, NANG_LUC, MALOAI_NT, TENLOAI_NT)
**HOSO_THAU**(*MA_HST*, MA_NT, MA_GT, NGAY_HST, GIA_HST, TRUNG_THAU, DIEM_HST)
## 1 - TRUY VẤN (relational algebra)
#### a) Liệt kê các mã và tên gói thầu (MA_GT, TEN_GT) được mở và?o ngày 01/03/2022 và có liên quan đến sản phẩm có giá thầu dự kiến (GIA_DUKIEN) < 2.5 tỉ đồng
$$\begin{align*}
\pi_{_{MAGT, TENGT}}\sigma_{_{NGAYMO = '01/03/2022' \ \wedge \ GIA\_DUKIEN < 2.5e9}}(GOI\_THAU \Join SANPHAM\_THAU)
\end{align*}$$
#### b) Cho biết thông tin các nhà thầu (MA_NT, TEN_NT) đã đã có hơn 3 hồ sơ trúng thầu
$$\begin{align*}
{\pi_{_{MA\_NT, TEN\_NT}}}\sigma_{_{COUNT\_MAHST \geq 3}}(\ _{_{MA\_NT, TEN\_NT}}ℱ_{_{COUNT(MA\_HST)}} \sigma_{_{TRUNG\_THAU = 'Yes'}}(NHATHAU \Join HOSO\_THAU))
\end{align*}$$
## 2 - TRUY VẤN SQL
#### a) Cho biết các nhà thầu (MA_NT, TEN_NT) thuộc loai (TENLOAI_NT) = “trung bình” và tên sản phẩm (TEN_SP) mà nhà thầu trúng gói thầu được mở trong năm 2021
```SQL
SELECT NT.MANT, NT.TENNT, SP.TEN_SP
FROM NHATHAU NT
JOIN HOSO_THAU HS ON HS.MANT = NT.MANT
JOIN GOI_THAU GT ON NT.MAGT = HS.MAGT
JOIN SAMPHAM_THAU SP ON NT.MASP = GT.MASP
WHERE NT.TENLOAI_NT = 'TRUNG BÌNH' AND YEAR(GT.NGAYMO) = 2021 AND HS.TRUNGTHAU = 'Yes'
```

#### b) Cho biết thông tin nhà thầu (MA_NT, TEN_NT) có số lần trúng thầu nhiều nhất với các gói thầu < 3 tỉ
```SQL
SELECT NT.MANT, NT.TENNT
FROM NHATHAU NT
JOIN HOSO_THAU HS ON NT.MANT = HS.MANT
WHERE HS.GIA_HST < 3e9 AND HS.TRUNG_THAU = 'Yes'
GROUP BY NT.MANT, NT.TENNT
HAVING COUNT(HS.MA_HST) = (SELECT TOP 1 COUNT(MA_HST)
                          FROM HOSOTHAU 
                          WHERE GIA_HST < 3e9 AND TRUNG_THAU = 'Yes'
                          GROUP BY MA_NT
                          ORDER BY COUNT(MA_HST) DESC;)


SELECT NT.MA_NT, NT.TEN_NT, COUNT(*) AS SL_TT 
FROM NHATTHAU NT JOIN HOSO_THAU HST ON HST.MA_NT = NT.MA_NT 
WHERE HST.TRUNG_THAU = ‘Yes’ AND HST.GIA_HST < 3.10^9 
GROUP BY NT.MA_NT, NT.TEN_NT 
HAVING COUNT(*) >= ALL(SELECT COUNT(*)  
                       FROM NHATTHAU NT1 JOIN HOSO_THAU HST1 ON HST1.MA_NT = NT1.MA_NT 
                       WHERE HST1.TRUNG_THAU = ‘Yes’ AND HST1.GIA_HST < 3.10^9 
                       GROUP BY NT1.MA_NT)
```
## 3
Hãy mô tả bối cảnh, nội dung và bảng tầm ảnh hưởng của các ràng buộc được phát biểu dưới đây:
>Ngày nộp hồ sơ thầu (NGAY_HST) của một gói thầu phải thuộc khoảng ngày mở (NGAYMO) và ngày đóng (NGAYDONG) của gói thầu đó

Context: HOSO_THAU, GOI_THAU
Content: 
$$\begin{align*}
&(\forall t)(HOSO\_THAU(t) \ \wedge \\
&(\exists s)GOI\_THAU(s) \ \wedge \ t.MA\_GT = s.MA\_GT \ \wedge \\
&s.NGAYMO \leq t.NGAY\_HST \ \wedge \ s.NGAYDONG \geq t.NGAY\_HST)
\end{align*}$$
Influence Table:

|           | insert | delete | modify              |
| --------- | ------ | ------ | ------------------- |
| HOSO_THAU | +      | -      | +(NGAY_HST)         |
| GOI_THAU  | -      | +      | +(NGAYMO, NGAYDONG) |

>Mỗi gói thầu có nhiều nhất 1 hồ sơ trúng thầu (có thể có nhiều hồ sơ nộp thầu)

Context: HOSO_THAU
Content:
$$\begin{align*}
&(\forall t)(GOI\_THAU(t) \ \wedge \\
((&(\nexists u)(HOSO\_THAU(u) \ \wedge \ s.MA\_GT = t.MA\_GT) \ \vee\\ 
(&(\exists !s)(HOSO\_THAU(s) \ \wedge \ s.MA\_GT = t.MA\_GT \ \wedge \ s.TRUNG\_THAU = 'Yes'))
\end{align*}$$

|           | insert | delete | modify        |
| --------- | ------ | ------ | ------------- |
| HOSO_THAU | +      | -      | +(MADA, MACV) |

## 4
**GOI_THAU**(***MA_GT***, TEN_GT, NGAYMO, NGAYDONG, MA_SP)
F = {f1: MA_GT → TEN_GT, NGAYMO, NGAYDONG, MA_SP}

**SANPHAM_THAU**(***MA_SP***, TEN_SP, THONGSO_KT, LOAI_SP, GIA_DUKIEN)
F = {f2: MA_SP → MA_SP, TEN_SP, THONGSO_KT, LOAI_SP; f3: LOAI_SP → GIA_DUKIEN}

**NHATHAU**(***MA_NT***, TEN_NT, DCHI_NT, DTHOAI_NT, NANG_LUC, MALOAI_NT, TENLOAI_NT)
F = {f4: MA_NT → TEN_NT, DCHI_NT, DTHOAI_NT, NANG_LUC,MALOAI_NT; f5: MALOAI_NT → TENLOAI_NT}

**HOSO_THAU**(***MA_HST***, MA_NT, MA_GT, NGAY_HST, GIA_HST, TRUNG_THAU, DIEM_HST)
F = {f6: MA_HST → MA_NT, MA_GT, NGAY_HST, GIA_HST, TRUNG_THAU, DIEM_HST}

#### a) Chỉ ra điểm trùng lắp thông tin trên lược đồ cơ sở dữ liệu và cho biết lược đồ đạt dạng chuẩn mấy? Giải thích.
Phụ thuộc bắc cầu:
```python
f3: LOAI_SP -> GIA_DUKIEN # LOAI_SP(non-prime)
f5: MALOAI_NT -> TENLOAI_NT # MALOAI_NT(non-prime)
```
Lược đồ GOI_THAU, HOSO_THAU đạt 1NF, lược đồ trên không có phụ thuộc 1 phần => đạt 2NF. Không có phụ thuộc bắc cầu => đạt chuẩn 3NF
Lược đồ SAMPHAM_THAU, NHATHAU đạt 2NF. Vì còn phụ thuộc bắc cầu nên không thể đạt 3NF.

#### b) Chuẩn hóa lược đồ về dạng chuẩn BCNF
Tách các thuộc tính trong phụ thuộc bắc cầu ra thành các lược đồ quan hệ riêng.
```python
R1(MA_GT, TEN_GT, NGAYMO, NGAYDONG, MA_SP)
f1: MA_GT -> TEN_GT, NGAYMO, NGAYDONG, MA_SP

R2(MA_SP, MA_SP, TEN_SP, THONGSO_KT, LOAI_SP)
f2: MA_SP -> MA_SP, TEN_SP, THONGSO_KT, LOAI_SP

R3(LOAI_SP, GIA_DUKIEN)
f3: LOAI_SP -> GIA_DUKIEN

R4(MA_NT, TEN_NT, DCHI_NT, DTHOAI_NT, NANG_LUC, MALOAI_NT)
f4: MA_NT -> TEN_NT, DCHI_NT, DTHOAI_NT, NANG_LUC, MALOAI_NT

R5(MALOAI_NT, TENLOAI_NT)
f5: MALOAI_NT -> TENLOAI_NT

R6(MA_HST, MA_NT, MA_GT, NGAY_HST, GIA_HST, TRUNG_THAU, DIEM_HST)
f6: MA_HST -> MA_NT, MA_GT, NGAY_HST, GIA_HST, TRUNG_THAU, DIEM_HST
```

# 4
**DIENVIEN**(***MADV***, HODV, TEDV, GIOITINH)
**PHIM**(***MAPH***, TENPH, NAM, DOANHTHU)
**DAODIEN**(***MADD***, DODD, TENDD)
**THAMGIA**(***MADV, MAPH***, VAIDIEN)
**THUCHIEN**(***MAPH, MADD***)
## 1 - TRUY VẤN(relational algebra)
#### a) Cho biết họ tên và vai diễn của các diễn viên tham gia trong các bộ phim được sản xuất trong thập niên 1990(từ 1990 - 1999 bao gồm cả 2 năm đó)
$$\begin{align*}
\pi_{_{HODV, TENDV, VAIDIEN}}\sigma_{_{NAM \geq 1990 \wedge NAM \leq 1999}}(DIENVIEN \Join THAMGIA \Join PHIM)
\end{align*}$$
#### b) Cho biết mã số của các cặp diễn viên đã đóng cùng nhau trong ít nhất 2 bộ phim.
$$\begin{align*}
&r_1 \leftarrow \pi_{_{MADV}}(DIENVIEN) \\
&r_2 \leftarrow \pi_{_{MADV}}(DIENVIEN) \\
&r_3(DV1, DV2, PHIM) \leftarrow \pi_{_{r_1.MADV, r_2.MADV, MAPH}}(r_1 \Join r_2 \Join PHIM) \\
&KQ \leftarrow {\sigma_{_{COUNT\_PHIM \geq 2}}} \ (_{_{DV1, DV2}}ℱ_{_{COUNT(PHIM)}}(r_3))
\end{align*}$$
## 2 - Xác định và mô tả mộ ràng buộc toàn vẹn của cơ sở dữ liệu trên.
#### a) Mỗi diễn viên có một mã số duy nhất, họ tên, giới tính.
Context: DIENVIEN
Content:
$$\begin{align*}
&(\forall t)(DIENVIEN(t) \ \wedge \ (\nexists s)(DIENVIEN(s) \ \wedge \\
&t.MADV = s.MADV \ \wedge \\
&(t.HODV \neq s.HODV \ \vee \ t.TENDV \neq s.TENDV \ \vee \ t.GIOITINH \neq s.GIOITINH )))
\end{align*}$$
#### b) Mỗi phim, đạo diễn có một xxx duy nhất...
...
#### c) Mỗi diễn viên tham gia vai diễn trong nhiều phim, mỗi phim có nhiều diễn viên tham gia
Context: DIENVIEN, THAMGIA, PHIM
Content:
$$\begin{align*}
&((\forall t)(DIENVIEN(t) \ \wedge \ (\exists s)(THAMGIA(s) \ \wedge \ t.MADV = s.MADV))) \\
&\wedge \\
&((\forall t)(PHIM(t) \ \wedge \ (\exists s)(THAMGIA(s) \ \wedge \ t.MAPH = s.MAPH)))
\end{align*}$$
Influence Table

|          | insert | delete | modify        |
| -------- | ------ | ------ | ------------- |
| DIENVIEN | +      | +      | +(MADV)       |
| THAMGIA  | -      | +      | +(MADV, MAPH) |
| PHIM     | +      | +      | +(MAPH)       |
## 3
**THUEXE**(***MAKH, SOHD***, TENKH, SOGIOTHUE, SOXE, MAUXE)
f1: MAKH, SOHD -> TENKH, SOGIOTHUE, SOXE, MAUXE
f2: MAKH -> TENKH
f3: SOXE -> MAUXE

Phụ thuộc 1 phần:
```python
f2: MAKH -> TENKH
```
Phụ thuộc bắc cầu:
```python
f3: SOXE -> MAUXE # (SOXE là non-prime attribute)
```
#### a) Quan hệ THUEXE có ở dạng 3NF không? Giải thích.
Quan hệ thuê xe ở dạng chuẩn 1NF. Vì còn phụ thuộc 1 phần và phụ thuộc bắc cầu nên THUEXE không ở dạng 3NF.

#### b) Phân tách thành dạng 3NF
Tách phụ thuộc 1 phần và phụ thuộc bắc cầu sang  thành các lược đồ quan hệ mới, lúc này lược đồ cơ sở dữ liệu quan hệ trở thành:
```python
R1(MAKH, SOHD, SOGIOTHUE, SOXE)
f1: MAKH, SOHD -> SOGIOTHUE, SOXE

R2(MAKH, TENKH)
f2: MAKH -> TENKH

R3(SOXE, MAUXE)
f3: SOXE -> MAUXE
```

# 5
**TUYENBUYT**(***MATB***, TENTUYEN, CULY, SOCHUYEN, LOAIXE, MADV)
**DVVANHANH**(***MADV***, TENDONVI)
**XEBUYT**(***BIENSO***, LOAIXE, HIEUXE, SOCHONGOI, MATB)
```python
f1: MATB -> TENTUYEN, CULY, SOCHUYEN, LOAIXE, MADV
f2: MADV -> TENDONVI
f3: BIENSO -> LOAIXE, HIEUXE, SOCHONGOI, MATB
f4: LOAIXE, HIEUXE -> SOCHONGOI
```
## 1 - TRUY VẤN (relational algebra)
#### a) Cho biết BIENSO, HIEUXE, SOCHONGOI của các thuyết xe buýt chạy tuyến "Ben Thanh - BX Cho Lon"
$$\begin{align*}
\pi_{_{BIENSO, HIEUXE, SOCHONGOI}} \sigma_{_{TENTUYEN = 'Ben Thanh - BX Cho Lon'}}(XEBUYT \Join TUYENBUYT)
\end{align*}$$

#### b) Với mỗi tuyến buýt cho biết MATB, TENTUYEN, LOAIXE và tổng số xe buýt dùng chở khách trên tuyến đó.
$$\begin{align*}
\pi_{_{MATB, TENTUYEN, LOAIXE, COUNT\_BIENSO}} \ (_{_{MATB, TENTUYEN, LOAIXE}}ℱ_{_{COUNT(BIENSO)}}(XEBUYT \Join TUYENBUYT))
\end{align*}$$

## 2 
Mô tả ràng buộc toàn vẹn sau:
>Mỗi tuyến xe buýt phải có ít nhất 5 xe buýt sử dụng vận chuyển hành khách

Context: TUYENBUYT, XEBUYT
Content:
$$\begin{align*}
&(\forall t)(TUYENBUYT(t) \ \wedge \ (\forall s)(XEBUYT(s) \ \wedge \ t.MATB = s.MATB) \ \wedge \ count(s) \geq 5)
\end{align*}$$
(trong slide thầy dùng card thay cho count, nên dùng card)
$$\begin{align*}
&(\forall t)(TUYENBUYT(t) \ \wedge \ card(\{ s | XEBUYT(s) \ \wedge \ t.MATB = s.MATB\}) \geq 5)
\end{align*}$$
Influence Table:

|           | insert | delete | modify  |
| --------- | ------ | ------ | ------- |
| TUYENBUYT | +      | -      | +(MATB) |
| XEBUYT    | -      | +      | +(MATB) |
## 3
#### a) Cho biết dạng chuẩn cao nhất của các quan hệ TUYENBUYT, DVVANHANH, XEBUYT
Phụ thuộc bắc cầu
```python
f4: LOAIXE, HIEUXE -> SOCHONGOI
```
Các quan hệ TUYENBUYT và DVVANHANH đạt chuẩn BCNF vì không có phụ thuộc 1 phần, phụ thuộc bắc cầu, và các yếu tố quyết định(determinant) trong các phụ thuộc hàm đều là siêu khóa.
Quan hệ  XEBUYT đạt 2NF vì không có phụ thuộc 1 phần nhưng có phụ thuộc bắc cầu.
#### b) Chỉ ra điểm trùng lặp thông tin và đưa về chuẩn BCNF.
Tách phụ thuộc bắc cầu $f_4$ thành một quan hệ mới.
Lược đồ cơ sở dữ liệu quan hệ lúc này trở thành:
```python
R1(MATB, TENTUYEN, CULY, SOCHUYEN, LOAIXE, MADV)
f1: MATB -> TENTUYEN, CULY, SOCHUYEN, LOAIXE, MADV

R2(MADV, TENDONVI)
f2: MADV -> TENDONVI

R3(BIENSO, LOAIXE, HIEUXE, MATB)
f3: BIENSO -> LOAIXE, HIEUXE, MATB

R4(LOAIXE, HIEUXE, SOCHONGOI)
f4: LOAIXE, HIEUXE -> SOCHONGOI
```

# 6
**SINHVIEN**(***MASV, MAHP,* *HOCKY***, HOTEN, MANDT, KHOA, DIEM)
**HOCPHAN**(***MAHP***, TENHP, SOTC, MACTDT, MANDT, KHOA)
```python
f1: MASV -> HOTEN, MANDT, KHOA
f2: MASV, MAHP, HOCKY -> DIEM
f3: MAHP -> TENHP, SOTC, MACTDT, MANDT, KHOA
f4: MACTDT -> MANDT, KHOA
```
## 1 - TRUY VẤN (relational algebra)
#### a) Cho biết MAHP, TENHP, SOTC của các học phần thuộc chương trình đào tạo có mã số là 7460101.
$$\begin{align*}
\pi_{_{MAHP, TENHP, SOTC}}\sigma_{_{MACTDT = '7460101'}}(HOCPHAN)
\end{align*}$$
#### b) Cho MAHP, TENHP, SOTC, HOCKY, DIEM của các học phần mà sinh viên có mã số 1911912 tham dự.
$$\begin{align*}
\pi_{_{MAHP, TENHP, SOTC, HOCKY, DIEM}}\sigma_{_{MASV = '1911912'}}(SINHVIEN \Join HOCPHAN)
\end{align*}$$
## 2. Mô tả ràng buộc toàn vẹn
> Một sinh viên không được tham dự nhiều hơn 8 học phần trong 1 kỳ.

Context: SINHVIEN
Content:
$$\begin{align*}
&R \leftarrow _{_{MASV, HOCKY}}\mathscr{F}_{_{COUNT(MAHP)}}(SINHVIEN) \\
&(\forall t)(R(t) \ \wedge (COUNT\_MAHP \leq 8))
\end{align*}$$
Influence Table:

|          | insert | delete | modify         |
| -------- | ------ | ------ | -------------- |
| SINHVIEN | +      | -      | +(MASV, HOCKY) |
## 3
Phụ thuộc một phần:
```python
f1: MASV -> HOTEN, MANDT, KHOA
```
Phụ thuộc bắc cầu:
```python
f4: MACTDT -> MANDT, KHOA
```
#### a) Cho biết dạng chuẩn cao nhất của SINHVIEN, HOCPHAN
Chuẩn cao nhất trong SINHVIEN, HOCPHAN là 2NF.
Trong đó:
- SINHVIEN đạt 1NF vì còn phụ thuộc một phần.
- HOCPHAN đạt 2NF vì còn phụ thuộc bắc cầu.

#### b) Chỉ ra điểm trùng lặp và chuẩn hóa về BCNF.
Các điểm trùng lặp trong các phụ thuộc một phần và bắc cầu nêu trên.
Cách chuẩn hóa: Tạo quan hệ mới dựa trên các phụ thuộc.
Ta có lược đồ cơ sở dữ liệu mới:
```python
R1(MASV, HOTEN, MANDT, KHOA)
f1: MASV -> HOTEN, MANDT, KHOA

R2(MASV, MAHP, HOCKY, DIEM)
f2: MASV, MAHP, HOCKY -> DIEM

R3(MAHP, TENHP, SOTC, MACTDT)
f3: MAHP -> TENHP, SOTC, MACTDT

R4(MACTDT, MANDT, KHOA)
f4: MACTDT -> MANDT, KHOA
```
