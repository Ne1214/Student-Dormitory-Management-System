## Schema：帳密管理

```sql
CREATE TABLE account_management (
    account_id VARCHAR(50) PRIMARY KEY,
    password_hash VARCHAR(255),
    salt VARCHAR(255),
    role VARCHAR(50),
    last_login DATETIME,
    created_at DATETIME,
    is_active BOOLEAN
);
```

## 範例

```sql
INSERT INTO account_management VALUES
('user1', 'hash1', 'salt1', 'student', '2025-05-01 10:00:00', '2023-09-01 12:00:00', TRUE);
```

## 說明

| 欄位            | 值                | 說明              |
| --------------- | ---------------- | ----------------- |
| account\_id     | user1            | 帳號唯一 ID        |
| password\_hash  | hash1            | 密碼雜湊           |
| salt            | salt1            | 加鹽值(隨機)        |
| role            | student          | 使用者角色         |
| last\_login     | 2025-05-01 10:00 | 最後登入時間       |
| created\_at     | 2023-09-01 12:00 | 建立時間           |
| is\_active      | TRUE             | 帳號是否啟用       |

---

## Schema：學生資料

```sql
CREATE TABLE student (
    student_id VARCHAR(50) PRIMARY KEY,
    account_id VARCHAR(50),
    name VARCHAR(100),
    gender VARCHAR(10),
    department VARCHAR(100),
    grade VARCHAR(50),
    phone VARCHAR(15),
    email VARCHAR(100),
    emergency_contact VARCHAR(100),
    emergency_contact_phone VARCHAR(15),
    created_at DATETIME,
    FOREIGN KEY (account_id) REFERENCES account_management(account_id)
);
```

## 範例

```sql
INSERT INTO student VALUES
('S001', 'user1', 'Alice', 'Female', 'Computer Science', 'Year 1', '123456789', 'alice@example.com', 'John', '987654321', '2023-09-01 12:00:00');
```

## 說明

| 欄位                     | 值                 | 說明            |
| ------------------------ | ----------------- | --------------- |
| student\_id              | S001              | 學生唯一 ID      |
| account\_id              | user1             | 帳號對應 ID      |
| name                     | Alice             | 學生姓名         |
| gender                   | Female            | 性別            |
| department               | Computer Science  | 系所            |
| grade                    | Year 1            | 年級            |
| phone                    | 123456789         | 聯絡電話         |
| email                    | alice@example.com | 電子郵件         |
| emergency\_contact       | John              | 緊急聯絡人       |
| emergency\_contact\_phone | 987654321         | 緊急聯絡電話      |
| created\_at              | 2023-09-01 12:00  | 建立時間         |

---

## Schema：宿舍大樓

```sql
CREATE TABLE dormitory_building (
    building_id INT PRIMARY KEY,
    building_name VARCHAR(100),
    address TEXT,
    total_floors INT,
    manager VARCHAR(100),
    is_active BOOLEAN
);
```

## 範例

```sql
INSERT INTO dormitory_building VALUES
(1, 'Maple Building', '123 Dormitory Lane', 5, 'Mr. Smith', TRUE);
```

## 說明

| 欄位             | 值                 | 說明           |
| ---------------- | ----------------- | -------------- |
| building\_id     | 1                 | 大樓唯一 ID     |
| building\_name   | Maple Building    | 大樓名稱        |
| address          | 123 Dormitory Lane | 大樓地址        |
| total\_floors    | 5                 | 樓層數          |
| manager          | Mr. Smith         | 管理員姓名      |
| is\_active       | TRUE              | 大樓是否啟用    |

---

---

## Schema：宿舍房間

```sql
CREATE TABLE dormitory_room (
    room_id INT PRIMARY KEY,
    building_id INT,
    room_number VARCHAR(50),
    floor INT,
    bed_count INT,
    room_type VARCHAR(50),
    monthly_rent DECIMAL(10, 2),
    is_available BOOLEAN,
    FOREIGN KEY (building_id) REFERENCES dormitory_building(building_id)
);
```

## 範例

```sql
INSERT INTO dormitory_room VALUES
(1, 1, 'A101', 1, 4, 'Standard', 1200.00, TRUE);
```

## 說明

| 欄位            | 值          | 說明               |
| --------------- | ----------- | ------------------ |
| room\_id        | 1           | 房間唯一 ID         |
| building\_id    | 1           | 所屬大樓的 ID       |
| room\_number    | A101        | 房號               |
| floor           | 1           | 所在樓層           |
| bed\_count      | 4           | 可容納床位數         |
| room\_type      | Standard    | 房型標準           |
| monthly\_rent   | 1200.00     | 每月租金（元）       |
| is\_available   | TRUE        | 是否為可用狀態       |

---

## Schema：床位分配

```sql
CREATE TABLE bed_allocation (
    allocation_id INT PRIMARY KEY,
    room_id INT,
    student_id VARCHAR(50),
    bed_number VARCHAR(50),
    check_in_date DATE,
    check_out_date DATE,
    current_status BOOLEAN,
    FOREIGN KEY (room_id) REFERENCES dormitory_room(room_id),
    FOREIGN KEY (student_id) REFERENCES student(student_id)
);
```

## 範例

```sql
INSERT INTO bed_allocation VALUES
(1, 1, 'S001', 'B1', '2025-01-15', NULL, TRUE);
```

## 說明

| 欄位               | 值          | 說明               |
| ------------------ | ----------- | ------------------ |
| allocation\_id     | 1           | 床位分配唯一 ID      |
| room\_id           | 1           | 房間 ID             |
| student\_id        | S001        | 學生 ID             |
| bed\_number        | B1          | 床位編號            |
| check\_in\_date    | 2025-01-15  | 入住宿舍的日期       |
| check\_out\_date   | NULL        | 退宿日期（NULL 表示尚未退宿）|
| current\_status    | TRUE        | 是否已入住此床位     |

---

## Schema：抽籤活動

```sql
CREATE TABLE lottery_activity (
    activity_id INT PRIMARY KEY,
    activity_name VARCHAR(100),
    academic_year VARCHAR(50),
    semester VARCHAR(10),
    application_start DATETIME,
    application_end DATETIME,
    lottery_date DATETIME,
    status VARCHAR(50)
);
```

## 範例

```sql
INSERT INTO lottery_activity VALUES
(1, '2025 Spring Lottery', '2024-2025', 'Spring', '2024-11-01 08:00:00', '2024-11-15 23:59:59', '2024-11-20 10:00:00', 'Completed');
```

## 說明

| 欄位               | 值                     | 說明              |
| ------------------ | --------------------- | ----------------- |
| activity\_id       | 1                     | 抽籤活動唯一 ID     |
| activity\_name     | 2025 Spring Lottery   | 活動名稱          |
| academic\_year     | 2024-2025            | 學年              |
| semester           | Spring               | 學期              |
| application\_start | 2024-11-01 08:00:00  | 申請開始時間       |
| application\_end   | 2024-11-15 23:59:59  | 申請結束時間       |
| lottery\_date      | 2024-11-20 10:00:00  | 抽籤時間          |
| status             | Completed            | 活動狀態          |

---

## Schema：抽籤申請

```sql
CREATE TABLE lottery_application (
    application_id INT PRIMARY KEY,
    activity_id INT,
    student_id VARCHAR(50),
    preferred_room_type VARCHAR(50),
    priority INT,
    application_date DATETIME,
    application_status VARCHAR(50),
    FOREIGN KEY (activity_id) REFERENCES lottery_activity(activity_id),
    FOREIGN KEY (student_id) REFERENCES student(student_id)
);
```

## 範例

```sql
INSERT INTO lottery_application VALUES
(1, 1, 'S001', 'Standard', 1, '2024-11-10 14:00:00', 'Accepted');
```

## 說明

| 欄位                  | 值                  | 說明              |
| --------------------- | ------------------ | ----------------- |
| application\_id       | 1                  | 抽籤申請唯一 ID     |
| activity\_id          | 1                  | 所屬抽籤活動的 ID   |
| student\_id           | S001               | 學生 ID            |
| preferred\_room\_type | Standard           | 志願房型          |
| priority              | 1                  | 申請優先順序        |
| application\_date     | 2024-11-10 14:00   | 申請時間          |
| application\_status   | Accepted           | 申請狀態          |

---

---

## Schema：抽籤結果

```sql
CREATE TABLE lottery_result (
    result_id INT PRIMARY KEY,
    application_id INT,
    room_id INT,
    result_status VARCHAR(50),
    lottery_time DATETIME,
    is_confirmed BOOLEAN,
    FOREIGN KEY (application_id) REFERENCES lottery_application(application_id),
    FOREIGN KEY (room_id) REFERENCES dormitory_room(room_id)
);
```

## 範例

```sql
INSERT INTO lottery_result VALUES
(1, 1, 1, 'Allocated', '2024-11-20 10:30:00', TRUE);
```

## 說明

| 欄位            | 值                  | 說明              |
| --------------- | ------------------ | ----------------- |
| result\_id      | 1                  | 抽籤結果唯一 ID     |
| application\_id | 1                  | 對應抽籤申請的 ID   |
| room\_id        | 1                  | 分配房間的 ID       |
| result\_status  | Allocated          | 抽籤結果狀態       |
| lottery\_time   | 2024-11-20 10:30   | 抽籤時間          |
| is\_confirmed   | TRUE               | 是否已確認接受此結果 |

---

## Schema：門禁卡

```sql
CREATE TABLE access_card (
    card_id VARCHAR(50) PRIMARY KEY,
    student_id VARCHAR(50),
    issue_date DATE,
    expiration_date DATE,
    is_active BOOLEAN,
    remarks TEXT,
    FOREIGN KEY (student_id) REFERENCES student(student_id)
);
```

## 範例

```sql
INSERT INTO access_card VALUES
('Card001', 'S001', '2024-01-01', '2025-01-01', TRUE, 'First card issued.');
```

## 說明

| 欄位          | 值                 | 說明                |
| ------------- | ------------------ | ------------------- |
| card\_id      | Card001            | 門禁卡唯一 ID        |
| student\_id   | S001               | 對應學生的 ID        |
| issue\_date   | 2024-01-01         | 發卡日期            |
| expiration\_date | 2025-01-01       | 到期日期            |
| is\_active    | TRUE               | 門禁卡是否啟用       |
| remarks       | First card issued. | 備註                |

---

## Schema：門禁記錄

```sql
CREATE TABLE access_log (
    log_id INT PRIMARY KEY,
    card_id VARCHAR(50),
    building_id INT,
    access_type VARCHAR(50),
    swipe_time DATETIME,
    result VARCHAR(50),
    FOREIGN KEY (card_id) REFERENCES access_card(card_id),
    FOREIGN KEY (building_id) REFERENCES dormitory_building(building_id)
);
```

## 範例

```sql
INSERT INTO access_log VALUES
(1, 'Card001', 1, 'Entry', '2025-01-15 08:00:00', 'Success');
```

## 說明

| 欄位          | 值                | 說明               |
| ------------- | ----------------- | ------------------ |
| log\_id       | 1                 | 門禁記錄唯一 ID      |
| card\_id      | Card001           | 使用門禁卡的 ID      |
| building\_id  | 1                 | 所屬大樓的 ID       |
| access\_type  | Entry             | 門禁類型（例如進入/離開） |
| swipe\_time   | 2025-01-15 08:00  | 刷卡時間           |
| result        | Success           | 刷卡結果           |

---

## Schema：訪客資料

```sql
CREATE TABLE visitor (
    visitor_id INT PRIMARY KEY,
    name VARCHAR(100),
    id_number VARCHAR(50),
    phone VARCHAR(15),
    visitor_type VARCHAR(50),
    created_at DATETIME
);
```

## 範例

```sql
INSERT INTO visitor VALUES
(1, 'John Doe', 'A123456789', '987654321', 'Parent', '2025-01-01 10:00:00');
```

## 說明

| 欄位          | 值                | 說明               |
| ------------- | ----------------- | ------------------ |
| visitor\_id   | 1                 | 訪客唯一 ID         |
| name          | John Doe          | 訪客姓名           |
| id\_number    | A123456789        | 訪客身分證號        |
| phone         | 987654321         | 訪客聯絡電話        |
| visitor\_type | Parent            | 訪客類型           |
| created\_at   | 2025-01-01 10:00  | 訪客資料建立時間     |

---

## Schema：訪客記錄

```sql
CREATE TABLE visitor_log (
    log_id INT PRIMARY KEY,
    visitor_id INT,
    student_id VARCHAR(50),
    building_id INT,
    visit_time DATETIME,
    leave_time DATETIME,
    purpose TEXT,
    approval_status VARCHAR(50),
    FOREIGN KEY (visitor_id) REFERENCES visitor(visitor_id),
    FOREIGN KEY (student_id) REFERENCES student(student_id),
    FOREIGN KEY (building_id) REFERENCES dormitory_building(building_id)
);
```

## 範例

```sql
INSERT INTO visitor_log VALUES
(1, 1, 'S001', 1, '2025-01-15 14:00:00', '2025-01-15 16:00:00', 'Deliver documents', 'Approved');
```

## 說明

| 欄位              | 值                  | 說明                |
| ----------------- | ------------------- | ------------------- |
| log\_id           | 1                   | 訪客記錄唯一 ID       |
| visitor\_id       | 1                   | 訪客的 ID            |
| student\_id       | S001                | 被訪學生的 ID         |
| building\_id      | 1                   | 所屬大樓的 ID         |
| visit\_time       | 2025-01-15 14:00    | 訪問時間             |
| leave\_time       | 2025-01-15 16:00    | 離開時間             |
| purpose           | Deliver documents   | 訪問事由             |
| approval\_status  | Approved            | 訪問狀態（核准/未核准） |

---

---

## Schema：水電度數記錄

```sql
CREATE TABLE utility_meter (
    meter_id INT PRIMARY KEY,
    room_id INT,
    current_electricity DECIMAL(10, 2),
    previous_electricity DECIMAL(10, 2),
    current_water DECIMAL(10, 2),
    previous_water DECIMAL(10, 2),
    reading_date DATE,
    reading_by VARCHAR(50),
    FOREIGN KEY (room_id) REFERENCES dormitory_room(room_id)
);
```

## 範例

```sql
INSERT INTO utility_meter VALUES
(1, 1, 120.5, 110.0, 25.5, 20.0, '2025-01-01', 'Staff A');
```

## 說明

| 欄位                   | 值        | 說明               |
| ---------------------- | --------- | ------------------ |
| meter\_id              | 1         | 水電表記錄唯一 ID      |
| room\_id               | 1         | 對應房間的 ID         |
| current\_electricity   | 120.5     | 本期電表度數（度）     |
| previous\_electricity  | 110.0     | 上期電表度數（度）     |
| current\_water         | 25.5      | 本期水表度數（度）     |
| previous\_water        | 20.0      | 上期水表度數（度）     |
| reading\_date          | 2025-01-01 | 抄表日期           |
| reading\_by            | Staff A   | 抄表人員            |

---

## Schema：水電帳單

```sql
CREATE TABLE utility_bill (
    bill_id INT PRIMARY KEY,
    room_id INT,
    meter_id INT,
    electricity_amount DECIMAL(10, 2),
    water_amount DECIMAL(10, 2),
    total_amount DECIMAL(10, 2),
    bill_date DATE,
    payment_due_date DATE,
    payment_status VARCHAR(50),
    FOREIGN KEY (room_id) REFERENCES dormitory_room(room_id),
    FOREIGN KEY (meter_id) REFERENCES utility_meter(meter_id)
);
```

## 範例

```sql
INSERT INTO utility_bill VALUES
(1, 1, 1, 300.0, 100.0, 400.0, '2025-01-05', '2025-01-20', 'Unpaid');
```

## 說明

| 欄位                 | 值        | 說明             |
| -------------------- | --------- | ---------------- |
| bill\_id             | 1         | 帳單唯一 ID       |
| room\_id             | 1         | 對應房間的 ID     |
| meter\_id            | 1         | 對應水電表記錄的 ID |
| electricity\_amount  | 300.0     | 電費（元）        |
| water\_amount        | 100.0     | 水費（元）        |
| total\_amount        | 400.0     | 總費用（元）       |
| bill\_date           | 2025-01-05 | 帳單日期         |
| payment\_due\_date   | 2025-01-20 | 繳費截止日期       |
| payment\_status      | Unpaid    | 繳費狀態         |

---

## Schema：報修單

```sql
CREATE TABLE repair_request (
    request_id INT PRIMARY KEY,
    student_id VARCHAR(50),
    room_id INT,
    category VARCHAR(50),
    description TEXT,
    severity VARCHAR(50),
    request_date DATETIME,
    status VARCHAR(50),
    FOREIGN KEY (student_id) REFERENCES student(student_id),
    FOREIGN KEY (room_id) REFERENCES dormitory_room(room_id)
);
```

## 範例

```sql
INSERT INTO repair_request VALUES
(1, 'S001', 1, 'Electrical', 'Light bulb not working', 'Normal', '2025-01-15 09:00:00', 'Pending');
```

## 說明

| 欄位            | 值                        | 說明               |
| --------------- | ------------------------- | ------------------ |
| request\_id     | 1                         | 報修單唯一 ID        |
| student\_id     | S001                      | 提出報修的學生 ID    |
| room\_id        | 1                         | 對應房間的 ID        |
| category        | Electrical                | 報修類別            |
| description     | Light bulb not working    | 報修描述            |
| severity        | Normal                    | 報修嚴重程度         |
| request\_date   | 2025-01-15 09:00:00       | 報修時間            |
| status          | Pending                   | 報修處理狀態         |

---

## Schema：公告內容

```sql
CREATE TABLE announcement (
    announcement_id INT PRIMARY KEY,
    category_id INT,
    title VARCHAR(100),
    content TEXT,
    posted_by VARCHAR(50),
    posted_at DATETIME,
    expires_at DATETIME,
    is_pinned BOOLEAN,
    views INT,
    FOREIGN KEY (category_id) REFERENCES announcement_category(category_id)
);
```

## 範例

```sql
INSERT INTO announcement VALUES
(1, 1, 'Maintenance Notice', 'Power outage on 2nd floor.', 'Admin', '2025-01-01 09:00:00', '2025-01-31 23:59:59', TRUE, 123);
```

## 說明

| 欄位             | 值                           | 說明           |
| ---------------- | ---------------------------- | -------------- |
| announcement\_id | 1                            | 公告唯一 ID     |
| category\_id     | 1                            | 所屬公告類別 ID |
| title            | Maintenance Notice           | 公告標題        |
| content          | Power outage on 2nd floor.  | 公告內容        |
| posted\_by       | Admin                        | 發布人          |
| posted\_at       | 2025-01-01 09:00:00         | 發布時間        |
| expires\_at      | 2025-01-31 23:59:59         | 失效時間        |
| is\_pinned       | TRUE                         | 是否置頂        |
| views            | 123                          | 瀏覽次數        |

---

---

## Schema：通知模板

```sql
CREATE TABLE notification_template (
    template_id INT PRIMARY KEY,
    template_name VARCHAR(100),
    notification_type VARCHAR(50),
    subject_template TEXT,
    body_template TEXT,
    is_active BOOLEAN
);
```

## 範例

```sql
INSERT INTO notification_template VALUES
(1, 'Payment Reminder', 'Email', 'Payment Due Reminder', 'Dear Student, your payment is due on {due_date}.', TRUE);
```

## 說明

| 欄位               | 值                           | 說明               |
| ------------------ | ---------------------------- | ------------------ |
| template\_id       | 1                            | 模板唯一 ID         |
| template\_name     | Payment Reminder             | 通知模板名稱         |
| notification\_type | Email                        | 通知類型 (Email 或 SMS) |
| subject\_template  | Payment Due Reminder         | 通知主旨模板         |
| body\_template     | Dear Student, your payment...| 通知內容模板         |
| is\_active         | TRUE                         | 模板是否啟用         |

---

## Schema：通知記錄

```sql
CREATE TABLE notification_log (
    log_id INT PRIMARY KEY,
    template_id INT,
    recipient_email VARCHAR(100),
    subject TEXT,
    message_content TEXT,
    sent_at DATETIME,
    status VARCHAR(50),
    related_student_id VARCHAR(50),
    FOREIGN KEY (template_id) REFERENCES notification_template(template_id)
);
```

## 範例

```sql
INSERT INTO notification_log VALUES
(1, 1, 'alice@example.com', 'Payment Due Reminder', 'Dear Student, your payment is due on 2025-01-15.', '2025-01-10 08:00:00', 'Sent', 'S001');
```

## 說明

| 欄位                 | 值                              | 說明               |
| -------------------- | ------------------------------ | ------------------ |
| log\_id              | 1                              | 通知記錄唯一 ID     |
| template\_id         | 1                              | 對應通知模板的 ID    |
| recipient\_email     | alice@example.com              | 收件人電子郵件地址   |
| subject              | Payment Due Reminder           | 通知主旨           |
| message\_content     | Dear Student, your payment ... | 通知內容           |
| sent\_at             | 2025-01-10 08:00:00           | 發送時間           |
| status               | Sent                           | 發送狀態           |
| related\_student\_id | S001                           | 對應的學生 ID       |

---

---

## Schema：費率設定

```sql
CREATE TABLE pricing_config (
    pricing_id INT PRIMARY KEY,
    pricing_type VARCHAR(50),
    unit_price DECIMAL(10, 2),
    effective_from DATE,
    effective_until DATE,
    is_active BOOLEAN
);
```

## 範例

```sql
INSERT INTO pricing_config VALUES
(1, 'Electricity', 5.00, '2025-01-01', '2025-12-31', TRUE);
```

## 說明

| 欄位              | 值          | 說明          |
| ----------------- | ----------- | ------------- |
| pricing\_id       | 1           | 費率唯一 ID    |
| pricing\_type     | Electricity | 費率類型       |
| unit\_price       | 5.00        | 單價（元/度）   |
| effective\_from   | 2025-01-01  | 生效日期       |
| effective\_until  | 2025-12-31  | 失效日期       |
| is\_active        | TRUE        | 是否為啟用狀態  |

---

## Schema：繳費記錄

```sql
CREATE TABLE payment_record (
    payment_id INT PRIMARY KEY,
    bill_id INT,
    student_id VARCHAR(50),
    amount_paid DECIMAL(10, 2),
    payment_date DATETIME,
    payment_method VARCHAR(50),
    receipt_number VARCHAR(50),
    FOREIGN KEY (bill_id) REFERENCES utility_bill(bill_id),
    FOREIGN KEY (student_id) REFERENCES student(student_id)
);
```

## 範例

```sql
INSERT INTO payment_record VALUES
(1, 1, 'S001', 400.00, '2025-01-15 10:00:00', 'Credit Card', 'R123456');
```

## 說明

| 欄位               | 值           | 說明             |
| ------------------ | ------------ | ---------------- |
| payment\_id        | 1            | 繳費記錄唯一 ID    |
| bill\_id           | 1            | 關聯的帳單 ID      |
| student\_id        | S001         | 繳費學生的 ID      |
| amount\_paid       | 400.00       | 繳費金額（元）     |
| payment\_date      | 2025-01-15   | 繳費時間          |
| payment\_method    | Credit Card  | 繳費方式          |
| receipt\_number    | R123456      | 收據編號          |

---

## Schema：維修記錄

```sql
CREATE TABLE maintenance_record (
    record_id INT PRIMARY KEY,
    request_id INT,
    maintenance_staff VARCHAR(50),
    start_time DATETIME,
    completion_time DATETIME,
    maintenance_details TEXT,
    cost DECIMAL(10, 2),
    remarks TEXT,
    FOREIGN KEY (request_id) REFERENCES repair_request(request_id)
);
```

## 範例

```sql
INSERT INTO maintenance_record VALUES
(1, 1, 'John Doe', '2025-01-20 09:00:00', '2025-01-20 11:00:00', 'Replaced light bulb', 35.00, 'No issues.');
```

## 說明

| 欄位                  | 值                  | 說明            |
| --------------------- | ------------------- | --------------- |
| record\_id            | 1                   | 維修記錄唯一 ID   |
| request\_id           | 1                   | 關聯的報修單 ID   |
| maintenance\_staff    | John Doe            | 維修人員姓名      |
| start\_time           | 2025-01-20 09:00    | 維修開始時間      |
| completion\_time      | 2025-01-20 11:00    | 維修完成時間      |
| maintenance\_details  | Replaced light bulb | 維修細節描述      |
| cost                  | 35.00               | 維修費用（元）     |
| remarks               | No issues.          | 備註            |

---

---

## Schema：公告類別

```sql
CREATE TABLE announcement_category (
    category_id INT PRIMARY KEY,
    category_name VARCHAR(100),
    description TEXT,
    display_order INT,
    is_active BOOLEAN
);
```

## 範例

```sql
INSERT INTO announcement_category VALUES
(1, 'Maintenance Notice', 'Notices about maintenance and repairs.', 1, TRUE),
(2, 'Event Announcement', 'Notices about upcoming events or activities.', 2, TRUE);
```

## 說明

| 欄位            | 值                     | 說明                  |
| --------------- | ---------------------- | --------------------- |
| category\_id    | 1                      | 公告類別唯一 ID         |
| category\_name  | Maintenance Notice     | 分類名稱（如維修通知、活動公告）|
| description     | Notices about maint... | 公告分類的描述          |
| display\_order  | 1                      | 展示順序，數字越小越優先 |
| is\_active      | TRUE                   | 是否啟用該公告分類      |

---
