Là ngôn ngữ truy vấn thủ tục, trong đó người dùng yêu cầu thông tin từ cơ sở dữ liệu bằng cách yêu cầu một chuỗi các hoạt động được thực hiện trên cơ sở dữ liệu
### Các phép toán đại số quan hệ 
**1. Phép toán trên tập hợp** 
- Hợp: Kết quả của phép hợp bao gồm các tuple có trong $r$ hoặc trong $s$, hoặc trong cả hai quan hệ (loại bỏ các tuple trùng lặp). **Ký hiệu: $r \cup s$** 
- Giao: Kết quả của phép giao bao gồm các tuple có trong cả $r$ và $s$. **Ký hiệu:** **$r \cap s$** 
- Trừ: Kết quả của phép trừ bao gồm các tuple có trong $r$ và không có trong $s$. **Ký hiệu: $r - s$** 
**2. Phép chọn** Phép chọn được sử dụng để chọn một tập hợp con các tuple từ một quan hệ thỏa mãn một điều kiện lựa chọn $P$. 
- **Ký hiệu: $\sigma_P(r)$**
- Ví dụ: $\sigma_{(A=B) \wedge (D > 5)}(r)= \sigma_{A=B}(\sigma_{D>5}(r)) = \sigma_{D>5}(\sigma_{A=B}(r))$
**3. Phép chiếu** Phép chiếu chọn một số cột nhất định từ một quan hệ $r$ và loại bỏ các cột khác. 
- **Ký hiệu: $\pi_{A_1,A_2,...,A_k}(r)$
- $\pi_{A_1, A_2,...,A_n}(\pi_{A_1,A_2,...,A_m}(r)) = \pi_{A_1,A_2,...,A_n}(r)$ với $n \leq m$.
- Ví dụ: $\pi_{HOTEN, LUONG}(\sigma_{PHAI = 'Nữ'}(GIAOVIEN))$ 
``` sql
SELECT HOTEN, LUONG
FROM GIAOVIEN
WHERE PHAI = N'NỮ'
```
**4. Phép tích Descartes** Phép toán này được sử dụng để kết hợp các tuple từ hai quan hệ theo kiểu kết hợp. 
- **Ký hiệu: $r \times s$** 
- Giả sử ***r*** có ***m*** dòng, ***n*** cột; ***s*** có ***m'*** dòng, ***n'*** cột
- Mỗi dòng của $r \times s$ gồm 1 dòng của $r$ và 1 dòng $s$
- $r \times s$ có $m \times m'$ dòng
- $r \times s$ có $n + n'$ cột
**5. Phép nối** Phép nối được sử dụng để xác định và chọn các tuple liên quan từ hai quan hệ. 
- **Ký hiệu: $r \Join s$** 
- Giả sử: $R(A_1, A_2,...,A_n)$ và $S(B_1,B_2,...,B_m)$, $q = r \Join s$ có:
- $m+n$ các giá trị (cột): $Q(A_1,A_2,...,A_n,B_1,B_2,...,B_m)$
- Mỗi dòng của $r \Join s$ là sự kết hợp của 1 dòng trong $r$ và 1 dòng trong $s$ thỏa **điều kiện ghép nối(join condition)**:
    - Điều kiện ghép: $A_i  \ \theta \ B_j$
    - $A_i$ là thuộc tính thuộc R và $B_j$ là thuộc tính thuộc $S$
    - $A_i$ và $B_j$ có cùng miền giá trị
    - $\theta$ là các điều kiện so sánh như $=, \neq, \geq, \leq, >, <$
- Theta join:
    - Ký hiệu: $r \Join_{c} s$, $c$ là điều kiện ghép nối.
    - Truy vấn giáo viên có lương cao hơn giáo viên Nguyễn Văn An$$\begin{align*} &GIAOVIEN(MAGV,HOTEN, LUONG,...) &\\ &R_1(LG) \leftarrow \pi_{LUONG}(\sigma_{HOTEN='Nguyễn \ Văn \ An'}(GIAOVIEN)) &\\ &KQ \leftarrow GIAOVIEN \Join_{LUONG>LG} R_1 &\\ &KQ(MAGV, HOTEN, LUONG,..., LG)\end{align*}$$​
- Equi join:
    - Là trường hợp đặc biệt của theta join mà chỉ dùng phép so sánh bằng '='
    - Với mỗi project, liệt kê giáo viên quản lý project đó$$\begin{align*} &DETAI(MADT,TENDT,KINHPHI,...,GVCNDT) &\\ &GIAOVIEN(MAGV,HOTEN,LUONG,PHAI,...) &\\ \\ &KQ\leftarrow DETAI \Join_{GVCNDT=MAGV}GIAOVIEN &\\ &KQ(MADT,TENDT,KINHPHI,...,GVCNDT,MAGV,HOTEN,...)\end{align*}$$
- Natural join:
    - Là trường hợp đặc biệt của equi join mà tồn tại ít nhất 1 thuộc tính xuất hiện giữa 2 quan hệ. Giá trị ở 2 quan hệ phải cùng tên và cùng miền giá trị.
        - $R_1(A, B, C), R_2(B, E, F)$
        - Có cùng $B$, nếu $dom(B_{R_{1}}) = dom(B_{R_{2}})$ thì **OK**
    - Ký hiệu: $r \Join s$ hoặc $r * s$
    - Với mỗi giáo viên, liệt kê thông tin bộ môn mà giáo viên làm việc $$\begin{align*} &GIAOVIEN(MAGV,HOTEN,LUONG,PHAI,...,MABM,...) &\\ &BOMON(MABM, TENBM, PHONG,...) &\\ \\ &KQ \leftarrow GIAOVIEN \Join BOMON &\\ &KQ(MAGV,HOTEN, ...,MABM,TENBM,PHONG,...\end{align*}$$
**6. Phép chia** Phép chia được sử dụng để trích xuất các tuple trong quan hệ $r(A)$thỏa mãn tất cả các tuple trong quan hệ $s(B)$. Với A và B là tập thuộc tính. 
- **Ký hiệu: $r \div s$** 
- Tập thuộc tính của s phải là tập con của r, hay $B \subseteq A$
- Quan hệ kết quả của phép chia có tập thuộc tính bằng: $A-B$.
- $r \div s$ = các hàng trong r liên kết với tất cả các hàng trong s.
- Ví dụ: ![[division]]
**7. Hàm kết hợp** Một loại yêu cầu không thể được biểu thị trong đại số quan hệ cơ bản là chỉ định các hàm tổng hợp toán học trên các tập hợp giá trị từ cơ sở dữ liệu.  Các hàm phổ biến được áp dụng cho các tập hợp giá trị số bao gồm **SUM, AVERAGE, MAXIMUM** và **MINIMUM**. Hàm **COUNT** được sử dụng để đếm tuple hoặc giá trị. 
**8. Gom nhóm** Gom nhóm các quan hệ thành các nhóm dựa trên điều kiện nhóm.
- **Ký hiệu: $_{G_1, G_2, ..., G_n} ℱ_{F_1(A_1), F_2(A_2), ..., F_n(A_n)} (E)$** 
- $E$ là quan hệ hay biểu thức quan hệ.
- $G_1,G_2,...$ là thuộc tính định nghĩa gom nhóm, hay điều kiện gom nhóm.
- $F_1, F_2,...$ là các hàm kết hợp.
- $A_1, A_2,...$ là các thuộc tính
- Ví dụ: Khoa nào có nhiều giáo viên nhất $$\begin{align*} &\text{Đếm số lượng họ tên theo từng khoa:} \\ &r_1 \leftarrow _{_{MAKHOA,TENKHOA}}ℱ_{_{COUNT(HOTEN)}} (GIAOVIEN * BOMON * KHOA) &\\ \\ &\text{Tính số lượng giáo viên nhiều nhất mà các khoa có thể có:} \\ &r_2(MAX\_COUNT) \leftarrow ℱ_{_{MAX(COUNT\_HOTEN)}}r_1 &\\ \\ &\text{Chọn các khoa có tổng số giáo viên bằng số giáo viên cao nhất:} \\ &KQ \leftarrow \pi_{_{MAKHOA,TENKHOA}}(r_1 \Join_{_{COUNT\_HOTEN \ = \ MAX\_COUNT}}r_2) \end{align*}$$
**9. Nối ngoài** Mở rộng phép toán **JOIN** để tránh mất thông tin. Có 3 kiểu nối ngoài: nối ngoài trái, nối ngoài phải và nối ngoài đầy đủ. 
- ![[join]]
- ![[Join.png]]
**10. Các phép toán cập nhật** Dữ liệu có thể được cập nhật bằng các thao tác chèn, xóa và sửa đổi. **Ký hiệu: $r_{new} \leftarrow$ *relational operations* $r_{old}$**
- Chèn: $r_{new} \leftarrow r_{old} \cup E$
    - $r$ là quan hệ, $E$ là biểu thức đại số quan hệ
    - $THAMGIADT \leftarrow THAMGIADT \cup ('001', '001', 4, 2)$
- Xóa: $r_{new} \leftarrow r_{old} - E$
    - $THAMGIADT \leftarrow THAMGIADT - \sigma_{MAGV='001'}(THAMGIADT)$
- Cập nhật: $r_{new} \leftarrow \pi_{F_1, F_2,...}(r_{old})$
    - $F_i$ là biểu thức tính giá trị mới cho thuộc tính được cập nhật
    - $THAMGIADT \leftarrow \pi_{MAGV, MADT, STT, PHUCAP \ * \ 1.5}(THAMGIADT)$
**11. Phép đổi tên**: cho quan hệ $r(B,C,D)$
- $\rho_s(r)$: đổi tên quan hệ **r** thành **s**.
- $\rho_{X,C,D}(r)$: đổi tên giá trị(cột) **B** thành **X**.
- $\rho_{s(X,C,D)}(r)$: đổi tên **r** thành **s**, cột **B** thành **X**.
- ví dụ (1): cho $\pi_{MAGV,HOTEN}(\sigma_{MABM='HTTT'}(GIAOVIEN))$.
    - $GV\_HTT \leftarrow \sigma_{MABM = 'HTTT'}(GIAOVIEN)$
    - $KQ \leftarrow \pi_{MAGV, HOTEN}(GV\_HTTT)$
- ví dụ (cách 2): 
    - $KQ(MA,TEN) \leftarrow \pi_{MAGV, HOTEN}(GV\_HTTT)$
    - $\rho_{KQ(MA,TEN)}(\pi_{MAGV, HOTEN}(GV\_HTTT))$
    
