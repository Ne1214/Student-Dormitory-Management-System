## Schema：房間資訊
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
| 欄位                  | 值              | 說明        |
| ------------------- | -------------- | --------- |
| room\_id            | 1              | 房間唯一 ID   |
| room\_number        | A101           | 房號        |
| building            | Maple Building | 所屬大樓      |
| capacity            | 4              | 最多可住 4 人  |
| current\_occupants  | 3              | 目前有 3 名學生 |
| electricity\_usage  | 120.5          | 電使用量（度）   |
| electricity\_fee    | 300.0          | 電費（元）     |
| water\_usage        | 25.0           | 水使用量（度）   |
| water\_fee          | 100            | 水費（元）     |
| is\_paid            | TRUE           | 是否已繳費     |
| last\_payment\_date | 2025-05-01     | 上次繳費日期    |
| note                | 無異常            | 備註        |
## Schema：住戶資訊
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
## 範例
```sql
INSERT INTO resident VALUES
(101, 'Alice Chen', 'female', '2003-03-15', '0912345678', 'alice@mail.com', 1);
```
## 說明
| 欄位           | 值                                       | 說明       |
| ------------ | --------------------------------------- | -------- |
| resident\_id | 101                                     | 學生唯一識別碼  |
| name         | Alice Chen                              | 學生姓名     |
| gender       | female                                  | 性別       |
| dob          | 2003-03-15                              | 出生日期     |
| phone        | 0912345678                              | 連絡電話     |
| email        | [alice@mail.com](xxx@mail.com) | Email 地址 |
| room\_id     | 1                                       | 入住房間 ID  |
| 元素                         | 說明                                                   |
| FOREIGN KEY              | 關鍵字，用來宣告這是一個外鍵欄位                                     |
| (room_id)                | 欲設定為外鍵的欄位名稱（在本表中）                                    |
| REFERENCES room(room_id) | 指定所參照的資料表及欄位，即 room 資料表中的 room_id 欄位             |
| 關聯邏輯                    | 代表本表的 room_id 必須對應 room 表中的某筆 room_id，否則無法插入資料 |

## Schema：維修紀錄
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
## 範例
```sql
INSERT INTO repair VALUES
(1, 101, 1, '冷氣漏水', 'in_progress', '2025-05-01', NULL);
```
## 說明
| 欄位               | 值            | 說明         |
| ---------------- | ------------ | ---------- |
| repair\_id       | 1            | 維修單編號      |
| resident\_id     | 101          | 提出者 ID     |
| room\_id         | 1            | 房間 ID      |
| issue            | 冷氣漏水         | 問題描述       |
| status           | in\_progress | 維修狀態（進行中）  |
| request\_date    | 2025-05-01   | 提出日期       |
| completion\_date | NULL         | 尚未完成（NULL） |
## Schema：公告
```sql
CREATE TABLE announcement (
    announcement_id INT PRIMARY KEY,
    title VARCHAR(200),
    content TEXT,
    created_at DATE,
    updated_at DATE
);
```
## 範例
```sql
INSERT INTO announcement VALUES
(1, '停電通知', '因電力維修，5月10日停電一天。', '2025-05-05', '2025-05-05');
```
## 說明
| 欄位               | 值                | 說明    |
| ---------------- | ---------------- | ----- |
| announcement\_id | 1                | 公告 ID |
| title            | 停電通知             | 公告標題  |
| content          | 因電力維修，5月10日停電一天。 | 公告內容  |
| created\_at      | 2025-05-05       | 建立日期  |
| updated\_at      | 2025-05-05       | 更新日期  |

## Schema：物品租借
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
## 範例
```sql
INSERT INTO rental VALUES
(1, 101, '吸塵器', 1, 'borrowed', '2025-05-03', NULL);
```
## 說明
| 欄位           | 值          | 說明      |
| ------------ | ---------- | ------- |
| rental\_id   | 1          | 借用紀錄 ID |
| resident\_id | 101        | 借用人     |
| item\_name   | 吸塵器        | 借用物品名稱  |
| quantity     | 1          | 借用數量    |
| status       | borrowed   | 借用中     |
| borrow\_date | 2025-05-03 | 借出日期    |
| return\_date | NULL       | 尚未歸還    |

## Schema： 訪客紀錄
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
## 範例
```sql
INSERT INTO visitor VALUES
(1, 'John Doe', '0911222333', '拜訪朋友', 101, '2025-05-05 15:00:00', '2025-05-05 17:30:00');
```
## 說明
| 欄位           | 值                   | 說明       |
| ------------ | ------------------- | -------- |
| visitor\_id  | 1                   | 訪客紀錄 ID  |
| name         | John Doe            | 訪客姓名     |
| phone        | 0911222333          | 訪客電話     |
| purpose      | 拜訪朋友                | 來訪目的     |
| resident\_id | 101                 | 拜訪的學生 ID |
| check\_in    | 2025-05-05 15:00:00 | 入宿時間     |
| check\_out   | 2025-05-05 17:30:00 | 離開時間     |

## Schema：抽籤紀錄
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
## 範例
```sql
INSERT INTO lottery VALUES
(1, 101, '2025-04-01', 'selected', TRUE, 1);
```
## 說明
| 欄位                | 值          | 說明        |
| ----------------- | ---------- | --------- |
| lottery\_id       | 1          | 抽籤 ID     |
| resident\_id      | 101        | 學生 ID     |
| application\_date | 2025-04-01 | 申請抽籤日期    |
| status            | selected   | 抽中狀態（已中籤） |
| acceptance        | TRUE       | 是否接受分發    |
| room\_id          | 1          | 分配房間 ID   |
