- **Phụ thuộc hàm** là một ràng buộc giữa các thuộc tính, trong đó giá trị của một tập hợp các thuộc tính xác định duy nhất giá trị của một tập hợp các thuộc tính khác. Ví dụ, trong một bảng chứa thông tin về sinh viên, mã số sinh viên có thể xác định duy nhất tên sinh viên, ngày sinh và địa chỉ.
- **Chuẩn hóa** là quá trình phân tách các quan hệ "xấu" bằng cách chia các thuộc tính của chúng thành các quan hệ nhỏ hơn để cải thiện thiết kế cơ sở dữ liệu. Mục tiêu của chuẩn hóa là giảm thiểu dư thừa dữ liệu, tránh các bất thường khi cập nhật và đảm bảo tính toàn vẹn dữ liệu.
# Phụ thuộc hàm
- Gọi $R(A_1, A_2,...A_n), r(R)$, ký hiệu $R^{+} = \{A_1, A_2,...,A_n\}$
- Phụ thuộc hàm(FD) giữa 2 tập thuộc tính $X, Y \subseteq R^+$:
    - Ký hiệu: $X \rightarrow Y$ (X xác định Y), $\forall r \in R, t_1, t_2 \in r$. Nếu $t_1[X] = t_2[X]$ thì $t_1[Y] = t_2[Y]$.
    - Là một ràng buộc trong tất cả các quan hệ liên quan đến $r(R)$
- Ví dụ: $FD_1: MAGV \rightarrow \{TENGV,NGSINH,DCHI,MABM\}$
- Ví dụ: $FD_2: MABM \rightarrow \{TENBM, TRGBM\}$
- FD là một thuộc tính của lược đồ quan hệ R.
- FD đúng với mọi $r(R)$
- K là khóa chính của R thì K xác định đầy đủ hàm tất cả thuộc tính, ký hiệu $K \rightarrow R^+$
- FD được sử dụng để đánh giá thiết kế cơ sở dữ liệu quan hệ
- FD được suy luận từ các ràng buộc trong thế giới thực.
# Luật suy luận Armstrong:
- Cho một tập hợp các FD - F, ta có thể suy luận thêm các FD khác mà vẫn bảo đảm tính đúng đắn khi các FD trong F được thỏa mãn.
- **IR1 - Phản xạ:** Nếu Y là tập con của X, thì $X \rightarrow Y$.
- **IR2 - Tăng cường:** Nếu $X \rightarrow Y$, thì $XZ \rightarrow YZ$(XZ là hợp của X và Z).
    - Ví dụ: $MSSV \rightarrow TUOI$ thì $MSSV, TEN \rightarrow TUOI, TEN$
- **IR3 - Bắc cầu:** Nếu $X \rightarrow Y$ và $Y \rightarrow Z$, thì $X \rightarrow Z$.
    - Ví dụ: $MSSV \rightarrow NS, NS \rightarrow TUOI$ thì $MSSV \rightarrow TUOI$
- **IR4 - Phân tách:** Nếu $X \rightarrow YZ$ thì $X \rightarrow Y, X \rightarrow Z$
- **IR5 - Kết hợp:** Nếu $X \rightarrow Y, X \rightarrow Z$ thì $X \rightarrow YZ$
- **IR6 - Giả bắc cầu:** Nếu $X \rightarrow Y, WY \rightarrow Z$ thì $WX \rightarrow Z$
- Một phụ thuộc hàm $X \rightarrow Y$ được gọi là **phụ thuộc hàm đầy đủ** nếu việc loại bỏ bất kỳ thuộc tính nào khỏi X sẽ làm mất đi sự phụ thuộc hàm. Nói cách khác, Y phụ thuộc hoàn toàn vào tất cả các thuộc tính trong X, không phải chỉ một phần của X.
![[Armstrong's inference rules]]
# Chuẩn hóa
- **Chuẩn hóa** là quá trình ***cải thiện cấu trúc*** của một cơ sở dữ liệu quan hệ bằng cách ***phân tách*** các quan hệ ***lớn, "xấu"*** thành các quan hệ ***nhỏ hơn***, ***tốt*** hơn.
- Mục tiêu của chuẩn hóa là loại bỏ các vấn đề như dư thừa dữ liệu, bất thường cập nhật (update anomalies), và bất thường xóa (deletion anomalies).
- Quá trình chuẩn hóa giúp đảm bảo tính toàn vẹn dữ liệu, giảm thiểu không gian lưu trữ và tăng hiệu suất truy vấn

- **Dạng chuẩn** là một tập hợp các điều kiện hoặc tiêu chí mà một lược đồ quan hệ (relation schema) phải đáp ứng để được coi là ở một dạng chuẩn cụ thể. Các điều kiện này thường liên quan đến việc sử dụng khóa (keys) và phụ thuộc hàm (functional dependencies - FDs) để xác định cấu trúc của quan hệ.
- Mỗi dạng chuẩn cao hơn (ví dụ: 3NF cao hơn 2NF) thường khắc phục các vấn đề của dạng chuẩn thấp hơn và mang lại một thiết kế cơ sở dữ liệu tốt hơn.
- **Prime attribute** - thuộc tính chính: Là thuộc tính nằm trong khóa chính
- **Non-prime attribute**: Là thuộc tính không nằm trong khóa chính
## 1NF (Dạng chuẩn thứ nhất):
Không cho phép: 
- Các thuộc tính phức hợp: Địa chỉ(số nhà, đường, quận,...)
- Đa giá trị 
- Các quan hệ lồng nhau. 
Giá trị của bộ riêng lẻ phải là nguyên tử (không chia nhỏ được).
Các bước thực hiện:
- Loại bỏ các nhóm lặp: Mỗi dòng phải xác định một thực thể duy nhất. Một dòng chứa nhiều số điện thoại thì tách ra thành nhiều dòng.
- Xác định khóa chính
- *(Xác định các phụ thuộc: các phụ thuộc 1 phần và phụ thuộc bắc cầu)*
![[1NF]]
## 2NF (Dạng chuẩn thứ hai): 
Là dạng chuẩn 1NF mà mọi thuộc tính không chính(non-prime) phải phụ thuộc vào toàn bộ khóa chính, không phụ thuộc vào một phần khóa chính.
Các bước thực hiện: *(Đã có bước 3 ở 1NF)* 
![[Dependencies indent]]
- Loại bỏ các phụ thuộc 1 phần bằng cách tạo bảng mới: Tạo bảng mới với các khóa chính là các yếu tố quyết định(determinant, hay X nếu $X \rightarrow Y$). ![[elimination partial dependencies]]
- Gán lại các thuộc tính phụ thuộc: Di chuyển các thuộc tính phụ thuộc vào các bảng tương ứng. ![[Reassignment]]
- Các thuộc tính khác(phụ thuộc hoàn toàn) thì tạo bảng mới gồm các thuộc tính của khóa chính cũ, và các thuộc tính còn lại. Tạo FK cho các thuộc tính tham chiếu đến khóa chính của các bảng vừa tạo. ![[reass 2]] 
## 3NF (Dạng chuẩn thứ ba): 
Là dạng 2NF mà mọi thuộc tính không chính không được phụ thuộc bắc cầu vào khóa chính. Nghĩa là một thuộc tính không chính không được phụ thuộc vào một thuộc tính không chính khác.![[NF3]]
Các bước thực hiện:
- Loại bỏ phụ thuộc bắc cầu bằng cách tạo bảng mới: Tách các phụ thuộc bắc cầu ra bảng mới với khóa chính là các yếu tố quyết định.
- Gán lại các thuộc tính phụ thuộc: Di chuyển các thuộc tính và gán FK
![[NF3_final]]
## BCNF (Dạng chuẩn Boyce-Codd): 
Mạnh hơn 3NF, yêu cầu rằng bất cứ khi nào một $X \rightarrow A$ được thỏa mãn, thì X phải là một siêu khóa.
Các bước:(ở dạng 3NF)
- Xác định các phụ thuộc hàm $X \rightarrow Y$, mà $X \neq Y$ và $X$ không phải siêu khóa hay khóa.
- Phân tách quan hệ ban đầu thành 2 quan hệ mới. 
    - $Q_1 = \{X, Y\}$
    - $Q_2 = \{Các \ \ thuộc \ \  tính \ \ còn \ \ lại\} - \{Y\}$  (loại Y)
- Lặp bước trên(b2) cho $Q_2$ cho đến khi không còn phân tách được nữa
- Kết quả: quan hệ $Q_1$ và các $\{Q_i\}$ được tách từ $Q_2$ ![[BCNF]]