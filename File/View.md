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

2. 賣家（Seller）  
> 了解顧客的會員等級與對應優惠  
```sql
CREATE VIEW seller_membership_levels_view AS
SELECT level_id, level_name, required_points, discount_rate
FROM membership_levels;
```
📌 用途：出貨/行銷依據的會員等級分析

3. 顧客（Customer）  
> 顧客查詢自身等級與升級條件、優惠資訊  
```sql
CREATE VIEW customer_membership_levels_view AS
SELECT level_id, level_name, required_points, discount_rate
FROM membership_levels;
```
📌 用途：顧客等級查詢、升等動機引導

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

6. 行銷/營運（Marketing）  
> 用於活動規劃與會員推升策略分析  
```sql
CREATE VIEW marketing_membership_levels_view AS
SELECT level_id, level_name, required_points, discount_rate, created_at, updated_at
FROM membership_levels;
```
📌 用途：等級制度設計與會員經營分析

7. 客服人員（Support）  
> 需要查詢等級名稱與點數門檻，幫助顧客了解升級條件  
```sql
CREATE VIEW support_membership_levels_view AS
SELECT level_id, level_name, required_points
FROM membership_levels;
```
📌 用途：客服說明「您還差 XX 點就升等」等功能


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


2. 賣家（Seller）
> 用於查看顧客的聯絡方式與收貨資訊，以處理訂單出貨
```sql
CREATE VIEW seller_user_view AS
SELECT user_id, username, email, phone, is_active, shipping_address
FROM users;
```
📌 用途：顧客出貨資訊、會員等級追蹤

3. 顧客（Customer）
> 用於顧客查看個人資料
```sql
CREATE VIEW customer_user_view AS
SELECT username, email, phone, registration_date, membership_level_id, shipping_address, billing_address
FROM users;
```
📌 用途：顧客個資查詢與修改介面

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

6. 行銷/營運（Marketing）
> 分析會員活躍度與聯絡管道
```sql
CREATE VIEW marketing_user_view AS
SELECT user_id, username, email, registration_date, membership_level_id
FROM users;
```
📌 用途：會員成長與行銷策略追蹤

7. 客服人員（Support）
> 查看顧客聯絡方式與會員等級以協助處理問題
```sql
CREATE VIEW support_user_view AS
SELECT user_id, username, email, phone, registration_date, is_active, membership_level_id
FROM users;
```
📌 用途：協助顧客查詢等級、聯絡方式與註冊狀態

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

2. 賣家（Seller）  
> 管理自己商店資訊  
```sql
CREATE VIEW seller_seller_view AS
SELECT seller_id, user_id, store_name, store_description, bank_account, created_at, updated_at
FROM sellers;
```
📌 用途：賣家查詢與維護自己的店家資訊與帳戶

3. 顧客（Customer）  
> 僅顯示店家名稱與簡介，用於商城展示  
```sql
CREATE VIEW customer_seller_view AS
SELECT store_name, store_description
FROM sellers;
```
📌 用途：商城店家頁面展示資訊

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

6. 行銷/營運（Marketing）  
> 分析店家成長與營運時間分布  
```sql
CREATE VIEW marketing_seller_view AS
SELECT seller_id, user_id, store_name, store_description, created_at, updated_at
FROM sellers;
```
📌 用途：商店行銷活動規劃、營運統計與報告

7. 客服人員（Support）  
> 提供店家基本資訊給顧客查詢或處理爭議  
```sql
CREATE VIEW support_seller_view AS
SELECT store_name, store_description
FROM sellers;
```
📌 用途：客服查詢店家資訊以協助顧客

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

3. 顧客（Customer）  
> 個人點數獲取與使用記錄
```sql
CREATE VIEW customer_points_view AS
SELECT point_id, user_id, points_earned, transaction_date, description
FROM points
WHERE user_id = CURRENT_USER();
```
📌 用途：點數餘額查詢與兌換追蹤 

5. 財務人員（Finance）  
> 點數發放與使用記錄
```sql
CREATE VIEW finance_points_view AS
SELECT point_id, user_id, points_earned, transaction_date
FROM points;
```
📌 用途：點數資產負債核算 

7. 客服人員（Support）  
> 客戶點數交易記錄
```sql
CREATE VIEW support_points_view AS
SELECT point_id, user_id, points_earned, transaction_date, description
FROM points;
```
📌 用途：點數問題排查與補償處理 


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

6. 行銷/營運（Marketing）  
> 優惠券活動數據
```sql
CREATE VIEW marketing_coupons_view AS
SELECT coupon_id, coupon_code, discount_amount, expiry_date, 
       usage_limit, is_active, coupon_type, created_at
FROM coupons;
```
📌 用途：促銷活動規劃與效果追蹤 

3. 顧客（Customer）  
> 當前有效優惠券列表
```sql
CREATE VIEW customer_coupons_view AS
SELECT coupon_id, coupon_code, discount_amount, expiry_date
FROM coupons
WHERE is_active = TRUE AND (expiry_date IS NULL OR expiry_date >= CURDATE());
```
📌 用途：優惠券查詢與結帳使用 

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

3. 顧客（Customer）  
> 個人收發訊息記錄
```sql
CREATE VIEW customer_messages_view AS
SELECT message_id, sender_id, receiver_id, message_content, send_time, is_read
FROM messages
WHERE sender_id = CURRENT_USER() OR receiver_id = CURRENT_USER();
```
📌 用途：買賣雙方溝通歷史查詢 

7. 客服人員（Support）  
> 訊息元數據查詢
```sql
CREATE VIEW support_messages_view AS
SELECT message_id, sender_id, receiver_id, send_time, is_read
FROM messages;
```
📌 用途：糾紛調解與通訊異常處理 

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

3. 顧客（Customer）  
> 個人系統通知列表
```sql
CREATE VIEW customer_notifications_view AS
SELECT notification_id, notification_type, subject, 
       message, sent_at, is_read, related_entity, related_entity_id
FROM notifications
WHERE user_id = CURRENT_USER();
```
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

7. 客服人員（Support）  
> 客戶通知記錄
```sql
CREATE VIEW support_notifications_view AS
SELECT notification_id, user_id, notification_type, 
       subject, sent_at, is_read
FROM notifications;
```
📌 用途：通知補發與客戶查詢處理 

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
SELECT * FROM Warehouses;
```
📌 用途：系統後台進行倉庫全面管理與稽核

2. 倉儲人員（Warehouse）
>倉庫基本運營資訊
```spl
CREATE VIEW warehouse_warehouses_view AS
SELECT * FROM Warehouses;
```
📌 用途：日常倉庫管理與維護作業

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
SELECT * FROM Inventory;
```

📌 用途：全局庫存分析與異常監測

2. 賣家（Seller）
>賣家相關商品庫存狀態
```sql
CREATE VIEW seller_inventory_view AS
SELECT inventory_id, warehouse_id, product_id, stock_quantity, reorder_level, last_updated
FROM Inventory;
```

📌 用途：商品備貨與補貨決策依據

3. 倉儲人員（Warehouse）
>實時庫存操作介面
```sql
CREATE VIEW warehouse_inventory_view AS
SELECT * FROM Inventory;
```
📌 用途：每日盤點與庫位管理

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
SELECT * FROM Suppliers;
```
📌 用途：供應鏈全生命周期管理

2. 賣家（Seller）
>合作供應商基本資料
```sql
CREATE VIEW seller_suppliers_view AS
SELECT supplier_id, supplier_name, contact_info, country, rating, created_at
FROM Suppliers;
```

📌 用途：採購決策與供應商評估

3. 倉儲人員（Warehouse）
>物流聯絡所需資訊
```sql
CREATE VIEW warehouse_suppliers_view AS
SELECT supplier_id, supplier_name, contact_info, country, rating
FROM Suppliers;
```

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
SELECT * FROM Inbound_Shipments;
```
📌 用途：入庫流程審計與分析

2. 賣家（Seller）  
> 商品採購入庫狀態
```sql
CREATE VIEW seller_inbound_view AS
SELECT inbound_id, supplier_id, warehouse_id, product_id, quantity, arrival_date, status
FROM Inbound_Shipments;
```

📌 用途：追蹤採購單到貨情況

3. 倉儲人員（Warehouse）  
> 入庫作業明細
```sql
CREATE VIEW warehouse_inbound_view AS
SELECT * FROM Inbound_Shipments;
```

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
SELECT * FROM Outbound_Shipments;
```

📌 用途：出庫異常分析與追溯

2. 賣家（Seller）  
>商品出庫狀態監控
```sql
CREATE VIEW seller_outbound_view AS
SELECT outbound_id, order_id, warehouse_id, product_id, quantity, dispatch_date, status
FROM Outbound_Shipments;
```

📌 用途：訂單履約進度追蹤

3. 倉儲人員（Warehouse）  
>出庫作業單據
```sql
CREATE VIEW warehouse_outbound_view AS
SELECT * FROM Outbound_Shipments;
```
📌 用途：實際揀貨與出庫作業

4. 客服人員（Support）  
>客戶出庫查詢介面
```sql
CREATE VIEW support_outbound_view AS
SELECT outbound_id, order_id, product_id, quantity, dispatch_date, status
FROM Outbound_Shipments;
```

📌 用途：解答客戶物流進度問題

### `Orders` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| order_id        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| customer_name   |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| order_status    |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| total_amount    |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| created_at      |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |

1. 系統管理員（Admin）  
>平台所有訂單數據
```sql
CREATE VIEW admin_orders_view AS
SELECT * FROM Orders;
```

📌 用途：訂單全生命周期管理

2. 賣家（Seller）  
>關聯訂單基本資訊
```sql
CREATE VIEW seller_orders_view AS
SELECT order_id, customer_name, order_status, total_amount, created_at
FROM Orders;
```

📌 用途：訂單處理與客戶溝通

3. 顧客（Customer）  
>個人歷史訂單查詢
```sql
CREATE VIEW customer_orders_view AS
SELECT order_id, customer_name, order_status, total_amount, created_at
FROM Orders
WHERE customer_name = CURRENT_USER();
```

📌 用途：訂單狀態自主追蹤

4. 財務人員（Finance）  
>訂單金額資訊
```sql
CREATE VIEW finance_orders_view AS
SELECT order_id, customer_name, total_amount, created_at
FROM Orders;
```

📌 用途：收入確認與報表生成

5. 客服人員（Support）  
>客戶訂單查詢介面
```sql
CREATE VIEW support_orders_view AS
SELECT order_id, customer_name, order_status, total_amount, created_at
FROM Orders;
```

📌 用途：訂單問題排查與處理

### `Order_Items` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| order_item_id   |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| order_id        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| product_id      |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| quantity        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| price           |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |

1. 系統管理員（Admin）  
>所有訂單商品明細
```sql
CREATE VIEW admin_order_items_view AS
SELECT * FROM Order_Items;
```

📌 用途：商品銷售分析、組合推薦

2. 賣家（Seller）  
>賣家相關訂單商品
```sql
CREATE VIEW seller_order_items_view AS
SELECT order_item_id, order_id, product_id, quantity, price
FROM Order_Items;
```

📌 用途：銷售統計、庫存預測 

3. 顧客（Customer）  
>個人訂購商品明細
```sql
CREATE VIEW customer_order_items_view AS
SELECT oi.order_item_id, oi.order_id, oi.product_id, oi.quantity, oi.price
FROM Order_Items oi
JOIN Orders o ON oi.order_id = o.order_id
WHERE o.customer_name = CURRENT_USER();
```

📌 用途：訂單詳情查看、再次購買 

4. 財務人員（Finance）  
> 訂單商品金額明細
```sql
CREATE VIEW finance_order_items_view AS
SELECT order_item_id, order_id, product_id, quantity, price
FROM Order_Items;
```

📌 用途：收入分攤、成本計算 

5. 客服人員（Support）  
> 訂單商品查詢介面
```sql
CREATE VIEW support_order_items_view AS
SELECT order_item_id, order_id, product_id, quantity, price
FROM Order_Items;
```

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
SELECT * FROM Shipments;
```

📌 用途：物流效能分析、運費結算 

2. 賣家（Seller）  
> 賣家相關物流資訊
```sql
CREATE VIEW seller_shipments_view AS
SELECT shipment_id, order_id, tracking_number, shipment_status, estimated_delivery, actual_delivery, carrier
FROM Shipments;
```

📌 用途：物流進度追蹤、客戶通知 

3. 顧客（Customer）  
> 個人訂單物流狀態
```sql
CREATE VIEW customer_shipments_view AS
SELECT s.shipment_id, s.order_id, s.tracking_number, s.shipment_status, 
       s.estimated_delivery, s.actual_delivery, s.carrier
FROM Shipments s
JOIN Orders o ON s.order_id = o.order_id
WHERE o.customer_name = CURRENT_USER();
```

📌 用途：包裹追蹤、收貨準備 

4. 倉儲人員（Warehouse）  
> 物流出庫關聯資訊
```sql
CREATE VIEW warehouse_shipments_view AS
SELECT shipment_id, order_id, tracking_number, shipment_status, carrier
FROM Shipments;
```

📌 用途：物流交接、運單打印 

5. 客服人員（Support）  
> 客戶物流查詢介面
```sql
CREATE VIEW support_shipments_view AS
SELECT shipment_id, order_id, tracking_number, shipment_status, 
       estimated_delivery, actual_delivery, carrier
FROM Shipments;
```

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
SELECT * FROM Warehouse_Transfers;
```

📌 用途：庫存調度分析、倉庫效能評估 

2. 倉儲人員（Warehouse）  
> 倉庫間調撥作業單
```sql
CREATE VIEW warehouse_transfers_view AS
SELECT * FROM Warehouse_Transfers;
```

📌 用途：庫存調撥執行、在途庫存管理 

### `Payments` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| payment_id      |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| order_id        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| payment_method  |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| payment_status  |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| amount          |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| payment_date    |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |

1. 系統管理員（Admin）  
> 所有支付交易記錄
```sql
CREATE VIEW admin_payments_view AS
SELECT * FROM Payments;
```

📌 用途：支付全流程監控、異常處理 

2. 賣家（Seller）  
> 關聯訂單支付狀態
```sql
CREATE VIEW seller_payments_view AS
SELECT payment_id, order_id, payment_method, payment_status, amount, payment_date
FROM Payments;
```
📌 用途：銷售款項確認、財務對賬 

3. 顧客（Customer）  
> 個人支付記錄
```sql
CREATE VIEW customer_payments_view AS
SELECT p.payment_id, p.order_id, p.payment_method, p.payment_status, p.amount, p.payment_date
FROM Payments p
JOIN Orders o ON p.order_id = o.order_id
WHERE o.customer_name = CURRENT_USER();
```
📌 用途：支付憑證查詢、退款申請 

4. 財務人員（Finance）  
> 完整支付財務資料
```sql
CREATE VIEW finance_payments_view AS
SELECT * FROM Payments;
```
📌 用途：賬務處理、銀行對賬、報表生成 

5. 客服人員（Support）  
> 客戶支付查詢介面
```sql
CREATE VIEW support_payments_view AS
SELECT payment_id, order_id, payment_method, payment_status, amount, payment_date
FROM Payments;
```
📌 用途：支付問題排查、退款處理 

### `Returns_Refunds` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| return_id       |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| order_id        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| product_id      |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| reason         |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| status         |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| refund_amount  |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| created_at     |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |

1. 系統管理員（Admin）  
> 所有退貨退款記錄
```sql
CREATE VIEW admin_returns_view AS
SELECT * FROM Returns_Refunds;
```
📌 用途：退貨率分析、流程優化 

2. 賣家（Seller）  
> 關聯商品退貨申請
```sql
CREATE VIEW seller_returns_view AS
SELECT return_id, order_id, product_id, reason, status, refund_amount, created_at
FROM Returns_Refunds;
```
📌 用途：退貨審核、庫存恢復 

3. 顧客（Customer）  
> 個人退貨退款記錄
```sql
CREATE VIEW customer_returns_view AS
SELECT rr.return_id, rr.order_id, rr.product_id, rr.reason, rr.status, rr.refund_amount, rr.created_at
FROM Returns_Refunds rr
JOIN Orders o ON rr.order_id = o.order_id
WHERE o.customer_name = CURRENT_USER();
```
📌 用途：退貨進度查詢、退款追蹤 

4. 財務人員（Finance）  
> 退款財務資料
```sql
CREATE VIEW finance_returns_view AS
SELECT return_id, order_id, product_id, status, refund_amount, created_at
FROM Returns_Refunds;
```
📌 用途：退款處理、賬務調整 

5. 客服人員（Support）  
> 退貨退款全資訊
```sql
CREATE VIEW support_returns_view AS
SELECT * FROM Returns_Refunds;
```
📌 用途：退貨流程處理、客戶溝通 

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
SELECT * FROM Sales_Analysis;
```
📌 用途：業務決策支持、績效評估 

2. 賣家（Seller）  
> 賣家銷售績效數據
```sql
CREATE VIEW seller_sales_analysis_view AS
SELECT record_id, date, product_id, category_id, total_sales, revenue, average_price
FROM Sales_Analysis;
```
📌 用途：銷售策略調整、商品優化 

3. 行銷/營運（Marketing）  
> 銷售趨勢分析數據
```sql
CREATE VIEW marketing_sales_analysis_view AS
SELECT * FROM Sales_Analysis;
```
📌 用途：促銷活動規劃、市場定位 

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
SELECT * FROM Inventory_Analytics;
```
📌 用途：庫存周轉分析、採購策略 

2. 賣家（Seller）  
> 商品庫存流動趨勢
```sql
CREATE VIEW seller_inventory_analytics_view AS
SELECT record_id, date, warehouse_id, product_id, starting_stock, ending_stock, sold_units, received_units
FROM Inventory_Analytics;
```
📌 用途：庫存健康度監測、補貨預測 

3. 倉儲人員（Warehouse）  
> 庫存操作效能數據
```sql
CREATE VIEW warehouse_inventory_analytics_view AS
SELECT * FROM Inventory_Analytics;
```
📌 用途：倉儲效率優化、作業排程 

4. 行銷/營運（Marketing）  
> 商品流動關聯數據
```sql
CREATE VIEW marketing_inventory_analytics_view AS
SELECT record_id, date, warehouse_id, product_id, sold_units, received_units
FROM Inventory_Analytics;
```
📌 用途：促銷商品選擇、清倉規劃 

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
SELECT * FROM Order_Conversion_Stats;
```
📌 用途：網站效能評估、業務健康度 

2. 行銷/營運（Marketing）  
> 訂單轉換率趨勢
```sql
CREATE VIEW marketing_conversion_stats_view AS
SELECT * FROM Order_Conversion_Stats;
```
📌 用途：廣告投放優化、用戶體驗改進 

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
SELECT * FROM Shipment_Performance;
```
📌 用途：物流商評估、配送策略 

2. 倉儲人員（Warehouse）  
> 物流操作效能指標
```sql
CREATE VIEW warehouse_shipment_performance_view AS
SELECT record_id, date, carrier, total_shipments, on_time_deliveries, late_deliveries, average_delivery_time
FROM Shipment_Performance;
```
📌 用途：出庫流程優化、物流商協調 

3. 行銷/營運（Marketing）  
> 客戶配送體驗數據
```sql
CREATE VIEW marketing_shipment_performance_view AS
SELECT record_id, date, carrier, total_shipments, on_time_deliveries, late_deliveries
FROM Shipment_Performance;
```
📌 用途：服務承諾設計、客戶溝通 

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
SELECT * FROM Customer_Feedback_Stats;
```
📌 用途：服務品質監控、全面改進 

2. 賣家（Seller）  
> 商品評價統計數據
```sql
CREATE VIEW seller_feedback_stats_view AS
SELECT record_id, date, product_id, total_reviews, average_rating, positive_reviews, negative_reviews
FROM Customer_Feedback_Stats;
```
📌 用途：商品改進、評價管理 

3. 行銷/營運（Marketing）  
> 客戶體驗分析數據
```sql
CREATE VIEW marketing_feedback_stats_view AS
SELECT * FROM Customer_Feedback_Stats;
```
📌 用途：品牌形象管理、忠誠度計劃 

4. 客服人員（Support）  
> 客戶服務相關反饋
```sql
CREATE VIEW support_feedback_stats_view AS
SELECT record_id, date, product_id, total_reviews, average_rating
FROM Customer_Feedback_Stats;
```
📌 用途：服務品質提升、客訴預防 



### `products` 欄位可視權限表

| 欄位名稱               | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|------------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| `product_id`           |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| `product_name`         |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| `sku`                  |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✘     |    ✔    |
| `brand`                |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| `model`                |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| `description`          |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `category_id`          |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `variant_type`         |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `price`                |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |
| `promotional_price`    |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |
| `promotion_start_date` |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `promotion_end_date`   |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `seller_id`            |  ✔   |   ✘   |    ✘     |     ✘     |    ✔    |     ✔     |    ✔    |
| `shipping_weight`      |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✘     |    ✘    |
| `image_url`            |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `barcode`              |  ✔   |   ✔   |    ✘     |     ✔     |    ✔    |     ✘     |    ✘    |
| `reviews_count`        |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `favorites_count`      |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `date_added`           |  ✔   |   ✔   |    ✘     |     ✔     |    ✔    |     ✔     |    ✔    |
| `last_updated`         |  ✔   |   ✔   |    ✘     |     ✔     |    ✔    |     ✔     |    ✔    |

1. 系統管理員（Admin）  
> 檢視所有商品詳細資訊以便管理或審查  
```sql
CREATE VIEW admin_product_view AS
SELECT *
FROM products;
```
📌 用途：商品全面監管、資料驗證與系統稽核

2. 賣家（Seller）  
> 管理自己商店的商品庫存、價格、促銷設定  
```sql
CREATE VIEW seller_product_view AS
SELECT product_id, product_name,sku,brand,model,description,
       category_id,variant_type,price,promotional_price,shipping_weight,
       image_url,barcode,reviews_count, favorites_count,date_added,last_updated
FROM products
WHERE seller_id = CURRENT_USER_ID();
```
📌 用途：賣家商品管理與促銷調整（可配合登入身分過濾）

3. 顧客（Customer）  
> 商品展示所需資訊  
```sql
CREATE VIEW customer_product_view AS
SELECT product_id, product_name, brand, model, description, category_id,
       variant_type, price, promotional_price, promotion_start_date,
       promotion_end_date, image_url, reviews_count, favorites_count,
       sku,shipping_weight
FROM products;
```
📌 用途：商城前台商品頁面資訊展示

4. 倉儲人員（Warehouse）  
> 管理實體庫存與包裝需要資訊  
```sql
CREATE VIEW warehouse_product_view AS
SELECT product_id, product_name, sku, brand, model, shipping_weight, barcode, date_added, last_updated
FROM products;
```
📌 用途：出貨包裝、儲位管理與存貨識別

5. 財務人員（Finance）  
> 商品價格與財報相關資訊  
```sql
CREATE VIEW finance_product_view AS
SELECT product_id, sku, price, promotional_price, shipping_weight, seller_id, barcode, date_added, last_updated
FROM products;
```
📌 用途：商品售價、成本與物流財報整合

6. 行銷/營運（Marketing）  
> 用於促銷活動與商品分析  
```sql
CREATE VIEW marketing_product_view AS
SELECT product_id, product_name, brand, model, category_id, variant_type,
       price, promotional_price, promotion_start_date, promotion_end_date,
       image_url, reviews_count, favorites_count, seller_id, date_added, last_updated
FROM products;
```
📌 用途：促銷行銷活動設計與商品熱度追蹤

7. 客服人員（Support）  
> 協助顧客查詢商品資訊或處理退貨相關問題  
```sql
CREATE VIEW support_product_view AS
SELECT product_id, product_name, brand, model, description, category_id,
       variant_type, price, promotional_price, promotion_start_date,
       promotion_end_date, image_url, reviews_count, favorites_count,
       seller_id,sku,date_added,last_updated
FROM products;
```
📌 用途：客服輔助顧客進行商品查詢與說明


### `reviews` 欄位可視權限表

| 欄位名稱      | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|---------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| `review_id`   |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `product_id`  |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `user_id`     |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| `rating`      |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `comment`     |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `created_at`  |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |

1. 系統管理員（Admin）  
> 全面監控評價內容，必要時刪除惡意留言或偽造評論  
```sql
CREATE VIEW admin_reviews_view AS
SELECT *
FROM reviews;
```
📌 用途：管理評價品質與用戶行為審核

2. 賣家（Seller）  
> 查看自家商品的評價內容（⚠️ 請在程式層過濾 seller 自家商品）  
```sql
CREATE VIEW seller_reviews_view AS
SELECT review_id, product_id, user_id, rating, comment, created_at
FROM reviews;
```
📌 用途：追蹤顧客對商品的回饋（需搭配商品歸屬驗證）

3. 顧客（Customer）  
> 查看自己的歷史評論或他人商品評論  
```sql
CREATE VIEW customer_reviews_view AS
SELECT review_id, product_id, user_id, rating, comment, created_at
FROM reviews;
```
📌 用途：查詢/撰寫商品評價，並回顧自己過往留言

4. 倉儲人員（Warehouse）  
> 無需存取顧客評價  
📌 用途：不授權存取商品評論資料

5. 財務人員（Finance）  
> 無需存取顧客評價  
📌 用途：不授權存取商品評論資料

6. 行銷/營運（Marketing）  
> 分析評價分數與評論內容，評估商品/活動效益  
```sql
CREATE VIEW marketing_reviews_view AS
SELECT review_id, product_id, rating, comment, created_at
FROM reviews;
```
📌 用途：回饋收集與評價分數統計、詞頻分析等

7. 客服人員（Support）  
> 查看顧客留言與用戶 ID，便於處理抱怨或負評  
```sql
CREATE VIEW support_reviews_view AS
SELECT review_id, product_id, user_id, rating, comment, created_at
FROM reviews;
```
📌 用途：處理顧客爭議、負評回覆與使用者行為追蹤

### `categories` 欄位可視權限表

| 欄位名稱             | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|----------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| `category_id`        |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| `category_name`      |  ✔   |   ✔   |    ✔     |     ✔     |    ✘    |     ✔     |    ✔    |
| `category_description`| ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |

1. 系統管理員（Admin）  
> 管理商品分類與說明文字，維護分類邏輯與結構  
```sql
CREATE VIEW admin_categories_view AS
SELECT *
FROM categories;
```
📌 用途：商品分類資料完整管理與稽核

2. 賣家（Seller）  
> 查看可用的分類名稱與描述以便上架商品  
```sql
CREATE VIEW seller_categories_view AS
SELECT category_id, category_name, category_description
FROM categories;
```
📌 用途：商品上架時分類選擇參考

3. 顧客（Customer）  
> 查看商城可選的商品分類與說明  
```sql
CREATE VIEW customer_categories_view AS
SELECT category_id, category_name, category_description
FROM categories;
```
📌 用途：商城前台顯示分類資訊與過濾功能

4. 倉儲人員（Warehouse）  
> 僅需基本分類 ID 與名稱以標記實體倉位  
```sql
CREATE VIEW warehouse_categories_view AS
SELECT category_id, category_name
FROM categories;
```
📌 用途：分類編碼與實體分類標籤對應用途

5. 財務人員（Finance）  
> 無分類資訊存取需求  
📌 用途：不授權存取商品分類資料

6. 行銷/營運（Marketing）  
> 用於分類相關活動設計與商品分析統計  
```sql
CREATE VIEW marketing_categories_view AS
SELECT *
FROM categories;
```
📌 用途：促銷分類規劃與分類別商品成效分析

7. 客服人員（Support）  
>   
```sql
CREATE VIEW support_categories_view AS
SELECT category_id, category_name, category_description
FROM categories;
```
📌 用途：客服對應商品分類協助查詢與說明

### `login_logs` 欄位可視權限表

| 欄位名稱         | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| `log_id`         |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| `user_id`        |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| `email`          |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| `login_time`     |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| `ip_address`     |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✔    |
| `user_agent`     |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✔    |
| `success`        |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| `failure_reason` |  ✔   |   ✘   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |

1. 系統管理員（Admin）  
> 查看所有用戶登入紀錄與異常行為以偵測資安風險  
```sql
CREATE VIEW admin_login_logs_view AS
SELECT *
FROM login_logs;
```
📌 用途：資安稽核、帳號活動監控、異常登入偵測

2. 賣家（Seller）  
> 無法查閱 ，透過使用者(customer)查閱
📌 用途：賣家登入追蹤與帳號安全檢視

3. 顧客（Customer）  
> 查看自己的登入紀錄（僅成功與時間）  
```sql
CREATE VIEW customer_login_logs_view AS
SELECT log_id, user_id, email, login_time, success
FROM login_logs
WHERE user_id = CURRENT_USER_ID(); -- ⚠️ 應用層過濾
```
📌 用途：顧客登入活動查詢與自我監控

4. 倉儲人員（Warehouse）  
> 無登入紀錄存取需求  
📌 用途：不授權存取登入行為資料

5. 財務人員（Finance）  
> 無登入紀錄存取需求  
📌 用途：不授權存取登入行為資料

6. 行銷/營運（Marketing）  
> 無登入紀錄存取需求  
📌 用途：不授權存取登入行為資料

7. 客服人員（Support）  
> 協助顧客處理登入問題，查詢失敗原因與時間  
```sql
CREATE VIEW support_login_logs_view AS
SELECT log_id, user_id, email, login_time, ip_address, user_agent, success, failure_reason
FROM login_logs;
```
📌 用途：客服協助查詢登入失敗、帳號遭駭或異常使用情境分析

### `users` 欄位可視權限表（強化安全版）

| 欄位名稱               | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|------------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| `user_id`              |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✘    |
| `email`                |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |
| `password_hash`        |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✘     |    ✘    |
| `account_enable`       |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| `created_at`           |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `failed_login_attempts`|  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| `is_verified`          |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |
| `updated_at`           |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✔     |    ✔    |

1. 系統管理員（Admin）  
> 全面管理所有用戶狀態、帳號安全與權限驗證  
```sql
CREATE VIEW admin_secure_users_view AS
SELECT *
FROM users;
```
📌 用途：帳號控管、安全分析、資料一致性維護

2. 賣家（Seller）  
> 僅可查詢基本資料（不可見密碼與登入失敗）  
```sql
CREATE VIEW seller_secure_users_view AS
SELECT user_id, email, account_enable, created_at, is_verified, updated_at
FROM users
WHERE user_id = CURRENT_USER_ID(); -- ⚠️ 應由應用層過濾自身資料
```
📌 用途：供內部關聯查詢與管理自己帳號狀態

3. 顧客（Customer）  
> 查詢自己帳號的基本註冊與驗證狀態  
```sql
CREATE VIEW seller_secure_users_view AS
SELECT user_id, email, account_enable, created_at, is_verified, updated_at
FROM users;
WHERE user_id = CURRENT_USER_ID(); -- ⚠️ 應由應用層過濾自身資料
```
📌 用途：顧客介面顯示「帳號是否啟用、是否驗證」

4. 倉儲人員（Warehouse）  
> 無用戶存取需求  
📌 用途：不授權存取任何帳號資訊

5. 財務人員（Finance）  
> 僅可查 email 與帳號啟用狀態以處理帳務通知  
```sql
CREATE VIEW finance_secure_users_view AS
SELECT user_id, email, account_enable
FROM users;
```
📌 用途：帳務通知或退款需聯絡時使用

6. 行銷/營運（Marketing）  
> 查詢用戶建立時間與驗證狀況，進行用戶漏斗分析  
```sql
CREATE VIEW marketing_secure_users_view AS
SELECT user_id, email, created_at, is_verified, updated_at
FROM users;
```
📌 用途：行銷分析用戶轉換率與活躍程度

7. 客服人員（Support）  
> 查詢用戶帳號狀態、啟用與登入問題排解  
```sql
CREATE VIEW support_secure_users_view AS
SELECT user_id, email, account_enable, failed_login_attempts, is_verified, created_at, updated_at
FROM users;
```
📌 用途：協助用戶排解帳號鎖定、驗證失敗等問題



### `orders` 欄位可視權限表

| 欄位名稱       | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| `order_id`     |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| `customer_id`  |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |
| `order_status` |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| `total_amount` |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| `created_at`   |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| `updated_at`   |  ✔   |   ✔   |    ✘     |     ✘     |    ✔    |     ✔     |    ✔    |
| `shipping_fee` |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |
| `discount`     |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |
| `payment_id`   |  ✔   |   ✘   |    ✘     |     ✘     |    ✔    |     ✘     |    ✔    |
| `shipping_id`  |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✘     |    ✔    |
| `coupon_id`    |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |

1. 系統管理員（Admin）  
> 完整存取訂單資訊與跨部門管理用於監控、稽核  
```sql
CREATE VIEW admin_orders_view AS
SELECT *
FROM orders;
```
📌 用途：全面訂單稽核、跨部門資料整合分析

2. 賣家（Seller）  
> 查看來自顧客的訂單狀態與出貨需求（⚠️ 以商品對應查詢訂單）  
```sql
CREATE VIEW seller_orders_view AS
SELECT order_id, customer_id, order_status, total_amount, created_at, updated_at,
       shipping_fee, discount, shipping_id, coupon_id
FROM orders;
```
📌 用途：銷售訂單管理與出貨排程參考

3. 顧客（Customer）  
> 查詢個人訂單記錄與付款、運送狀態  
```sql
CREATE VIEW customer_orders_view AS
SELECT order_id, order_status, total_amount, created_at, shipping_fee, discount, shipping_id, coupon_id
FROM orders
WHERE customer_id = CURRENT_USER_ID(); -- ⚠️ 由應用層顧客識別
```
📌 用途：顧客訂單查詢、促銷與運費資訊查閱

4. 倉儲人員（Warehouse）  
> 查詢出貨所需的訂單資訊  
```sql
CREATE VIEW warehouse_orders_view AS
SELECT order_id, order_status, created_at, shipping_id,total_amount
FROM orders;
```
📌 用途：物流出貨排程與配送作業依據

5. 財務人員（Finance）  
> 查詢付款、金額、運費與折扣等資訊  
```sql
CREATE VIEW finance_orders_view AS
SELECT order_id, customer_id, total_amount, shipping_fee, discount, payment_id, created_at, updated_at
FROM orders;
```
📌 用途：財務對帳、收支報表、折扣與運費成本分析

6. 行銷/營運（Marketing）  
> 查詢訂單金額、促銷與優惠券使用狀況  
```sql
CREATE VIEW marketing_orders_view AS
SELECT order_id, customer_id, total_amount, discount, coupon_id, order_status, created_at
FROM orders;
```
📌 用途：行銷活動成效評估與顧客轉換分析

7. 客服人員（Support）  
> 協助查詢顧客訂單進度、付款與配送問題  
```sql
CREATE VIEW support_orders_view AS
SELECT order_id, customer_id, order_status, total_amount, shipping_fee, discount,
       created_at, updated_at, payment_id, shipping_id, coupon_id
FROM orders;
```
📌 用途：客服用於追蹤訂單流程與處理顧客疑問


### `order_items` 欄位可視權限表

| 欄位名稱       | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| `order_item_id`|  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| `order_id`     |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| `product_id`   |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| `quantity`     |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✔     |    ✔    |
| `unit_price`   |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |
| `subtotal`     |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✔     |    ✔    |

1. 系統管理員（Admin）  
> 查看所有訂單品項資訊與金額，確保資料一致性  
```sql
CREATE VIEW admin_order_items_view AS
SELECT *
FROM order_items;
```
📌 用途：品項級訂單監控、數據驗證、疑異偵測

2. 賣家（Seller）  
> 查詢出貨的品項、數量與金額（⚠️ 需配合商品對應過濾 seller）  
```sql
CREATE VIEW seller_order_items_view AS
SELECT order_item_id, order_id, product_id, quantity, unit_price, subtotal
FROM order_items;
```
📌 用途：對應自己商品的訂單明細、出貨處理

3. 顧客（Customer）  
> 查詢自己訂單的商品明細、數量與單價  
```sql
CREATE VIEW customer_order_items_view AS
SELECT order_item_id, order_id, product_id, quantity, unit_price, subtotal
FROM order_items
WHERE order_id IN (
  SELECT order_id FROM orders WHERE customer_id = CURRENT_USER_ID()
);
```
📌 用途：顧客訂單查詢，顯示商品品項與計價明細

4. 倉儲人員（Warehouse）  
> 查詢出貨品項與數量，但不含價格資訊  
```sql
CREATE VIEW warehouse_order_items_view AS
SELECT order_item_id, order_id, product_id, quantity
FROM order_items;
```
📌 用途：根據品項與數量安排出貨與包裝作業

5. 財務人員（Finance）  
> 查詢每筆品項的單價、數量與小計以進行對帳  
```sql
CREATE VIEW finance_order_items_view AS
SELECT order_item_id, order_id, product_id, quantity, unit_price, subtotal
FROM order_items;
```
📌 用途：商品級別銷售金額統計與利潤分析

6. 行銷/營運（Marketing）  
> 分析各品項數量、價格與銷售小計，評估熱門商品  
```sql
CREATE VIEW marketing_order_items_view AS
SELECT order_item_id, order_id, product_id, quantity, unit_price, subtotal
FROM order_items;
```
📌 用途：活動成效分析、商品熱銷排行與推薦系統資料來源

7. 客服人員（Support）  
> 查詢訂單商品明細協助處理顧客問題或退換貨  
```sql
CREATE VIEW support_order_items_view AS
SELECT order_item_id, order_id, product_id, quantity, unit_price, subtotal
FROM order_items;
```
📌 用途：客服查詢顧客下單內容、對應退換貨品項處理


### `payments` 欄位可視權限表

| 欄位名稱         | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|------------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| `payment_id`     |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| `order_id`       |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| `payment_method` |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| `payment_status` |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| `amount`         |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| `payment_date`   |  ✔   |   ✘   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |

1. 系統管理員（Admin）  
> 查看所有訂單的付款狀態與交易資訊  
```sql
CREATE VIEW admin_payments_view AS
SELECT *
FROM payments;
```
📌 用途：付款異常排查、金流整體管理與稽核

2. 賣家（Seller）  
> 無直接金流存取權限（付款由平台統一處理）  
📌 用途：不授權存取付款紀錄（避免敏感資訊外洩）

3. 顧客（Customer）  
> 查詢自己訂單的付款狀態與方式  
```sql
CREATE VIEW customer_payments_view AS
SELECT payment_id, order_id, payment_method, payment_status, amount, payment_date
FROM payments
WHERE order_id IN (
  SELECT order_id FROM orders WHERE customer_id = CURRENT_USER_ID()
);
```
📌 用途：查詢付款是否完成、付款方式與金額確認

4. 倉儲人員（Warehouse）  
> 無付款資訊存取需求  
📌 用途：不授權存取付款資訊

5. 財務人員（Finance）  
> 查詢付款方式、金額與狀態進行帳務對帳  
```sql
CREATE VIEW finance_payments_view AS
SELECT payment_id, order_id, payment_method, payment_status, amount, payment_date
FROM payments;
```
📌 用途：每日/每月金流報表與付款狀態確認

6. 行銷/營運（Marketing）  
> 無需存取付款細節（避免金流資訊外洩）  
📌 用途：不授權存取付款紀錄資料

7. 客服人員（Support）  
> 協助查詢顧客付款狀況，處理付款失敗與爭議問題  
```sql
CREATE VIEW support_payments_view AS
SELECT payment_id, order_id, payment_method, payment_status, amount, payment_date
FROM payments;
```
📌 用途：協助顧客處理付款問題、查詢狀態與申訴


### `return_refunds` 欄位可視權限表

| 欄位名稱        | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| `refund_id`     |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| `order_id`      |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| `product_id`    |  ✔   |   ✔   |    ✔     |     ✔     |    ✔    |     ✘     |    ✔    |
| `reason`        |  ✔   |   ✔   |    ✔     |     ✘     |    ✘    |     ✘     |    ✔    |
| `status`        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| `refund_amount` |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| `created_at`    |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |

1. 系統管理員（Admin）  
> 管理退貨退款申請並監督流程進度  
```sql
CREATE VIEW admin_return_refunds_view AS
SELECT *
FROM return_refunds;
```
📌 用途：稽核退貨退款流程與用戶行為監控

2. 賣家（Seller）  
> 查看顧客退貨請求與狀態以進行審核處理  
```sql
CREATE VIEW seller_return_refunds_view AS
SELECT refund_id, order_id, product_id, reason, status, refund_amount, created_at
FROM return_refunds;
```
📌 用途：賣家接收處理退貨需求與撥款流程

3. 顧客（Customer）  
> 查詢個人退貨紀錄與進度  
```sql
CREATE VIEW customer_return_refunds_view AS
SELECT refund_id, order_id, product_id, reason, status, refund_amount, created_at
FROM return_refunds
WHERE order_id IN (
  SELECT order_id FROM orders WHERE customer_id = CURRENT_USER_ID()
);
```
📌 用途：顧客追蹤退貨申請與退款金額

4. 倉儲人員（Warehouse）  
> 無需存取退貨資料  
📌 用途：不授權存取退貨退款資訊

5. 財務人員（Finance）  
> 查詢退款金額與狀態以安排出款  
```sql
CREATE VIEW finance_return_refunds_view AS
SELECT refund_id, order_id, product_id, refund_amount, status, created_at
FROM return_refunds;
```
📌 用途：確認退款流程與金額資料以安排退款

6. 行銷/營運（Marketing）  
> 無需存取退貨資料（避免洩漏顧客敏感行為）  
📌 用途：不授權存取退貨退款資訊

7. 客服人員（Support）  
> 協助顧客追蹤退貨申請與問題處理  
```sql
CREATE VIEW support_return_refunds_view AS
SELECT refund_id, order_id, product_id, reason, status, refund_amount, created_at
FROM return_refunds;
```
📌 用途：客服處理退貨進度、退款失敗與商品糾紛

