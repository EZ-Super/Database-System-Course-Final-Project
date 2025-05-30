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

## membership_levels

| 欄位名稱         | Admin         | Seller | Customer              | Warehouse | Finance             | Marketing                   | Support                      | 欄位說明                       |
|------------------|---------------|--------|------------------------|-----------|----------------------|-----------------------------|------------------------------|------------------------------|
| `level_id`       | 可見           | ✘      | 可見                  | ✘         | 可見                | 可見                        | 可見                         | 會員等級的唯一識別碼           |
| `level_name`     | 可見           | ✘      | 可見                  | ✘         | 可見                | 可見                        | 可見                         | 會員等級名稱   |
| `required_points`| 可見           | ✘  ✘      | 可見                  | ✘         | ✘                   | 可見                        | 可見                         | 升級至該等級所需累積點數        |
| `discount_rate`  | 可見           | ✘      | 可見（僅前台折扣顯示） | ✘         | 可見                | 可見                        | 可見                            | 該等級對應折扣率               |
| `created_at`     | 可見           | ✘      | ✘                    | ✘         | ✘                   | ✘                           | ✘                            | 會員建立時間                   |
| `updated_at`     | 可見           | ✘      | ✘                     | ✘         | ✘                   | ✘                           | ✘                            | 最後一次修改時間               |


1. 系統管理員（Admin）: 可讀取所有欄位，包含優惠與建立/更新時間

```sql
CREATE VIEW admin_all_membership_levels AS
SELECT *
FROM membership_levels;
```

2. 賣家（Seller）
> 不需要看到會員制度細節，不提供 View

❌ 無需檢視會員等級資訊（該資訊應由平台管理與顧客互動）

3. 顧客（Customer）
> 僅可看到目前等級與折扣（不包含升級條件）
```sql
CREATE VIEW customer_membership_public_view AS
SELECT level_id, level_name, discount_rate
FROM membership_levels;
```

4. 倉儲人員（Warehouse）
> 不需要使用會員等級，不提供 View

❌ 無需接觸顧客與會員制度

5. 財務人員（Finance）
> 僅關注與優惠有關的折扣率（可做對帳、折扣分析）
```sql
CREATE VIEW finance_membership_discount_view AS
SELECT level_id, level_name, discount_rate
FROM membership_levels;
```
📌 用途：用於對帳與財務分析，檢視各級會員折扣結構

6. 行銷／營運（Marketing）
> 可查看等級與門檻（便於設計升級誘因）
```sql
CREATE VIEW marketing_membership_campaign_view AS
SELECT level_id, level_name, required_points, discount_rate
FROM membership_levels;
```
📌 用途：設計行銷活動，決定升等點數門檻與優惠級距
7. 客服人員（Support）
>需要查詢等級名稱與點數門檻，幫助顧客了解升級條件
```sql
CREATE VIEW support_membership_info_view AS
SELECT level_id, level_name, required_points
FROM membership_levels;
```
📌 用途：客服說明「您還差 XX 點就升等」等功能


## users

### 🔍 欄位可視權限表（`users`）

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