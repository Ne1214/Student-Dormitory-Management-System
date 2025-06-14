!!!下面的篩選條件都是不輸入就不會使用

## 公告系統
```sql
SELECT
    a.announcement_id,
    ac.category_name,
    a.title,
    a.content,
    a.posted_by,
    a.posted_at,
    a.expires_at,
    a.is_pinned,
    a.views,
    CASE
        WHEN a.is_pinned = TRUE THEN 1
        ELSE 2
    END AS display_priority,
    DATEDIFF(a.expires_at, CURRENT_DATE) AS days_until_expiry
FROM Announcement a
JOIN AnnouncementCategory ac ON a.category_id = ac.category_id
WHERE a.expires_at <= CURRENT_DATE
  AND ac.is_active = TRUE
  AND (@category_id IS NULL OR a.category_id = @category_id)
  AND (@search_text IS NULL OR a.title LIKE CONCAT('%', @search_text, '%') OR a.content LIKE CONCAT('%', @search_text, '%'))
  AND (@start_date IS NULL OR a.posted_at >= @start_date)
  AND (@end_date IS NULL OR a.posted_at <= @end_date)
ORDER BY display_priority, a.posted_at DESC;
```
### 說明

學生和管理員查看當前有效的公告，
系統首頁展示重要通知。


|變數說明|說明|
|-------|----|
|@category_id|按公告分類篩選|
|@search_text|按公告標題或內容關鍵字搜索|
|@start_date|按公告發布時間的開始日期篩選|
|@end_date|按公告發布時間的結束日期篩選|
### 範例
![image](https://github.com/user-attachments/assets/ac03291d-4c7b-47f3-8bdd-0cd583cfca42)


## 門禁系統
```sql
SELECT
    al.log_id, al.swipe_time, ac.student_id, s.name AS student_name, db.building_name, al.access_type, al.result,
    CASE
        WHEN EXTRACT(HOUR FROM al.swipe_time) BETWEEN 0 AND 5 THEN '深夜時段'
        WHEN al.result != '成功' THEN '刷卡失敗'
        ELSE '正常'
    END AS alert_type
FROM AccessLog al
JOIN AccessCard ac ON al.card_id = ac.card_id
JOIN student s ON ac.student_id = s.student_id
JOIN DormitoryBuilding db ON al.building_id = db.building_id
WHERE (@student_id IS NULL OR ac.student_id = @student_id)
    AND (@building_name IS NULL OR db.building_name = @building_name)
    AND (@alert_type IS NULL OR (
        CASE
            WHEN EXTRACT(HOUR FROM al.swipe_time) BETWEEN 0 AND 5 THEN '深夜時段'
            WHEN al.result != '成功' THEN '刷卡失敗'
            ELSE '正常'
        END = @alert_type
    ))
    AND (@start_date IS NULL OR DATE(al.swipe_time) >= @start_date)
    AND (@end_date IS NULL OR DATE(al.swipe_time) <= @end_date)
ORDER BY al.swipe_time DESC;
```
### 說明
宿舍管理員監控異常進出記錄，
包括深夜進出和刷卡失敗的情況。


|變數說明|說明|
|-------|----|
|@student_id|按學生學號篩選|
|@building_name|按宿舍樓名篩選|
|@alert_type|按異常類型篩選|
|@start_date|按照刷卡時間的開始日期篩選|
|@end_date|按照刷卡時間的結束日期篩選|

### 範例
![image](https://github.com/user-attachments/assets/08e963bd-c287-4967-98fb-3433e1dba1e4)

## 抽籤系統
```sql
SELECT
    s.student_id,
    s.name AS student_name,
    s.department,
    s.grade,
    la.activity_name,
    la.academic_year,
    la.semester,
    lap.preferred_room_type,
    lap.application_date,
    lap.application_status,
    lr.result_status,
    db.building_name,
    dr.room_number,
    dr.monthly_rent
FROM student s
LEFT JOIN LotteryApplication lap ON s.student_id = lap.student_id
LEFT JOIN LotteryActivity la ON lap.activity_id = la.activity_id
LEFT JOIN LotteryResult lr ON lap.application_id = lr.application_id
LEFT JOIN DormitoryRoom dr ON lr.room_id = dr.room_id
LEFT JOIN DormitoryBuilding db ON dr.building_id = db.building_id
WHERE (@status IS NULL OR la.status = @status)
  AND (@student_id IS NULL OR s.student_id = @student_id)
  AND (@application_status IS NULL OR lap.application_status = @application_status)
  AND (@building_name IS NULL OR db.building_name = @building_name)
  AND (@room_type IS NULL OR dr.room_type = @room_type)
ORDER BY lap.application_date DESC;
```
### 說明
學生查詢自己的抽籤申請狀態、結果，
以及分配到的宿舍資訊，NULL表示沒中。


|變數說明|說明|
|-------|----|
|@status|是抽籤狀態篩選|
|@student_id|按學生學號篩選|
|@application_status|按申請狀態篩選|
|@building_name|按宿舍樓名篩選|
|@room_type|按房間類型篩選|

### 範例
![image](https://github.com/user-attachments/assets/2379a058-6985-47d3-ac6a-9221b19a26c3)


## 床位管理系統
```sql
SELECT
    db.building_id,    db.building_name,
    dr.room_type,      dr.floor,
    COUNT(DISTINCT dr.room_id) AS total_rooms,
    SUM(dr.bed_count) AS total_beds,
    SUM(CASE WHEN ba.current_status = TRUE THEN 1 ELSE 0 END) AS occupied_beds,
    SUM(dr.bed_count) - SUM(CASE WHEN ba.current_status = TRUE THEN 1 ELSE 0 END) AS available_beds,
    dr.monthly_rent
FROM DormitoryBuilding db
JOIN DormitoryRoom dr ON db.building_id = dr.building_id
LEFT JOIN BedAllocation ba ON dr.room_id = ba.room_id AND ba.current_status = TRUE
WHERE db.is_active = TRUE AND dr.is_available = TRUE
    AND (@building_name IS NULL OR db.building_name = @building_name)
    AND (@room_type IS NULL OR dr.room_type = @room_type)
    AND (@floor IS NULL OR dr.floor = @floor)
GROUP BY db.building_id, db.building_name, dr.room_type, dr.floor, dr.monthly_rent;
```
### 說明
檢視各宿舍樓層尚未被
使用的床位資訊和房型價格


|變數說明|說明|
|-------|----|
|@building_name|按宿舍樓名篩選|
|@room_type|按房間類型篩選|
|@floor|按樓層篩選|

### 範例
![image](https://github.com/user-attachments/assets/b4f2ff37-f313-4859-8f10-6049c4dd31fd)

