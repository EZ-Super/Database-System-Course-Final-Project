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

