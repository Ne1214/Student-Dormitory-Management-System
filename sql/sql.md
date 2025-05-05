## Schema
```sql
CREATE TABLE room (
    room_id INT PRIMARY KEY,
    room_number VARCHAR(50),
    building VARCHAR(50),
    capacity INT,
    current_occupants INT,
    electricity_usage FLOAT,
    electricity_fee FLOAT,
    water_usage FLOAT,
    water_fee FLOAT,
    is_paid BOOLEAN,
    last_payment_date DATE,
    note TEXT
);
```
## 範例
```sql
INSERT INTO room VALUES
(1, 'A101', 'Maple Building', 4, 3, 120.5, 300, 25.0, 100, TRUE, '2025-05-01', '無異常');
```
## 說明
|欄位|說明|
|:--:|:--:|
|room_id = 1|房間主鍵|
### room_number = 'A101'：房號 A101。
### building = 'Maple Building'：位於 Maple 大樓。
### capacity = 4：最多可住 4 人。
### current_occupants = 3：目前有 3 名學生入住。
### electricity_usage = 120.5（度）、electricity_fee = 300（元）：本期電費資訊。
### water_usage = 25.0（度）、water_fee = 100（元）：本期水費資訊。
### is_paid = TRUE：表示這間房水電費已繳。
### last_payment_date = '2025-05-01'：最後繳費日期。
### note = '無異常'：房況備註。
### 用途： 此表整合了房間狀況、容量與帳務資訊，是水電費結算與超額人數查核的核心依據。
```sql
CREATE TABLE resident (
    resident_id INT PRIMARY KEY,
    name VARCHAR(100),
    gender ENUM('male', 'female', 'other'),
    dob DATE,
    phone VARCHAR(20),
    email VARCHAR(100),
    room_id INT,
    FOREIGN KEY (room_id) REFERENCES room(room_id)
);
```
```sql
INSERT INTO resident VALUES
(101, 'Alice Chen', 'female', '2003-03-15', '0912345678', 'alice@mail.com', 1);
```
```sql
CREATE TABLE repair (
    repair_id INT PRIMARY KEY,
    resident_id INT,
    room_id INT,
    issue TEXT,
    status ENUM('pending', 'in_progress', 'completed', 'cancelled'),
    request_date DATE,
    completion_date DATE,
    FOREIGN KEY (resident_id) REFERENCES resident(resident_id),
    FOREIGN KEY (room_id) REFERENCES room(room_id)
);
```
```sql
INSERT INTO repair VALUES
(1, 101, 1, '冷氣漏水', 'in_progress', '2025-05-01', NULL);
```
```sql
CREATE TABLE announcement (
    announcement_id INT PRIMARY KEY,
    title VARCHAR(200),
    content TEXT,
    created_at DATE,
    updated_at DATE
);
```
```sql
INSERT INTO announcement VALUES
(1, '停電通知', '因電力維修，5月10日停電一天。', '2025-05-05', '2025-05-05');
```
```sql
CREATE TABLE rental (
    rental_id INT PRIMARY KEY,
    resident_id INT,
    item_name VARCHAR(100),
    quantity INT,
    status ENUM('borrowed', 'returned', 'overdue'),
    borrow_date DATE,
    return_date DATE,
    FOREIGN KEY (resident_id) REFERENCES resident(resident_id)
);
```
```sql
INSERT INTO rental VALUES
(1, 101, '吸塵器', 1, 'borrowed', '2025-05-03', NULL);
```
```sql
CREATE TABLE visitor (
    visitor_id INT PRIMARY KEY,
    name VARCHAR(100),
    phone VARCHAR(20),
    purpose TEXT,
    resident_id INT,
    check_in DATETIME,
    check_out DATETIME,
    FOREIGN KEY (resident_id) REFERENCES resident(resident_id)
);
```
```sql
INSERT INTO visitor VALUES
(1, 'John Doe', '0911222333', '拜訪朋友', 101, '2025-05-05 15:00:00', '2025-05-05 17:30:00');
```
```sql
CREATE TABLE lottery (
    lottery_id INT PRIMARY KEY,
    resident_id INT,
    application_date DATE,
    status ENUM('pending', 'selected', 'not_selected'),
    acceptance BOOLEAN,
    room_id INT,
    FOREIGN KEY (resident_id) REFERENCES resident(resident_id),
    FOREIGN KEY (room_id) REFERENCES room(room_id)
);
```
```sql
INSERT INTO lottery VALUES
(1, 101, '2025-04-01', 'selected', TRUE, 1);
```
