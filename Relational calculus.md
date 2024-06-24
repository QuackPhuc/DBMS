Relational Calculus là một ngôn ngữ truy vấn phi thủ tục, khai báo. Nó cho phép người dùng chỉ định những gì cần truy xuất từ cơ sở dữ liệu mà không cần chỉ định cách thức thực hiện. Có hai loại chính của Relational Calculus:
# Tuple Relational Calculus (TRC):
Mỗi truy vấn trong TRC có dạng $\{t | P(t)\}$ (hay $\{t.A | P(t)\}$, trong đó `t` là một biến tuple và `P(t)` là một điều kiện. Kết quả của truy vấn là tập hợp tất cả các tuple `t` thỏa mãn điều kiện `P(t)`.
- Ví dụ: Tìm nhân viên có lương lớn hơn 30000.
```SQL
    {t | EMPLOYEE(t) ∧ t.SALARY > 30000}
```
- Tìm tên và họ của nhân viên có lương lớn lơn 30k
```SQL
    {t.SSN, t.FNAME | EMPLOYEE(t) ∧ t.SALARY > 30000}
```
- Tìm nhân viên mà làm cho phòng 'Nghiên cứu'
```SQL
    {t.SSN | EMPLOYEE(t) ∧ (∃s)DEPARTMENT(s) ∧ s.DNAME='Nghiên cứu' ∧ s.DNUMBER = t.DNO}
```
- Tìm nhân viên làm trong project hoặc có người phụ thuộc
```SQL
    {t.NAME | EMPLOYEE(t) ∧ ((∃s)(WORK(s) ^ (t.SSN = s.ESSN)) ∨ (∃u)(DEPENDENT(u) ∧ (t.SSN = u.ESSN))}
```
- Tìm nhân viên làm trong project và không có người phụ thuộc
```SQL
    {t.NAME | EMPLOYEE(t) ∧ ((∃s)(WORK(s) ^ (t.SSN = s.ESSN)) ∨ ¬(∃u)(DEPENDENT(u) ∧ (t.SSN = u.ESSN))}
```
- Nhân viên có lương cao nhất
```SQL
    {t.SSN | EMPLOYEE(t) ∧ (∀s)(EMPLOYEE(e)(t.SALARY >= e.SALARY))}
```
- Nhân viên làm mọi project
```SQL
    {t.SSN | EMPLOYEE(t) ∧ (∀s)(PROJECT(s) ∧ (∃u)(WORK(u) ∧ (s.ID = u.PRJID) ∧ (t.SSN = u.ESSN)))}
```
# Domain Relational Calculus (DRC):
Trong DRC, các truy vấn được xây dựng bằng cách sử dụng các biến miền, mỗi biến đại diện cho một giá trị đơn lẻ từ miền của một thuộc tính.
- Ví dụ: Tìm SSN và tên của nhân viên có lương lớn hơn 30000.
```SQL
    {r, s | (∃x)(EMPLOYEE(p, q, r, s, t, u, v, x, y, z) ∧ x > 30000)}
```
- Tìm nhân viên mà làm ở phòng nghiên cứu:
```SQL
    {s | (∃z) (EMPLOYEE(p, q, r, s, t, u, v, x, y, z) ∧ (∃a, b)(DEPARTMENT(a, b, c, d) ∧ a = ‘Nghien cuu’ ∧ b = z ))}
```
**Các thành phần quan trọng:**

- **Biến tự do và biến ràng buộc:** Biến tự do có thể nhận bất kỳ giá trị nào từ miền của nó, trong khi biến ràng buộc bị giới hạn trong một phạm vi cụ thể.
- **Nguyên tử:** Là các thành phần cơ bản của công thức, đánh giá là TRUE hoặc FALSE.
- **Các quy tắc:** Xác định cách các công thức có thể được kết hợp để tạo thành các công thức phức tạp hơn.
- **Biểu thức an toàn:** Đảm bảo tạo ra một số lượng hữu hạn các tuple.
- **Các toán tử:** Bao gồm các toán tử so sánh (`<`, `>`, `<=`, `>=`, `!=`, `=`), toán tử logic (`∧`, `∨`, `¬`, `=>`), và các toán tử định lượng (`∃`, `∀`).
