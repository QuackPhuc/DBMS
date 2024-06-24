
- Ràng buộc toàn vẹn (IC) đảm bảo tính đúng đắn và ý nghĩa của dữ liệu trong cơ sở dữ liệu. 
- Chúng được khám phá từ ngữ nghĩa của dữ liệu hoặc cách biểu diễn dữ liệu trong doanh nghiệp.
- IC đảm bảo tính đúng đắn của dữ liệu và lược đồ dữ liệu, cũng như ngữ nghĩa của lược đồ cơ sở dữ liệu. 
- Khi một IC được khai báo, tất cả các trường hợp của cơ sở dữ liệu phải thỏa mãn IC đó tại mọi thời điểm. 
- IC được khám phá và khai báo bởi người thiết kế cơ sở dữ liệu trong giai đoạn thiết kế và được định nghĩa trên một lược đồ quan hệ hoặc liên quan đến nhiều lược đồ quan hệ.

# Đặc điểm của ràng buộc toàn vẹn:

- **Bối cảnh (Context):** Xác định các lược đồ quan hệ có thể bị vi phạm bởi IC khi thực hiện cập nhật dữ liệu (thêm, xóa, sửa đổi dữ liệu).
    - Ví dụ: $IC_1$: "Lương của một giảng viên không thể vượt quá lương của trưởng bộ môn".
    - Các thao tác cập nhật ảnh hưởng đến $IC_1$: Cập nhật lương của giảng viên, thêm giảng viên mới vào bộ môn. Bổ nhiệm trưởng bộ môn mới.
    - Bối cảnh: GIAOVIEN, BOMON
- **Nội dung (Content):** Được trình bày bằng ngôn ngữ tự nhiên (dễ hiểu nhưng thiếu tính chính thức, nhất quán) hoặc ngôn ngữ hình thức (ngắn gọn, nhất quán nhưng đôi khi khó hiểu). Các dạng ngôn ngữ hình thức bao gồm Đại số quan hệ, Tuples Relational Calculus và mã giả.
    - Ví dụ: $IC_2$ natural language form: "Người quản lý trực tiếp của một giáo viên phải là một giáo viên trong cùng bộ môn".
    - Formal language: 
```SQL
     (∀t)(GIAOVIEN(t) ∧ (t.GVQLCM != null => (∃s)(GIAOVIEN(s) ∧ s.MABM = t.MABM ∧ s.MAGV = t.GVQLCM )))
```
- **Bảng ảnh hưởng (Influence table):** Xác định các hoạt động cập nhật cần kiểm tra IC khi được thực hiện trên các quan hệ. Có hai loại bảng ảnh hưởng: bảng ảnh hưởng cho 1 IC và bảng ảnh hưởng tổng hợp.
![[EX_IC1]]
# Phân loại:

- **Ràng buộc toàn vẹn bắt buộc liên quan đến mô hình dữ liệu (Inherent model based constraints):** Ví dụ: Một quan hệ không thể chứa các bộ giá trị trùng lặp.
- **Ràng buộc toàn vẹn liên quan đến lược đồ của mô hình dữ liệu (Schema based constraints):** Ví dụ: Ràng buộc miền giá trị, ràng buộc khóa, ràng buộc null, ràng buộc tham chiếu.
- **Ràng buộc toàn vẹn dựa trên ứng dụng (Application based constraints):** Ví dụ: Lương của một giảng viên không được vượt quá trưởng bộ môn.

# Triển khai:

- **Khóa chính (Primary key):** Đảm bảo tính duy nhất của một thuộc tính hoặc một tập hợp các thuộc tính trong một quan hệ.
- **Khóa ngoại (Foreign key):** Đảm bảo rằng các giá trị của thuộc tính trong một quan hệ phải tham chiếu đến các giá trị của thuộc tính trong quan hệ khác.
- **Ràng buộc kiểm tra (Check constraint):** Giới hạn các giá trị có thể được gán cho một thuộc tính.
- **Khẳng định (Assertion):** Là một biểu thức SQL luôn trả về giá trị TRUE. Người dùng cần xác định điều gì phải đúng.
- **Trigger:** Một tập hợp các lệnh được tự động thực thi khi một sự kiện xảy ra trong cơ sở dữ liệu.
- **Giao dịch (Transaction):** Một tập hợp các lệnh thực hiện một tác vụ/giao dịch trong cơ sở dữ liệu, sao cho hoặc tất cả các lệnh được thực thi thành công hoặc không có lệnh nào được thực thi.
- **Thủ tục lưu trữ (Stored Procedure):** Một cách để lưu trữ các hàm hoặc thủ tục trong cơ sở dữ liệu để sử dụng trong các câu lệnh SQL.