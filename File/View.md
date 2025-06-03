# 👥 系統身分

| 角色身分（Role）           | 權限範圍說明                                       | 主要用途說明                           |
|----------------------------|----------------------------------------------------|----------------------------------------|
| 系統管理員（Admin）        | 可管理所有平台資料，包含帳號、訂單、商品等         | 總覽全平台帳號、訂單與業績分析         |
| 賣家（Seller）             | 僅能看到自己店家的商品、訂單、評價                 | 管理商品、訂單與顧客回饋               |
| 顧客（Customer）           | 僅能看到自己下的訂單、收藏、評價                   | 查詢個人資料與訂單紀錄                 |
| 倉儲人員（Warehouse）      | 只能查看倉儲、庫存與調度                           | 管理貨品出入與庫存更新                 |
| 財務人員（Finance）        | 查看訂單、退款、發票與付款資料                     | 審核款項與退款處理                     |
| 行銷/營運（Marketing）     | 分析熱門商品、訂單轉換率、用戶行為                 | 追蹤用戶成效與廣告 ROI                 |
| 客服人員（Support）        | 查看顧客訂單、評價與客服聯絡紀錄                   | 協助顧客查詢訂單、處理問題與退換貨     |


# View 

### `membership_levels` 欄位可視權限表

| 欄位名稱         | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| `level_id`       |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `level_name`     |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |
| `required_points`|  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `discount_rate`  |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |
| `created_at`     |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| `updated_at`     |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |

1. 系統管理員（Admin）  
> 用於管理會員等級與優惠調整設定  
```sql
CREATE VIEW admin_membership_levels_view AS
SELECT *
FROM membership_levels;
```
📌 用途：會員等級管理與稽核

![image](https://github.com/user-attachments/assets/4e12792a-4674-4f20-9f3b-4ab1534b229b)

2. 賣家（Seller）  
> 了解顧客的會員等級與對應優惠  
```sql
CREATE VIEW seller_membership_levels_view AS
SELECT level_id, level_name, required_points, discount_rate
FROM membership_levels;
```
📌 用途：出貨/行銷依據的會員等級分析

![image](https://github.com/user-attachments/assets/bfc273d6-d198-40b6-846c-499dbaf15c6f)


3. 顧客（Customer）  
> 顧客查詢自身等級與升級條件、優惠資訊  
```sql
CREATE VIEW customer_membership_levels_view AS
SELECT level_id, level_name, required_points, discount_rate
FROM membership_levels;
```
📌 用途：顧客等級查詢、升等動機引導

![image](https://github.com/user-attachments/assets/290da991-d8c5-4626-a912-28dd1eaec940)



4. 倉儲人員（Warehouse）  
> 無會員等級存取需求  
📌 用途：不授權存取會員等級資訊

5. 財務人員（Finance）  
> 僅需了解折扣率以計算財務報表  
```sql
CREATE VIEW finance_membership_levels_view AS
SELECT level_name,discount_rate
FROM membership_levels;
```
📌 用途：折扣率報表與銷售預估

![image](https://github.com/user-attachments/assets/c4bd5250-ed7a-468b-8ab5-858e1511511a)


6. 行銷/營運（Marketing）  
> 用於活動規劃與會員推升策略分析  
```sql
CREATE VIEW marketing_membership_levels_view AS
SELECT level_id, level_name, required_points, discount_rate, created_at, updated_at
FROM membership_levels;
```
📌 用途：等級制度設計與會員經營分析

![image](https://github.com/user-attachments/assets/caa263fb-0b1b-47d5-8d26-39644684e4c4)


7. 客服人員（Support）  
> 需要查詢等級名稱與點數門檻，幫助顧客了解升級條件  
```sql
CREATE VIEW support_membership_levels_view AS
SELECT level_id, level_name, required_points
FROM membership_levels;
```
📌 用途：客服說明「您還差 XX 點就升等」等功能

![image](https://github.com/user-attachments/assets/31618129-6fad-4ded-b82e-399a9f10f567)


## users

### `users` 欄位可視權限表

| 欄位名稱             | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|----------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| `user_id`            |  ✔   |   ✔   |    ✘     |     ✘     |    ✔    |     ✔     |    ✔    |
| `username`           |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |
| `email`              |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |
| `phone`              |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| `registration_date`  |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `is_active`          |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✘     |    ✔    |
| `membership_level_id`|  ✔   |   ✘  |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |
| `shipping_address`   |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✘     |    ✘    |
| `billing_address`    |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✘    |

1. 系統管理員（Admin）
> 查看所有使用者完整資訊以進行帳號管理與審核

```sql
CREATE VIEW admin_user_view AS
SELECT *
FROM users;
```
📌 用途：完整帳號管理、稽核與驗證

![image](https://github.com/user-attachments/assets/3d40d66b-1a97-48c6-be4c-4003a5116777)


2. 賣家（Seller）
> 用於查看顧客的聯絡方式與收貨資訊，以處理訂單出貨
```sql
CREATE VIEW seller_user_view AS
SELECT user_id, username, email, phone, is_active, shipping_address
FROM users;
```
📌 用途：顧客出貨資訊、會員等級追蹤

![image](https://github.com/user-attachments/assets/379a5063-45b8-4395-b054-26e25f181611)


3. 顧客（Customer）
> 用於顧客查看個人資料
```sql
CREATE VIEW customer_user_view AS
SELECT username, email, phone, registration_date, membership_level_id, shipping_address, billing_address
FROM users;
```
📌 用途：顧客個資查詢與修改介面

![image](https://github.com/user-attachments/assets/3839a0de-7d0e-4c7d-8f9b-d97c7aa322a6)

4. 倉儲人員（Warehouse）
> 無使用者資訊存取需求
📌 用途：不授權存取使用者資訊

5. 財務人員（Finance）
> 僅查看帳單與收付款聯絡資料
```sql
CREATE VIEW finance_user_view AS
SELECT user_id,username , email, phone,membership_level_id , billing_address
FROM users;
```
📌 用途：帳務聯繫與帳單寄送

![image](https://github.com/user-attachments/assets/88e1fd18-1c68-41c1-8ff9-91a020926ec8)

6. 行銷/營運（Marketing）
> 分析會員活躍度與聯絡管道
```sql
CREATE VIEW marketing_user_view AS
SELECT user_id, username, email, registration_date, membership_level_id
FROM users;
```
📌 用途：會員成長與行銷策略追蹤

![image](https://github.com/user-attachments/assets/4df7f737-daac-4d5e-bdf9-468e9ea96b90)


7. 客服人員（Support）
> 查看顧客聯絡方式與會員等級以協助處理問題
```sql
CREATE VIEW support_user_view AS
SELECT user_id, username, email, phone, registration_date, is_active, membership_level_id
FROM users;
```
📌 用途：協助顧客查詢等級、聯絡方式與註冊狀態

![image](https://github.com/user-attachments/assets/80aa2a52-501d-4ce1-906e-b414a5fdaa5e)

### `sellers` 欄位可視權限表

| 欄位名稱           | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|--------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| `seller_id`        |  ✔   |   ✔   |    ✘     |     ✘     |    ✔    |     ✔     |    ✘    |
| `user_id`          |  ✔   |   ✔   |    ✘     |     ✘     |    ✔    |     ✔     |    ✘    |
| `store_name`       |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `store_description`|  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `bank_account`     |  ✔   |   ✔   |    ✘     |     ✘     |    ✔    |     ✘     |    ✘    |
| `created_at`       |  ✔   |   ✔   |    ✘     |     ✘     |    ✔    |     ✔     |    ✘    |
| `updated_at`       |  ✔   |   ✔   |    ✘     |     ✘     |    ✔    |     ✔     |    ✘    |

1. 系統管理員（Admin）  
> 查看與管理賣家基本資料、帳戶資訊與商店營運狀況  
```sql
CREATE VIEW admin_seller_view AS
SELECT *
FROM sellers;
```
📌 用途：商店與賣家帳號管理、審核與稽核用途

![image](https://github.com/user-attachments/assets/32dd74cb-26b9-48d1-8ed9-a6a241dbabb5)

2. 賣家（Seller）  
> 管理自己商店資訊  
```sql
CREATE VIEW seller_seller_view AS
SELECT seller_id, user_id, store_name, store_description, bank_account, created_at, updated_at
FROM sellers;
```
📌 用途：賣家查詢與維護自己的店家資訊與帳戶

![image](https://github.com/user-attachments/assets/b726b406-e701-4bdb-82c4-9e7e6edd05fe)

3. 顧客（Customer）  
> 僅顯示店家名稱與簡介，用於商城展示  
```sql
CREATE VIEW customer_seller_view AS
SELECT store_name, store_description
FROM sellers;
```
📌 用途：商城店家頁面展示資訊

![image](https://github.com/user-attachments/assets/c2f72627-63b4-4d9a-a2b4-8d3c7f021d66)


4. 倉儲人員（Warehouse）  
> 無賣家資料存取需求  
📌 用途：不授權存取賣家資訊

5. 財務人員（Finance）  
> 用於查帳或轉帳賣家資訊  
```sql
CREATE VIEW finance_seller_view AS
SELECT seller_id, user_id, bank_account, created_at, updated_at
FROM sellers;
```
📌 用途：賣家出帳與匯款用途

![image](https://github.com/user-attachments/assets/24f89e5d-467f-458a-8fcd-f5597b93d142)

6. 行銷/營運（Marketing）  
> 分析店家成長與營運時間分布  
```sql
CREATE VIEW marketing_seller_view AS
SELECT seller_id, user_id, store_name, store_description, created_at, updated_at
FROM sellers;
```
📌 用途：商店行銷活動規劃、營運統計與報告

![image](https://github.com/user-attachments/assets/6286145d-ed66-4b1e-9cb4-7f616204749d)


7. 客服人員（Support）  
> 提供店家基本資訊給顧客查詢或處理爭議  
```sql
CREATE VIEW support_seller_view AS
SELECT store_name, store_description
FROM sellers;
```
📌 用途：客服查詢店家資訊以協助顧客

![image](https://github.com/user-attachments/assets/72f246db-abe6-4163-8e88-49060ca5ee4e)


### `Points` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| point_id        |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| user_id         |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| points_earned   |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| transaction_date|  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| description     |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |

1. 系統管理員（Admin）  
> 所有用戶點數交易記錄
```sql
CREATE VIEW admin_points_view AS
SELECT * FROM points;
```
📌 用途：點數系統全面監控與稽核 

![image](https://github.com/user-attachments/assets/cfe3a182-5f1c-4212-b741-0092deac9b4f)

3. 顧客（Customer）  
> 個人點數獲取與使用記錄
```sql
CREATE VIEW customer_points_view AS
SELECT point_id, user_id, points_earned, transaction_date, description
FROM points
WHERE user_id = CURRENT_USER();
```
📌 用途：點數餘額查詢與兌換追蹤 
* 以user 2 為例
* 
![image](https://github.com/user-attachments/assets/5eda139e-b3ee-4baa-8db3-5e5ec7b93c4d)


5. 財務人員（Finance）  
> 點數發放與使用記錄
```sql
CREATE VIEW finance_points_view AS
SELECT point_id, user_id, points_earned, transaction_date
FROM points;
```
📌 用途：點數資產負債核算 

![image](https://github.com/user-attachments/assets/ca0edec8-f920-4a4f-96f7-687da9972b66)

7. 客服人員（Support）  
> 客戶點數交易記錄
```sql
CREATE VIEW support_points_view AS
SELECT point_id, user_id, points_earned, transaction_date, description
FROM points;
```
📌 用途：點數問題排查與補償處理 

![image](https://github.com/user-attachments/assets/7f45d41d-53c9-4c96-9d38-5ffde2363641)


### `Coupons` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| coupon_id       |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| coupon_code     |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✔     |    ✘    |
| discount_amount |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✔     |    ✘    |
| expiry_date     |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✔     |    ✘    |
| usage_limit     |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| is_active       |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| coupon_type     |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| created_at      |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |

1. 系統管理員（Admin）  
> 所有優惠券完整資訊
```sql
CREATE VIEW admin_coupons_view AS
SELECT * FROM coupons;
```
📌 用途：優惠活動全面管理與分析 

![image](https://github.com/user-attachments/assets/14adf1b8-6e79-4df5-893a-bfe080466ca2)


6. 行銷/營運（Marketing）  
> 優惠券活動數據
```sql
CREATE VIEW marketing_coupons_view AS
SELECT coupon_id, coupon_code, discount_amount, expiry_date, 
       usage_limit, is_active, coupon_type, created_at
FROM coupons;
```
📌 用途：促銷活動規劃與效果追蹤 

![image](https://github.com/user-attachments/assets/a1191345-ab71-4702-a94f-983975306db5)


3. 顧客（Customer）  
> 當前有效優惠券列表
```sql
CREATE VIEW customer_coupons_view AS
SELECT coupon_id, coupon_code, discount_amount, expiry_date
FROM coupons
WHERE is_active = TRUE AND (expiry_date IS NULL OR expiry_date >= CURDATE());
```
📌 用途：優惠券查詢與結帳使用 

![image](https://github.com/user-attachments/assets/30876a08-3213-43e6-a259-f212313294bb)


### `Messages` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| message_id      |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| sender_id       |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| receiver_id     |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| message_content |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| send_time       |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| is_read         |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |

1. 系統管理員（Admin）  
> 所有用戶間通訊記錄
```sql
CREATE VIEW admin_messages_view AS
SELECT * FROM messages;
```
📌 用途：通訊安全監控與爭議處理 

![image](https://github.com/user-attachments/assets/e41873c0-e356-4780-a8a3-497fdd558b9c)


3. 顧客（Customer）  
> 個人收發訊息記錄
```sql
CREATE VIEW customer_messages_view AS
SELECT message_id, sender_id, receiver_id, message_content, send_time, is_read
FROM messages
WHERE sender_id = CURRENT_USER() OR receiver_id = CURRENT_USER();
```
![image](https://github.com/user-attachments/assets/196eabf1-7aa3-4ea1-a1a0-0529a375023d)


📌 用途：買賣雙方溝通歷史查詢 

7. 客服人員（Support）  
> 訊息元數據查詢
```sql
CREATE VIEW support_messages_view AS
SELECT message_id, sender_id, receiver_id, send_time, is_read
FROM messages;
```
📌 用途：糾紛調解與通訊異常處理 

![image](https://github.com/user-attachments/assets/aeab6f20-83bd-4dea-b462-abd05dad0bcb)


### `Notifications` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| notification_id |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| user_id         |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| notification_type| ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| subject         |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| message         |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| sent_at         |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| is_read         |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| related_entity  |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| related_entity_id| ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |

1. 系統管理員（Admin）  
> 所有系統通知記錄
```sql
CREATE VIEW admin_notifications_view AS
SELECT * FROM notifications;
```
📌 用途：通知系統效能監控與分析 

![image](https://github.com/user-attachments/assets/16c77dd9-0aff-4943-9868-bd2220677d4c)


3. 顧客（Customer）  
> 個人系統通知列表
```sql
CREATE OR REPLACE VIEW customer_notifications_view AS
SELECT notification_id, notification_type, subject, 
       message, sent_at, is_read, related_entity, related_entity_id,user_id
FROM notifications
WHERE user_id = CURRENT_USER();
```
![image](https://github.com/user-attachments/assets/177bc196-b2b5-4ac5-a319-73df0b162a15)


📌 用途：訂單狀態更新與活動通知查閱 

6. 行銷/營運（Marketing）  
> 通知發送統計數據
```sql
CREATE VIEW marketing_notifications_view AS
SELECT notification_id, user_id, notification_type, 
       sent_at, related_entity
FROM notifications;
```
📌 用途：用戶觸達效果分析與優化 

![image](https://github.com/user-attachments/assets/815ed792-70e4-4afd-8522-9b849aecd0ec)

7. 客服人員（Support）  
> 客戶通知記錄
```sql
CREATE VIEW support_notifications_view AS
SELECT notification_id, user_id, notification_type, 
       subject, sent_at, is_read
FROM notifications;
```
📌 用途：通知補發與客戶查詢處理 

![image](https://github.com/user-attachments/assets/e439822a-2280-471d-be60-34063c85834e)


## 倉庫管理

### `Warehouses` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| warehouse_id    |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| warehouse_name  |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| location       |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| capacity       |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| manager_id     |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| contact_info   |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| created_at     |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |

1. 系統管理員（Admin）
>完整倉庫管理資訊
```sql
CREATE VIEW admin_warehouses_view AS
SELECT * FROM warehouses;
```
📌 用途：系統後台進行倉庫全面管理與稽核

![image](https://github.com/user-attachments/assets/63a3d32a-e350-401e-a0fc-a9198c6ec383)


2. 倉儲人員（Warehouse）
>倉庫基本運營資訊
```spl
CREATE VIEW warehouse_warehouses_view AS
SELECT * FROM warehouses;
```
📌 用途：日常倉庫管理與維護作業

![image](https://github.com/user-attachments/assets/8ef68b4b-e4eb-4535-9cea-d6f68f38af13)


### `Inventory` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| inventory_id    |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| warehouse_id    |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| product_id      |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| stock_quantity  |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| reorder_level   |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| last_updated    |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |

1. 系統管理員（Admin）
>完整庫存狀態監控
```sql
CREATE VIEW admin_inventory_view AS
SELECT * FROM inventory;
```
![image](https://github.com/user-attachments/assets/2875d759-2e47-420a-9773-7203ddfd8f7b)

📌 用途：全局庫存分析與異常監測

2. 賣家（Seller）
>賣家相關商品庫存狀態
```sql
CREATE VIEW seller_inventory_view AS
SELECT inventory_id, warehouse_id, product_id, stock_quantity, reorder_level, last_updated
FROM inventory;
```

![image](https://github.com/user-attachments/assets/3dcb4165-5c82-413c-974d-9cd74fa82bef)


📌 用途：商品備貨與補貨決策依據

3. 倉儲人員（Warehouse）
>實時庫存操作介面
```sql
CREATE VIEW warehouse_inventory_view AS
SELECT * FROM inventory;
```

📌 用途：每日盤點與庫位管理

![image](https://github.com/user-attachments/assets/aafec53c-bf5a-46d0-9cd3-daa6404bf09d)


### `Suppliers` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| supplier_id     |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| supplier_name   |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| contact_info    |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| country         |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| rating          |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| created_at      |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |

1. 系統管理員（Admin）
供應商完整檔案
```sql
CREATE VIEW admin_suppliers_view AS
SELECT * FROM suppliers;
```
📌 用途：供應鏈全生命周期管理

![image](https://github.com/user-attachments/assets/ada18f49-03e8-48f8-8641-1420d56660bc)


2. 賣家（Seller）
>合作供應商基本資料
```sql
CREATE VIEW seller_suppliers_view AS
SELECT supplier_id, supplier_name, contact_info, country, rating, created_at
FROM suppliers;
```
![image](https://github.com/user-attachments/assets/bf8c5a78-8d7e-4fcd-81f0-e990cab005b9)


📌 用途：採購決策與供應商評估

3. 倉儲人員（Warehouse）
>物流聯絡所需資訊
```sql
CREATE VIEW warehouse_suppliers_view AS
SELECT supplier_id, supplier_name, contact_info, country, rating
FROM suppliers;
```

![image](https://github.com/user-attachments/assets/d583817a-80a1-4ad9-bf32-231c2f2c518f)


📌 用途：到貨異常時緊急聯絡

### `Inbound_Shipments` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| inbound_id      |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| supplier_id     |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| warehouse_id    |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| product_id      |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| quantity        |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| arrival_date    |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| status          |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |

1. 系統管理員（Admin）
>完整入庫流水記錄
```sql
CREATE VIEW admin_inbound_view AS
SELECT * FROM inbound_shipments;
```
📌 用途：入庫流程審計與分析

![image](https://github.com/user-attachments/assets/cc24f134-b332-4c2c-88b5-2d8f5e79c672)


2. 賣家（Seller）  
> 商品採購入庫狀態
```sql
CREATE VIEW seller_inbound_view AS
SELECT inbound_id, supplier_id, warehouse_id, product_id, quantity, arrival_date, status
FROM inbound_shipments;
```

📌 用途：追蹤採購單到貨情況

![image](https://github.com/user-attachments/assets/c4392cf2-e53b-41b4-bf7a-748d4ebbbc45)


3. 倉儲人員（Warehouse）  
> 入庫作業明細
```sql
CREATE VIEW warehouse_inbound_view AS
SELECT * FROM inbound_shipments;
```
![image](https://github.com/user-attachments/assets/66a8362e-c79f-4d80-b818-aea5e2ddf6b7)


📌 用途：實際收貨與驗收入庫

### `Outbound_Shipments` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| outbound_id     |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| order_id        |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✘     |    ✔    |
| warehouse_id    |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| product_id      |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| quantity        |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| dispatch_date   |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| status          |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |

1. 系統管理員（Admin）  
>完整出庫歷史記錄
```sql
CREATE VIEW admin_outbound_view AS
SELECT * FROM outbound_shipments;
```

![image](https://github.com/user-attachments/assets/9a64db8b-bf15-4120-a3c0-e93a428480f7)


📌 用途：出庫異常分析與追溯

2. 賣家（Seller）  
>商品出庫狀態監控
```sql
CREATE VIEW seller_outbound_view AS
SELECT outbound_id, order_id, warehouse_id, product_id, quantity, dispatch_date, status
FROM outbound_shipments;
```

![image](https://github.com/user-attachments/assets/d3505bd4-9f1f-4d3e-aa2e-d767246125fd)


📌 用途：訂單履約進度追蹤

3. 倉儲人員（Warehouse）  
>出庫作業單據
```sql
CREATE VIEW warehouse_outbound_view AS
SELECT * FROM outbound_shipments;
```
📌 用途：實際揀貨與出庫作業

![image](https://github.com/user-attachments/assets/d1bfc204-3721-49e8-b7b2-324c5b7db3fe)


4. 客服人員（Support）  
>客戶出庫查詢介面
```sql
CREATE VIEW support_outbound_view AS
SELECT outbound_id, order_id, product_id, quantity, dispatch_date, status
FROM outbound_shipments;
```

📌 用途：解答客戶物流進度問題

![image](https://github.com/user-attachments/assets/4cd9a1ea-a64c-4532-9f9b-94c605f21abe)




📌 用途：退換貨處理、訂單爭議解決 

### `Shipments` 欄位可視權限表
| 欄位                | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|---------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| shipment_id         |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✘     |    ✔    |
| order_id           |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✘     |    ✔    |
| tracking_number    |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✘     |    ✔    |
| shipment_status    |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✘     |    ✔    |
| estimated_delivery |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✘     |    ✔    |
| actual_delivery    |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✘     |    ✔    |
| carrier            |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✘     |    ✔    |
| shipping_cost      |  ✔   |   ✔   |    ✘     |     ✔     |    ✔    |     ✘     |    ✔    |

1. 系統管理員（Admin）  
> 所有物流配送記錄
```sql
CREATE VIEW admin_shipments_view AS
SELECT * FROM shipments;
```

📌 用途：物流效能分析、運費結算 

![image](https://github.com/user-attachments/assets/9c8f3e14-76f9-408a-9679-c61045ee7dd4)


2. 賣家（Seller）  
> 賣家相關物流資訊
```sql
CREATE VIEW seller_shipments_view AS
SELECT shipment_id, order_id, tracking_number, shipment_status, estimated_delivery, actual_delivery, carrier
FROM shipments;
```

📌 用途：物流進度追蹤、客戶通知 

![image](https://github.com/user-attachments/assets/69b14d6c-c60a-42ec-be04-3f0a84a320ba)


3. 顧客（Customer）  
> 個人訂單物流狀態
```sql
CREATE VIEW customer_shipments_view AS
SELECT s.shipment_id, s.order_id, s.tracking_number, s.shipment_status, 
       s.estimated_delivery, s.actual_delivery, s.carrier,o.customer_name
FROM shipments s
JOIN orders o ON s.order_id = o.order_id
WHERE o.customer_id = CURRENT_USER();
```
![image](https://github.com/user-attachments/assets/7127aced-1a17-4c9c-83a7-9b569652f634)


📌 用途：包裹追蹤、收貨準備 

4. 倉儲人員（Warehouse）  
> 物流出庫關聯資訊
```sql
CREATE VIEW warehouse_shipments_view AS
SELECT shipment_id, order_id, tracking_number, shipment_status, carrier
FROM shipments;
```

![image](https://github.com/user-attachments/assets/2e0a2577-f028-4315-ad15-b8d8cb1d2076)


📌 用途：物流交接、運單打印 

5. 客服人員（Support）  
> 客戶物流查詢介面
```sql
CREATE VIEW support_shipments_view AS
SELECT shipment_id, order_id, tracking_number, shipment_status, 
       estimated_delivery, actual_delivery, carrier
FROM shipments;
```

![image](https://github.com/user-attachments/assets/5c56b118-b637-4390-8126-fe520bd49900)


📌 用途：物流異常處理、客戶諮詢 

### `Warehouse_Transfers` 欄位可視權限表
| 欄位                | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|---------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| transfer_id         |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| from_warehouse_id   |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| to_warehouse_id     |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| product_id          |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| quantity            |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| transfer_status     |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| transfer_date       |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |

1. 系統管理員（Admin）  
> 所有倉庫調撥記錄
```sql
CREATE VIEW admin_transfers_view AS
SELECT * FROM warehouse_transfers;
```

![image](https://github.com/user-attachments/assets/a60b5e04-82e5-4519-9d41-505ff9991778)


📌 用途：庫存調度分析、倉庫效能評估 

2. 倉儲人員（Warehouse）  
> 倉庫間調撥作業單
```sql
CREATE VIEW warehouse_transfers_view AS
SELECT * FROM warehouse_transfers;
```
![image](https://github.com/user-attachments/assets/d21ff1bf-7d0e-4e2f-b3e8-ef64e8a2a50b)



## 數據分析

### `Sales_Analysis` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| record_id       |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| date           |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| product_id     |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| category_id    |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| total_sales    |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| revenue        |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| average_price  |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |

1. 系統管理員（Admin）  
> 完整銷售分析數據
```sql
CREATE VIEW admin_sales_analysis_view AS
SELECT * FROM sales_analysis;
```
📌 用途：業務決策支持、績效評估 

![image](https://github.com/user-attachments/assets/8408af8d-4791-413b-995b-e18e1f56db94)

2. 賣家（Seller）  
> 賣家銷售績效數據
```sql
CREATE VIEW seller_sales_analysis_view AS
SELECT record_id, date, product_id, category_id, total_sales, revenue, average_price
FROM sales_analysis;
```
📌 用途：銷售策略調整、商品優化 

![image](https://github.com/user-attachments/assets/c74fd2b2-07b9-44dc-900a-3f6344e11ce1)


3. 行銷/營運（Marketing）  
> 銷售趨勢分析數據
```sql
CREATE VIEW marketing_sales_analysis_view AS
SELECT * FROM sales_analysis;
```
📌 用途：促銷活動規劃、市場定位 

![image](https://github.com/user-attachments/assets/db4c3172-8998-4877-98c1-bf4f8a84d722)


### `Inventory_Analytics` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| record_id       |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |
| date           |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |
| warehouse_id   |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |
| product_id     |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |
| starting_stock |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |
| ending_stock   |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |
| sold_units     |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |
| received_units |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |

1. 系統管理員（Admin）  
> 完整庫存流動數據
```sql
CREATE VIEW admin_inventory_analytics_view AS
SELECT * FROM inventory_analytics;
```
📌 用途：庫存周轉分析、採購策略 

![image](https://github.com/user-attachments/assets/facb037c-75fb-41c0-a05b-795e0a42c67a)


2. 賣家（Seller）  
> 商品庫存流動趨勢
```sql
CREATE VIEW seller_inventory_analytics_view AS
SELECT record_id, date, warehouse_id, product_id, starting_stock, ending_stock, sold_units, received_units
FROM inventory_analytics;
```
📌 用途：庫存健康度監測、補貨預測 

![image](https://github.com/user-attachments/assets/2742b941-da33-4a08-b80b-af37a64e82de)


3. 倉儲人員（Warehouse）  
> 庫存操作效能數據
```sql
CREATE VIEW warehouse_inventory_analytics_view AS
SELECT * FROM inventory_analytics;
```
📌 用途：倉儲效率優化、作業排程 

![image](https://github.com/user-attachments/assets/7740ed91-3898-4626-bcfa-b6540af9eb84)


4. 行銷/營運（Marketing）  
> 商品流動關聯數據
```sql
CREATE VIEW marketing_inventory_analytics_view AS
SELECT record_id, date, warehouse_id, product_id, sold_units, received_units
FROM inventory_analytics;
```
📌 用途：促銷商品選擇、清倉規劃 

![image](https://github.com/user-attachments/assets/28ed8a22-955b-42d1-96b1-64cb90d4f507)


### `Order_Conversion_Stats` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| record_id       |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| date           |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| total_visits   |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| total_orders   |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| conversion_rate|  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |

1. 系統管理員（Admin）  
> 完整轉換漏斗數據
```sql
CREATE VIEW admin_conversion_stats_view AS
SELECT * FROM order_conversion_stats;
```
📌 用途：網站效能評估、業務健康度 

![image](https://github.com/user-attachments/assets/e1469d50-7d38-46ca-aeca-517bced1c074)


2. 行銷/營運（Marketing）  
> 訂單轉換率趨勢
```sql
CREATE VIEW marketing_conversion_stats_view AS
SELECT * FROM order_conversion_stats;
```
📌 用途：廣告投放優化、用戶體驗改進 

![image](https://github.com/user-attachments/assets/f503882d-e320-4a27-9843-bb3995987996)


### `Shipment_Performance` 欄位可視權限表
| 欄位                    | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-------------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| record_id               |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |
| date                   |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |
| carrier                |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |
| total_shipments        |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |
| on_time_deliveries     |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |
| late_deliveries        |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |
| average_delivery_time  |  ✔   |   ✘   |    ✘     |     ✔     |    ✘    |     ✔     |    ✘    |

1. 系統管理員（Admin）  
> 物流全維度效能數據
```sql
CREATE VIEW admin_shipment_performance_view AS
SELECT * FROM shipment_performance;
```
📌 用途：物流商評估、配送策略 

![image](https://github.com/user-attachments/assets/f90366a4-5fb6-415a-a122-6e6fd997b91c)


2. 倉儲人員（Warehouse）  
> 物流操作效能指標
```sql
CREATE VIEW warehouse_shipment_performance_view AS
SELECT record_id, date, carrier, total_shipments, on_time_deliveries, late_deliveries, average_delivery_time
FROM shipment_performance;
```
📌 用途：出庫流程優化、物流商協調 

![image](https://github.com/user-attachments/assets/fd470bf4-79e7-49bf-8000-9bfe889a3fd3)


3. 行銷/營運（Marketing）  
> 客戶配送體驗數據
```sql
CREATE VIEW marketing_shipment_performance_view AS
SELECT record_id, date, carrier, total_shipments, on_time_deliveries, late_deliveries
FROM shipment_performance;
```
📌 用途：服務承諾設計、客戶溝通 

![image](https://github.com/user-attachments/assets/41b7d5f9-e3cf-454e-ba2f-2a711d7924e2)


### `Customer_Feedback_Stats` 欄位可視權限表
| 欄位                | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|---------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| record_id           |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✔    |
| date               |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✔    |
| product_id         |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✔    |
| total_reviews      |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✔    |
| average_rating     |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✔    |
| positive_reviews   |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✔    |
| negative_reviews   |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✔    |

1. 系統管理員（Admin）  
> 完整客戶反饋數據
```sql
CREATE VIEW admin_feedback_stats_view AS
SELECT * FROM customer_feedback_stats;
```

📌 用途：服務品質監控、全面改進 

![image](https://github.com/user-attachments/assets/13ff8b18-63fd-4401-b821-a9c16508871c)

2. 賣家（Seller）  
> 商品評價統計數據
```sql
CREATE VIEW seller_feedback_stats_view AS
SELECT record_id, date, product_id, total_reviews, average_rating, positive_reviews, negative_reviews
FROM customer_feedback_stats;
```
📌 用途：商品改進、評價管理 

![image](https://github.com/user-attachments/assets/1a70ea4f-3b3f-4948-a781-95940a61cfba)


3. 行銷/營運（Marketing）  
> 客戶體驗分析數據
```sql
CREATE VIEW marketing_feedback_stats_view AS
SELECT * FROM customer_feedback_stats;
```
📌 用途：品牌形象管理、忠誠度計劃 

![image](https://github.com/user-attachments/assets/af9b3824-b436-44f5-afae-a82368a1236f)


4. 客服人員（Support）  
> 客戶服務相關反饋
```sql
CREATE VIEW support_feedback_stats_view AS
SELECT record_id, date, product_id, total_reviews, average_rating
FROM customer_feedback_stats;
```
📌 用途：服務品質提升、客訴預防 

![image](https://github.com/user-attachments/assets/fa43e874-1506-4a79-97a3-58f31220e63b)


## 商品管理 & 驗證伺服器

### `Products` 欄位可視權限表
| 欄位                | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|---------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| product_id          |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| product_name        |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| sku                 |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✔     |    ✔    |
| brand               |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| model               |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| description         |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| category_id         |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| variant_type        |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| price               |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| promotional_price   |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| promotion_start_date|  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| promotion_end_date  |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| seller_id           |  ✔   |   ✔   |    ✘     |     ✘     |    ✘    |     ✔     |    ✔    |
| shipping_weight     |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✘     |    ✘    |
| image_url           |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| barcode             |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| reviews_count       |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| favorites_count     |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| date_added          |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| last_updated        |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |

1. 系統管理員（Admin）  
> 所有產品完整資訊
```sql
CREATE VIEW admin_products_view AS
SELECT * FROM products;
```
📌 用途：產品全生命周期管理與審計 

![image](https://github.com/user-attachments/assets/708144da-a076-4eb1-aba1-60cc92d3ab5d)


2. 賣家（Seller）  
> 賣家自有產品管理介面
```sql
CREATE VIEW seller_products_view AS
SELECT product_id, product_name, sku, brand, model, description, 
       category_id, variant_type, price, promotional_price, 
       promotion_start_date, promotion_end_date, shipping_weight,
       image_url, barcode, reviews_count, favorites_count,
       date_added, last_updated,seller_id
FROM products
WHERE seller_id = CURRENT_USER_ID();
```
📌 用途：商品上架、價格調整與庫存管理 

![image](https://github.com/user-attachments/assets/aea14a8e-7a00-4048-891e-25275bde20fb)


3. 顧客（Customer）  
> 顧客可見產品資訊
```sql
CREATE VIEW customer_products_view AS
SELECT product_id, product_name, brand, model, description,
       category_id, variant_type, price, promotional_price,
       promotion_start_date, promotion_end_date, image_url,
       reviews_count, favorites_count
FROM products
WHERE (promotion_end_date IS NULL OR promotion_end_date >= NOW());
```
📌 用途：商品瀏覽與購買決策 

![image](https://github.com/user-attachments/assets/7fa0709f-19c5-46cb-b942-46a7d32c7ad2)


4. 倉儲人員（Warehouse）  
> 產品物流相關資訊
```sql
CREATE VIEW warehouse_products_view AS
SELECT product_id, product_name, sku, shipping_weight, 
       barcode, last_updated
FROM products;
```
📌 用途：揀貨、包裝與庫存管理 

![image](https://github.com/user-attachments/assets/a914e5b9-5700-4890-a181-5bf07808118f)


5. 行銷/營運（Marketing）  
> 產品銷售分析數據
```sql
CREATE VIEW marketing_products_view AS
SELECT product_id, product_name, brand, category_id,
       price, promotional_price, reviews_count,
       favorites_count, date_added
FROM products;
```
📌 用途：市場定位與促銷策略制定 

![image](https://github.com/user-attachments/assets/f275e020-2d34-4343-a93f-2be27447b5f7)


6. 客服人員（Support）  
> 產品基本查詢資訊
```sql
CREATE VIEW support_products_view AS
SELECT product_id, product_name, brand, model,
       category_id, price, image_url, seller_id
FROM products;
```
📌 用途：客戶諮詢與爭議處理 

![image](https://github.com/user-attachments/assets/231b08c3-d5e6-4192-8bb0-c8a28a82a274)


### `Reviews` 欄位可視權限表
| 欄位         | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|--------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| review_id    |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| product_id   |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| user_id      |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| rating       |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| comment      |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| created_at   |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |

1. 系統管理員（Admin）  
> 所有產品評論記錄
```sql
CREATE VIEW admin_reviews_view AS
SELECT * FROM reviews;
```
📌 用途：評論內容審核與管理 

![image](https://github.com/user-attachments/assets/78ac2ec8-463b-4ed4-b45b-e66204b04cd6)


2. 賣家（Seller）  
> 賣家產品相關評論
```sql
CREATE VIEW seller_reviews_view AS
SELECT r.review_id, r.product_id, r.rating, r.comment, r.created_at , p.seller_id
FROM reviews r
JOIN products p ON r.product_id = p.product_id
WHERE p.seller_id = CURRENT_USER_ID();
```
📌 用途：產品改進與客戶反饋分析 

![image](https://github.com/user-attachments/assets/700dd17b-7e90-4db0-a2a6-bf982bea7ed8)


3. 顧客（Customer）  
> 所有公開產品評論
```sql
CREATE VIEW customer_reviews_view AS
SELECT review_id, product_id, rating, comment, created_at
FROM reviews;
```
📌 用途：購買決策參考 

![image](https://github.com/user-attachments/assets/228a5ae3-0e17-42a4-9797-6f5cb12b08cb)


4. 行銷/營運（Marketing）  
> 評論統計分析數據
```sql
CREATE VIEW marketing_reviews_view AS
SELECT review_id, product_id, rating, created_at
FROM reviews;
```
📌 用途：產品滿意度評估 

![image](https://github.com/user-attachments/assets/a807d46e-9151-4067-a924-d8a1e200efc1)


5. 客服人員（Support）  
> 評論完整資訊
```sql
CREATE VIEW support_reviews_view AS
SELECT review_id, product_id, user_id, rating, 
       comment, created_at
FROM reviews;
```
📌 用途：不當評論處理與客戶溝通 

![image](https://github.com/user-attachments/assets/dcfd0230-cdf4-4f15-97e5-322f49e68db2)


### `Categories ` 欄位可視權限表
| 欄位                | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|---------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| category_id         |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| category_name       |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| category_description|  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |

1. 系統管理員（Admin）  
> 所有產品分類資訊
```sql
CREATE VIEW admin_categories_view AS
SELECT * FROM categories;
```
📌 用途：分類體系管理與調整 

![image](https://github.com/user-attachments/assets/9f718738-4fcc-46c3-83ba-911dd4d7c2aa)


2. 賣家（Seller）  
> 產品分類結構
```sql
CREATE VIEW seller_categories_view AS
SELECT category_id, category_name, category_description
FROM categories;
```
📌 用途：商品上架分類選擇 

![image](https://github.com/user-attachments/assets/b8775647-ff4d-4582-9cfc-f8c10d54d7b0)


3. 顧客（Customer）  
> 顧客可見分類資訊
```sql
CREATE VIEW customer_categories_view AS
SELECT category_description, category_name
FROM categories;
```
📌 用途：商品分類瀏覽 

![image](https://github.com/user-attachments/assets/b744823e-deee-486e-beb3-fe59638670f4)

4. 行銷/營運（Marketing）  
> 分類完整數據
```sql
CREATE VIEW marketing_categories_view AS
SELECT category_id, category_name, category_description
FROM categories;
```
📌 用途：分類銷售分析與促銷規劃 

![image](https://github.com/user-attachments/assets/d47347e1-3fd1-4a0e-98d9-f7490f9f0513)


### `Login_logs ` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| log_id          |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✘    |
| user_id         |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✘    |
| email           |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✘    |
| login_time      |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✘    |
| ip_address      |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✘    |
| user_agent      |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✘    |
| success         |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✘    |
| failure_reason  |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✘    |

1. 系統管理員（Admin）  
> 所有用戶登入記錄
```sql
CREATE VIEW admin_login_logs_view AS
SELECT * FROM login_logs;
```
📌 用途：安全審計與異常登入監控 

![image](https://github.com/user-attachments/assets/c700ccd5-1dc7-44e9-8f2b-eb451e75e2ab)

2. 客服人員（Support）  
> 用戶登入狀態查詢
```sql
CREATE VIEW support_login_logs_view AS
SELECT log_id, user_id, login_time, success
FROM login_logs;
```
📌 用途：帳號問題排查 

![image](https://github.com/user-attachments/assets/a7569678-1bbd-44a4-8837-c08a497396df)


### `Users ` 欄位可視權限表
| 欄位                  | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| user_id               |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✘    |
| email                 |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✔    |
| password_hash         |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✘    |
| account_enable        |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✔    |
| created_at            |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✔    |
| failed_login_attempts |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✔    |
| is_verified           |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✔    |
| updated_at            |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✔    |

1. 系統管理員（Admin）  
> 所有用戶完整資料
```sql
CREATE VIEW admin_users_view AS
SELECT * FROM users;
```
📌 用途：帳號全權限管理 

![image](https://github.com/user-attachments/assets/a06e5dae-696e-4039-9565-b50ccb7e76b2)


2. 客服人員（Support）  
> 用戶基本服務資訊
```sql
CREATE VIEW support_users_view AS
SELECT user_id, email, account_enable, 
       created_at, is_verified
FROM users;
```
📌 用途：帳號問題排查與處理 

![image](https://github.com/user-attachments/assets/585d0a98-6378-40c0-ae8c-9a4c3d0b1109)


## 訂單管理

### `Orders` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| order_id        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| customer_id     |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| order_status    |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✘     |    ✔    |
| total_amount    |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| created_at      |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| updated_at      |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| shipping_fee    |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✘     |    ✔    |
| discount        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| payment_id      |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| shipping_id     |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✘     |    ✔    |
| coupon_id       |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |

1. 系統管理員（Admin）  
> 所有訂單完整資訊
```sql
CREATE VIEW admin_orders_view AS
SELECT * FROM orders;
```
📌 用途：訂單全生命周期管理與審計 

![image](https://github.com/user-attachments/assets/ab79d1f0-c2c2-46d9-9b6a-918e639175f9)


2. 賣家（Seller）  
> 賣家相關訂單資訊
```sql
CREATE  VIEW seller_orders_view AS
SELECT o.order_id, o.customer_id, o.order_status, 
       o.total_amount, o.created_at, o.updated_at,
       o.shipping_fee, o.discount, o.coupon_id,p.seller_id
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
WHERE p.seller_id = CURRENT_USER_ID()
GROUP BY o.order_id;
```
📌 用途：訂單處理與出貨管理 

![image](https://github.com/user-attachments/assets/591e06d8-7c16-4b5a-9d48-d3fc67b7b8c9)


3. 顧客（Customer）  
> 顧客個人訂單記錄
```sql
CREATE VIEW customer_orders_view AS
SELECT order_id, order_status, total_amount, 
       created_at, updated_at, shipping_fee,
       discount, coupon_id , customer_id
FROM orders
WHERE customer_id = CURRENT_USER_ID();
```
📌 用途：訂單狀態追蹤與歷史查詢 

![image](https://github.com/user-attachments/assets/639b7ca8-234e-4d54-9fd3-ba060a1f6a06)


4. 財務人員（Finance）  
> 訂單財務相關資訊
```sql
CREATE VIEW finance_orders_view AS
SELECT order_id, customer_id, total_amount,
       created_at, shipping_fee, discount,
       payment_id, coupon_id
FROM orders;
```
📌 用途：收入確認與財務報表 

![image](https://github.com/user-attachments/assets/62af9212-efb9-456c-b9c5-9ac84d68fe54)


5. 客服人員（Support）  
> 訂單服務查詢資訊
```sql
CREATE VIEW support_orders_view AS
SELECT order_id, customer_id, order_status,
       total_amount, created_at, updated_at,
       shipping_id, coupon_id
FROM orders;
```
📌 用途：客戶訂單問題處理 

![image](https://github.com/user-attachments/assets/db0c5a4a-cdd0-47ad-8b07-19cbc6614c9e)


### `Order_items` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| order_item_id   |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| order_id        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| product_id      |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| quantity        |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| unit_price      |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |
| subtotal        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |

1. 系統管理員（Admin）  
> 所有訂單商品明細
```sql
CREATE VIEW admin_order_items_view AS
SELECT * FROM order_items;
```
📌 用途：訂單商品全面分析 

![image](https://github.com/user-attachments/assets/659600f7-a30a-4861-a156-7b1e29d0e1f2)


2. 賣家（Seller）  
> 賣家商品訂購明細
```sql
CREATE VIEW seller_order_items_view AS
SELECT oi.order_item_id, oi.order_id, oi.product_id,
       oi.quantity, oi.unit_price, oi.subtotal,p.seller_id
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
WHERE p.seller_id = CURRENT_USER_ID();
```
📌 用途：銷售統計與庫存預測 

![image](https://github.com/user-attachments/assets/a313001f-a44d-4325-bdc8-17ad0aa73cfb)


3. 顧客（Customer）  
> 個人訂單商品明細
```sql
CREATE OR REPLACE VIEW customer_order_items_view AS
SELECT oi.order_item_id, oi.order_id, oi.product_id,
       oi.quantity, oi.unit_price, oi.subtotal,o.customer_id
FROM order_items oi
JOIN orders o ON oi.order_id = o.order_id
WHERE o.customer_id = CURRENT_USER_ID();
```
📌 用途：訂單詳情查看與再次購買 

![image](https://github.com/user-attachments/assets/ad2197f3-bbe7-45b3-904d-7b96d617434a)


4. 財務人員（Finance）  
> 訂單商品財務數據
```sql
CREATE VIEW finance_order_items_view AS
SELECT order_item_id, order_id, product_id,
       quantity, unit_price, subtotal
FROM order_items;
```
📌 用途：收入分攤與成本核算

![image](https://github.com/user-attachments/assets/e6de60f6-8357-457d-99a3-62486d4d0caa)


5. 客服人員（Support）  
> 訂單商品查詢資訊
```sql
CREATE VIEW support_order_items_view AS
SELECT order_item_id, order_id, product_id,
       quantity, unit_price, subtotal
FROM order_items;
```
📌 用途：退換貨與訂單爭議處理 

![image](https://github.com/user-attachments/assets/35436ca4-cfcd-44cd-8bf0-6288b138fe43)


### `Payments` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| payment_id      |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| payment_method  |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| payment_status  |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| amount          |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| payment_date    |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |

1. 系統管理員（Admin）  
> 所有付款交易記錄
```sql
CREATE VIEW admin_payments_view AS
SELECT * FROM payments;
```
📌 用途：支付全流程監控與異常處理 

![image](https://github.com/user-attachments/assets/72e89f8a-999c-43d2-abfd-952b2722a0b4)


2. 顧客（Customer）  
> 個人支付記錄
```sql
CREATE OR REPLACE VIEW customer_payments_view AS
SELECT p.payment_id, p.payment_method,
       p.payment_status, p.amount, p.payment_date,o.customer_id,o.order_id
FROM payments p
JOIN orders o ON p.payment_id = o.payment_id
WHERE o.customer_id = CURRENT_USER_ID();
```
📌 用途：支付憑證查詢與退款申請 

![image](https://github.com/user-attachments/assets/43d69954-2361-44d9-9db9-846382be9a0a)


3. 財務人員（Finance）  
> 完整付款財務資料
```sql
CREATE VIEW finance_payments_view AS
SELECT p.payment_id, p.payment_method,
       p.payment_status, p.amount, p.payment_date , o.order_id
FROM payments p
JOIN orders o ON p.payment_id = o.payment_id;
```
📌 用途：賬務處理與銀行對賬 

![image](https://github.com/user-attachments/assets/4190f2d2-d3a3-422f-95e2-52d72ce314e0)


4. 客服人員（Support）  
> 客戶支付查詢介面
```sql
CREATE VIEW support_payments_view AS
SELECT p.payment_id, o.order_id, p.payment_method,
       p.payment_status, p.amount, p.payment_date
FROM payments p
JOIN orders o ON p.payment_id = o.payment_id;
```
📌 用途：支付問題排查與退款處理 

![image](https://github.com/user-attachments/assets/6c3ad93e-0610-429f-8c77-22310738a52b)


### `Return_refunds` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| refund_id       |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| order_id        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| product_id      |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| reason          |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| status          |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| refund_amount   |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| created_at      |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |

1. 系統管理員（Admin）  
> 所有退貨退款記錄
```sql
CREATE VIEW admin_return_refunds_view AS
SELECT * FROM return_refunds;
```
📌 用途：退貨率分析與流程優化 

![image](https://github.com/user-attachments/assets/14c4e78a-373c-4435-8508-74c92b88cb8b)


2. 賣家（Seller）  
> 賣家商品退貨申請
```sql
CREATE VIEW seller_return_refunds_view AS
SELECT rr.refund_id, rr.order_id, rr.product_id,
       rr.reason, rr.status, rr.refund_amount, rr.created_at
       ,p.seller_id
FROM return_refunds rr
JOIN products p ON rr.product_id = p.product_id
WHERE p.seller_id = CURRENT_USER_ID();
```
📌 用途：退貨審核與庫存恢復 

![image](https://github.com/user-attachments/assets/aa187ceb-6907-472a-9e5d-8479a67b3fb4)


3. 顧客（Customer）  
> 個人退貨退款記錄
```sql
CREATE VIEW customer_return_refunds_view AS
SELECT rr.refund_id, rr.order_id, rr.product_id,
       rr.reason, rr.status, rr.refund_amount, rr.created_at
       ,o.customer_id
FROM return_refunds rr
JOIN orders o ON rr.order_id = o.order_id
WHERE o.customer_id = CURRENT_USER_ID();
```
📌 用途：退貨進度查詢與追蹤 

![image](https://github.com/user-attachments/assets/115176b6-5c2c-4589-abbc-4bd783cf6d76)


5. 財務人員（Finance）  
> 退款財務資料
```sql
CREATE VIEW finance_return_refunds_view AS
SELECT refund_id, order_id, product_id,
       status, refund_amount, created_at
FROM return_refunds;
```
📌 用途：退款處理與賬務調整 

![image](https://github.com/user-attachments/assets/a69609e7-3ce4-49e2-bb0e-80e17c130ffb)


7. 客服人員（Support）  
> 退貨退款全資訊
```sql
CREATE VIEW support_return_refunds_view AS
SELECT refund_id, order_id, product_id,
       reason, status, refund_amount, created_at
FROM return_refunds;
```
📌 用途：退貨流程處理與客戶溝通 

![image](https://github.com/user-attachments/assets/d0cf33be-8bb0-4950-a28e-0eb3521d82a5)

