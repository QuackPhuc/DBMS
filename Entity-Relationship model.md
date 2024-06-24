## **Các khái niệm cơ bản**

- **Thực thể (Entity):** Đối tượng trừu tượng trong thế giới thực (ví dụ: xe hơi, nhân viên, hóa đơn).
- **Tập thực thể (Entity set/Entity type):** Tập hợp các thực thể có cấu trúc tương tự (ví dụ: tập nhân viên, tập xe hơi).
- **Thuộc tính (Attribute):** Đặc điểm của thực thể (ví dụ: tên nhân viên, mã xe hơi).
    - **Thuộc tính đơn trị (Single-valued attribute):** Chỉ có một giá trị duy nhất. (Vd: ngày sinh)
    - **Thuộc tính đa trị (Multi-valued attribute):** Có thể có nhiều giá trị. (vd: một người có thể có nhiều số điện thoại) ![[multi-valued attribute]]
    - **Thuộc tính kết hợp (Composite attribute):** Được chia thành nhiều thuộc tính nhỏ hơn. (vd: thuộc tính địa chỉ có thể chia thành đường, quận, thành phố, tỉnh,...)
    - **Thuộc tính dẫn xuất (Derived attribute):** Giá trị được tính toán từ các thuộc tính khác. (vd: tuổi có thể được tính bởi ngày sinh) ![[derived attribute]]
- **Thuộc tính khóa (Key attribute):** Một hoặc nhiều thuộc tính xác định duy nhất một thực thể.
- **Mối kết hợp (Relationship):** Sự liên kết giữa hai hoặc nhiều thực thể.
- **Tập mối kết hợp (Relationship set):** Tập hợp các mối kết hợp tương tự.
- **Bản số (Cardinality):** Chỉ định số lượng thực thể liên quan đến một thực thể khác trong một mối kết hợp.
    - **Một-một (1:1):** Mỗi thực thể ở một bên chỉ liên kết với tối đa một thực thể ở bên kia.
    - **Một-nhiều (1:N):** Một thực thể ở một bên có thể liên kết với nhiều thực thể ở bên kia.
    - **Nhiều-nhiều (N:N):** Nhiều thực thể ở một bên có thể liên kết với nhiều thực thể ở bên kia.
    - Dùng bằng một cặp số (m, M) với m là min của số lần tham gia vào tập mối kết hợp, M là max số lần tham gia vào tập mối kết hợp.![[cardinary]]Một **người lao động** - **có** - 0 hoặc 1 CV. (employe tham gia vào quan hệ has với CV với min = 0, max = 1). Một CV(khi được tạo ra) thì nó chỉ thuộc về một người duy nhất(CV tham gia vào quan hệ has với employee với min = 1, max = 1)
- **Thực thể yếu (Weak entity set):** Thực thể không có khóa riêng và phụ thuộc vào một thực thể khác thông qua mối kết hợp. ![[weak entity]]