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

### `Inventory` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| inventory_id    |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| warehouse_id    |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| product_id      |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| stock_quantity  |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| reorder_level   |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| last_updated    |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |

### `Suppliers` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| supplier_id     |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| supplier_name   |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| contact_info    |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| country         |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| rating          |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |
| created_at      |  ✔   |   ✔   |    ✘     |     ✔     |    ✘    |     ✘     |    ✘    |

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

### `Orders` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| order_id        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| customer_name   |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| order_status    |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| total_amount    |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| created_at      |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |

### `Order_Items` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| order_item_id   |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| order_id        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| product_id      |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| quantity        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| price           |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |

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

### `Payments` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| payment_id      |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| order_id        |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| payment_method  |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| payment_status  |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| amount          |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |
| payment_date    |  ✔   |   ✔   |    ✔     |     ✘     |    ✔    |     ✘     |    ✔    |

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

### `Order_Conversion_Stats` 欄位可視權限表
| 欄位            | Admin | Seller | Customer | Warehouse | Finance | Marketing | Support |
|-----------------|:-----:|:------:|:--------:|:---------:|:-------:|:---------:|:-------:|
| record_id       |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| date           |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| total_visits   |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| total_orders   |  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |
| conversion_rate|  ✔   |   ✘   |    ✘     |     ✘     |    ✘    |     ✔     |    ✘    |

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

