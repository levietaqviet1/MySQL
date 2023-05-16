# MySQL

# :notebook_with_decorative_cover: Table of Contents
- [Cách mở ER Diagram](#cách-mở-er-diagram)
- [Code cơ bản để nhớ lại lệch trong SQL](#Code-cơ-bản-để-nhớ-lại-lệch-trong-SQL) 
- [Trigger](#Trigger)
- [STORED PROCEDURE](#STORED-PROCEDURE)

## Cách mở ER Diagram

- Bước 1: Mở MySQL 

- Bước 2: Chọn Database ở thanh công cụ

![image](https://github.com/levietaqviet1/MySQL/assets/85175337/4aeee6d0-8d6a-4c0b-a416-50d9d063a62d)

- Bước 3: Chọn Reverse ...

![image](https://github.com/levietaqviet1/MySQL/assets/85175337/310b9e42-54bc-49e2-851e-522dda77674c)

- Bước 4: Hiện ra cửa sổ Reverse thì bấm next nếu như mọi thông tin là chuẩn rồi

- Bước 5: Cứ next đến ô chọn database muốn hiện Diagram

![image](https://github.com/levietaqviet1/MySQL/assets/85175337/da1b6c78-45d3-4848-9046-9af02547ce32)

- Bước 6: Next đến khi nào ra thì thôi

![image](https://github.com/levietaqviet1/MySQL/assets/85175337/694f3977-0664-4594-a297-1d9c496d963a)

## Code cơ bản để nhớ lại lệch trong SQL

- Tạo một database mới:

```sql
CREATE DATABASE ten_database;
```

- Chọn một database để sử dụng:

```sql
USE ten_database;
```

- Tạo một bảng mới:

```sql
CREATE TABLE ten_bang (
    cot_1_kieu_du_lieu,
    cot_2_kieu_du_lieu,
    ...
);
```

- Thêm một dòng mới vào bảng:

```sql
INSERT INTO ten_bang (cot_1, cot_2, ...) VALUES (gia_tri_cot_1, gia_tri_cot_2, ...);
```

- Lấy tất cả các dòng từ bảng:

```sql
SELECT * FROM ten_bang;
```
- Lấy các dòng cụ thể từ bảng:

```sql
SELECT cot_1, cot_2, ... FROM ten_bang WHERE dieu_kien;
```

- Cập nhật dữ liệu trong bảng:

```sql
UPDATE ten_bang SET cot_1 = gia_tri_moi_cho_cot_1 WHERE dieu_kien;
```

- Xóa dữ liệu từ bảng:

```sql
DELETE FROM ten_bang WHERE dieu_kien;
```

- Xóa bảng:

```sql
DROP TABLE ten_bang;
```

## Trigger 

- `Trigger` là một đối tượng được định danh trong `CSDL` và được gắn chặt với một sự kiện xảy ra trên một bảng nào đó 
(điều này có nghĩa là nó sẽ được tự động thực thi khi xảy ra một sự kiện trên một bảng). Các sự kiện này bao gồm: `INSERT`, `UPDATE` hay `DELETE` một bảng.

- Lý do: Trigger được `thực thi tự động khi xuất hiện một hành động thay đổi trong bảng, nên người ta có thể ứng dụng trigger` để tạo ra các công việc tự động thay cho việc phải làm thủ công bằng tay như: kiểm tra dữ liệu, đồng bộ hóa dữ liệu, đảm bảo các mối quan hệ giữa các bảng...

- Cú pháp

```sql
CREATE TRIGGER trigger_name trigger_time trigger_event

 ON table_name

 FOR EACH ROW

 BEGIN

 ...

 END
```

Giải thích:

`CREATE TRIGGER` dùng để tạo các `trigger`

`Trigger_name` là tên của trigger được đặt theo trình tự `[trigger time][table name][trigger event]`

ví dụ: `before_products_update`

`Trigger_time` là thời điểm kích hoạt trigger.

`BEFORE`: khi bạn muốn xử lý trước khi thực hiện thay đổi trên bảng dữ liệu.

`AFTER`: thay đổi trên bảng dữ liệu trước rồi mới xử lý sau.

`Trigger_event` là theo dõi các sự kiện nào. Nó có 3 lựa chọn `INSERT, UPDATE hoặc DELETE..` Mỗi trigger chỉ theo dõi được một sự kiện duy nhất.

Mỗi trigger phải gắn liền với một bảng dữ liệu được chỉ ra sau từ khóa ON.

Phần thân của trigger nằm trong khối lệnh BEGIN…END

Từ khóa OLD chỉ đến dòng dữ liệu đang tồn tại trước khi thực hiện thao tác chỉnh sửa. Từ khóa NEW chỉ đến dòng dữ liệu mới xuất hiện sau khi thực hiện thao tác chỉnh sửa.


Ví dụ:

```sql
DELIMITER $$
CREATE TRIGGER add_order_history AFTER INSERT ON orders
FOR EACH ROW
BEGIN
    INSERT INTO order_history (order_id, order_status, notes)
    VALUES (NEW.id, 'New Order', 'A new order has been placed.');
END$$
DELIMITER ;
```

Khi một đơn hàng mới được thêm vào bảng "orders", trigger sẽ tự động thêm một bản ghi tương ứng vào bảng "order_history", giúp chúng ta theo dõi lịch sử của các đơn hàng.

```sql
DELIMITER $$
CREATE TRIGGER delete_order_items BEFORE DELETE ON orders
FOR EACH ROW
BEGIN
    DELETE FROM order_items WHERE order_id = OLD.id;
END$$
DELIMITER ;
```

Khi một đơn hàng bị xóa khỏi bảng "orders", trigger sẽ tự động xóa tất cả các mặt hàng liên quan từ bảng "order_items", giúp chúng ta duy trì tính toàn vẹn của database.

## STORED PROCEDURE

- Stored Procedure được tạo ra nhằm thực hiện các lệnh của mysql theo một nhóm việc cụ thể thay vì thực hiện từng thao tác (insert,update,delete).

- Lý do:  Làm tăng hiệu xuất sử lý giữ liệu, giảm thời gian giao tiếp giữa ứng dụng với hệ quản trị cơ sở giữ liệu. thay vì gửi từng câu truy vấn thì nay sẽ chỉ gửi một Stored Procedure. ...

- Cú pháp

```sql
CREATE PROCEDURE <owner>.<procedure name>
     <Param> <datatype>
AS
     <Body>
Ví dụ:
CREATE PROCEDURE Users_GetUserInfo
    @login nvarchar(30)=null
AS
    SELECT * from users
    WHERE ISNULL(@login,login)=login
```

Để thực thi precedure bằng script sử dụng lệnh EXECUTE:

```sql
EXECUTE procedure_name
```

- ví dụ: 

```sql
DELIMITER $$
CREATE PROCEDURE calculate_total_salary()
BEGIN
    SELECT SUM(salary) FROM salaries;
END$$
DELIMITER ;

CALL calculate_total_salary();

```

Stored Procedure được đặt tên là "calculate_total_salary". Khi Stored Procedure được gọi, nó sẽ thực hiện câu lệnh "SELECT SUM(salary) FROM salaries;", và trả về tổng lương của tất cả các nhân viên.



