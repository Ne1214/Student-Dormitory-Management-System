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
    room_id INT,
    FOREIGN KEY (room_id) REFERENCES room(room_id)
);
```
## 範例
```sql
INSERT INTO resident VALUES
(101, 'Alice Chen', 'female', '2003-03-15', 1);
```
## 說明
| 欄位           | 值                                       | 說明       |
| ------------ | --------------------------------------- | -------- |
| resident\_id | 101                                     | 學生唯一識別碼  |
| name         | Alice Chen                              | 學生姓名     |
| gender       | female                                  | 性別       |
| dob          | 2003-03-15                              | 出生日期     |
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
## Schema：設備資產管理表
```sql
CREATE TABLE asset (
    asset_id INT PRIMARY KEY,
    asset_name VARCHAR(100) NOT NULL,
    asset_type VARCHAR(50),
    room_id INT,
    purchase_date DATE,
    status ENUM('available', 'in_repair', 'damaged', 'disposed') DEFAULT 'available',
    value DECIMAL(10,2),
    note TEXT,
    FOREIGN KEY (room_id) REFERENCES room(room_id)
);
```
## 範例
```sql
INSERT INTO asset VALUES
(1, '冷氣機', '電器', 1, '2023-09-01', 'available', 15000.00, '每年定期清洗'),
(2, '書桌', '家具', 1, '2022-08-15', 'damaged', 2000.00, '抽屜損壞待修'),
(3, '公共飲水機', '電器', NULL, '2021-06-01', 'in_repair', 8000.00, '設於交誼廳');
```
## 說明
| 欄位名稱            | 資料型別          | 說明                    |
| --------------- | ------------- | --------------------- |
| `asset_id`      | INT（主鍵）       | 設備唯一識別碼               |
| `asset_name`    | VARCHAR(100)  | 設備名稱（如：冷氣、書桌）         |
| `asset_type`    | VARCHAR(50)   | 設備類型（如：電器、家具）         |
| `room_id`       | INT           | 所屬房間 ID（可為 NULL 表示共用） |
| `purchase_date` | DATE          | 購買日期                  |
| `status`        | ENUM          | 設備狀態（使用中、維修中等）        |
| `value`         | DECIMAL(10,2) | 設備價值（元）               |
| `note`          | TEXT          | 備註說明                  |
## Schema：帳密 
```sql
CREATE TABLE IF NOT EXISTS user_account (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    resident_id INT,
    phone VARCHAR(20),
    role ENUM('admin', 'student') NOT NULL DEFAULT 'student',
    email VARCHAR(100) UNIQUE,
    last_login DATETIME,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (resident_id) REFERENCES resident(resident_id)
);
```
## 範例
```sql
INSERT INTO user_account (
    username,
    password,
    resident_id,
    phone,
    role,
    email,
    last_login,
    created_at,
    updated_at
) VALUES
-- 管理員帳號
('admin01', '$2b$12$abcdEfghIjKlMnOpQrSTUvWxYz12345678abcdefghi', NULL,0951321654, 'admin', 'admin01@dorm.com', '2025-05-06 09:15:00', NOW(), NOW()),

-- 學生帳號（與 resident_id 連結）
('student101', '$2b$12$1234567890abcdefABCDEFghijklmnopQRSTuv', 101,0951321654, 'student', 'student101@dorm.com', NULL,  NOW(), NOW());
```
## 說明
| 欄位名稱          | 資料型別                    | 說明                       |
| ------------- | ----------------------- | ------------------------ |
| `user_id`     | INT AUTO\_INCREMENT     | 使用者 ID，自動遞增主鍵            |
| `username`    | VARCHAR(50)             | 學生統一帳號是學號，需唯一                |
| `password`    | VARCHAR(255)            | 密碼欄位，建議儲存加密後內容           |
| `resident_id` | INT                     | 對應 `resident` 表的 ID |
| `phone`       | VARCHAR(20)             | 聯絡電話                     |
| `role`        | ENUM('admin','student') | 使用者角色（admin / student）   |
| `email`       | VARCHAR(100)            | 電子信箱，唯一                  |
| `last_login`  | DATETIME                | 最後登入時間                   |
| `created_at`  | DATETIME                | 帳號建立時間                   |
| `updated_at`  | DATETIME                | 資料更新時間                   |
## 住宿生
| 欄位 / 表格  | 權限    | 說明                          |
| -------- | ----- | --------------------------- |
| `住宿生資料表` | R     | 查詢自己的住宿資訊                   |
| `報修資料表`  | C / R | 提出報修（Create），查詢自己報修紀錄（Read） |
| `租借資料表`  | C / R | 借用物品與查詢借用紀錄                 |
| `訪客資料表`  | C / R | 登記訪客、查詢來訪紀錄                 |
| `帳密資料表`  | R / U | 查詢與更新自己帳號密碼資料（可能僅限密碼與聯絡資料）  |


## 訪客
| 欄位 / 表格    | 權限 | 說明                        |
| ---------- | -- | ------------------------- |
| `無資料表直接操作權限` | -  | 僅由住宿生登記，不需要系統帳號，也不直接操作資料表 |
## 宿舍管理員
| 欄位 / 表格    | 權限        | 說明              |
| ---------- | --------- | --------------- |
| `住宿生資料表`   | C / R / U | 新增、查詢與調整住宿生資料   |
| `房間資料表`    | R / U     | 查詢與更新房間分配（不可刪除） |
| `報修資料表`    | R / U     | 查閱與更新報修處理狀況     |
| `租借資料表`    | R / U     | 查詢借用紀錄，並處理歸還與狀態 |
| `訪客資料表`    | R         | 查詢來訪紀錄          |
| `住宿生抽選資料表` | C / R / U | 審核抽籤與錄取資料       |
| `帳密資料表`    | R         | 查詢住宿生帳號（限特定欄位）  |

## 系統管理員
| 欄位 / 表格 | 權限            | 說明                               |
| ------- | ------------- | -------------------------------- |
| `所有資料表`   | C / R / U / D | 完整存取與管理整個系統的所有資料，包括新增/刪除帳號、權限控管等 |
| `公告資料表` | C / R / U / D | 管理公告訊息發佈與修改                      |
| `帳密資料表` | C / R / U / D | 建立與移除用戶帳號，修改角色與密碼等               |
|`權限管理`    | ✔             | 管理不同角色的權限設定與分派帳號角色               |

