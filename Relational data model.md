### Khái niệm Mô hình quan hệ

- **Quan hệ (Relation):** Một bảng giá trị, được coi như một tập hợp các hàng hoặc một tập hợp các cột. Mỗi hàng biểu diễn một thực thể hoặc mối quan hệ trong thế giới thực.
- **Lược đồ quan hệ (Schema of Relation):** Mô tả cấu trúc của quan hệ, bao gồm tên quan hệ và các thuộc tính. Degree of schema là số thuộc tính của lược đồ
- **Thuộc tính (Attribute):** Một cột trong bảng, nhận các giá trị thuộc miền giá trị của nó.
- **Bộ (Tuple):** Một hàng trong bảng, là một tập hợp các giá trị được sắp xếp theo thứ tự.
- **Miền giá trị (Domain):** Tập hợp các giá trị mà một thuộc tính có thể nhận. (viết tắt: dom)
- **Lược đồ cơ sở dữ liệu quan hệ (Relational Database Schema):** Bao gồm một tập hợp các lược đồ quan hệ. $S = \{R_1, R_2,..., R_n\}$
- Một quan hệ $r(R)$ của lược đồ $R(A_1, A_2,...,A_n)$ là: (A là attribute, t là tuple, dom là domain, v là value)
    - **Tập các bộ** - hay dòng(set of tuples) $r = \{t_1, t_2,...,t_m\}$, mỗi bộ t là một danh sách có thứ tự của n giá trị $t = \{v_1, v_2,...,v_n\}$
    - Mỗi $v_i, 1 \leq i \leq n, \in dom(A_i)$ or $null$. $Null$ chỉ sự không xác định(unknown) hoặc không tồn tại(not exist).
    - Dịch sang tiếng người: 
        - **Lược đồ** là dòng chứa tên các giá trị(vd: ***SV(MSSV, TEN, NGAYSINH, DIACHI)***, chỉ có tên các cột giá trị trừu tượng chứ không có giá trị cụ thể)
        - Mối quan hệ là bảng mà chứa giá trị vào các hàng. Một quan hệ có m dòng, mỗi dòng chứa n giá trị tương ứng với n cột. Mỗi giá trị trong dòng có thể thuộc miền giá trị của cột đó hoặc **null**
- Tóm lại: $r(R) \subseteq (dom(A_1) \times dom(A_2) \times ... \times dom(A_n))$
- Giá trị thứ ***i*** của dòng ***t*** được ký hiệu bởi $t.A_i$ hay $t[i]$
### Khóa (Keys)

- **Siêu khóa (Super key):** Một tập hợp các thuộc tính của quan hệ có thể nhận diện duy nhất mỗi bộ trong quan hệ đó.
    - Cho $SK$ là một tập con của các giá trị của $R$, $SK$ khác rỗng. $SK$ là siêu khóa khi $\forall r, \forall t_1, t_2 \in r, t_1 \neq t_2 \rightarrow t_1[SK] \neq t_2[SK]$ 
    - Ví dụ: ***KHOA(MÃKHOA, TÊNKHOA, NĂMTL, PHÒNG, ĐIỆNTHOẠI, NGÀYNHẬN CHỨC)***, **{MÃ KHOA, TÊN KHOA}** là siêu khóa. (tất cả các giá trị của quan hệ cũng làm siêu khóa được)
- **Khóa (Key):** Một siêu khóa tối thiểu, không chứa siêu khóa khác làm tập con.
    - Cho $K$ là một tập con của các giá trị của $R$, $K$ khác rỗng. $K$ là khóa khi $K$ là siêu khóa của $R$ và $\forall K' \subset K, K' \neq K, K'$ không la siêu khóa của $R$. 
- **Khóa chính (Primary Key):** Một khóa được chọn để nhận diện duy nhất các bộ trong quan hệ, thường được gạch chân trong lược đồ quan hệ.
    - ***KHOA(==MÃKHOA==, TÊNKHOA, NĂMTL, PHÒNG, ĐIỆNTHOẠI, NGÀYNHẬN CHỨC)***
- **Khóa ngoại (Foreign Key):** Một tập hợp các thuộc tính trong một quan hệ tham chiếu đến khóa chính của một quan hệ khác.
    - Cho 2 lược đồ quan hệ $R_1(A_1, A_2,...,A_n)$ và $R_2(B_1, B_2,..., B_m)$. $PK \subseteq \{A_1, ..., A_n\}$ là khóa chính của $R_1$, $FK \subseteq \{B_1,...,B_n\}$.
    - FK là khóa ngoại của $R_2$ nếu:
        - Thuộc tính của $FK$ cùng miền với thuộc tính khóa chính $PK$.
        - $\forall t_2 \in R_2, \exists t_1 \in R_1, t_2[FK] = t_1[PK]$
  ![[FK]]
  - Một thuộc tính có thể vừa là một phần khóa chính vừa là một phần khóa ngoại.
  - Một khóa ngoại có thể tham chiếu đến khóa chính của chính quan hệ đó. ![[self foreignkey]]
  - Nhiều khóa ngoại có thể tham chiếu đến cùng 1 khóa chính.
  - Ràng buộc toàn vẹn tham chiếu (Referential Integrity Constraint) = Ràng buộc khóa ngoại (Foreign Key Constraint): Ràng buộc toàn vẹn tham chiếu đảm bảo rằng các **giá trị của khóa ngoại** trong một bảng phải **tồn tại** trong **khóa chính** của bảng được tham chiếu.
### Đặc điểm của quan hệ

- Thứ tự các bộ trong quan hệ không quan trọng.
- Thứ tự các giá trị trong một bộ là quan trọng.
- Các giá trị trong một bộ phải là nguyên tử hoặc giá trị null.
- Không có bộ nào trùng nhau.

### Ánh xạ ER sang Quan hệ

- **Thực thể thông thường (Regular Entity Set):** Tạo một quan hệ tương ứng với cùng tên và cùng tập hợp các thuộc tính (trừ thuộc tính phức hợp và đa giá trị).
- **Thuộc tính phức hợp (Composite Attribute):** Chuyển đổi thành một thuộc tính đơn giá trị hoặc một tập hợp các thuộc tính đơn giá trị.
- **Thuộc tính đa giá trị (Multi-valued Attribute):** Tạo một quan hệ mới chứa khóa chính của quan hệ gốc và thuộc tính đa giá trị dưới dạng đơn giá trị.
- **Thực thể yếu (Weak Entity Set):** Tạo một quan hệ tương ứng, thêm các thuộc tính khóa của thực thể mà thực thể yếu phụ thuộc vào.
- **Tập quan hệ (Relationship Set):**
    - **1-n:** Thêm khóa của quan hệ "nhiều" vào quan hệ "một".
    - **1-1:** Thêm khóa của một quan hệ vào quan hệ kia, hoặc thêm khóa vào cả hai quan hệ, cùng với các thuộc tính trên mối quan hệ.
    - **n-n:** Tạo một quan hệ mới với tên là tên của mối quan hệ, chứa các thuộc tính khóa của các thực thể được kết nối và các thuộc tính trên mối quan hệ.
- **Các thao tác cập nhật trên quan hệ (Update Operations on Relations):** Chèn, xóa, sửa đổi bộ. Cần đảm bảo ràng buộc toàn vẹn không bị vi phạm.