![image](https://github.com/user-attachments/assets/031acdec-6a3b-4ebf-b909-6936bb1161c7)
## DataBase Authentication
* Account table
  
| 序號 | 欄位中文名稱     | 欄位名稱    | 資料型態      | 約束條件       | 備註            |
|------|-----------------|------------|---------------|----------------|-----------------|
| 1    | User ID         | user_id    | SERIAL        | PRIMARY KEY    | 自動生成，唯一識別每個用戶 |
| 2    | Account         | account    | VARCHAR(255)  | NOT NULL       | 用戶帳號        |
| 3    | Password        | password   | VARCHAR(255)  | NOT NULL       | 用戶密碼        |
| 4    | Current JWT token | jwt       | VARCHAR(255)  |                | 當前用戶的 JWT 令牌 |


## DataBase Shop
* 商品管理

| 序號 | 欄位中文名稱      | 欄位名稱             | 資料型態      | 約束條件              | 備註                                 |
|------|-------------------|----------------------|---------------|-----------------------|--------------------------------------|
| 1    | 商品ID            | product_id           | SERIAL        | PRIMARY KEY           | 自動生成，唯一識別每個商品             |
| 2    | 產品名稱          | product_name         | VARCHAR(255)  | NOT NULL              | 商品名稱必須填寫                      |
| 3    | SKU (Stock Keeping Unit) | sku                | VARCHAR(100)  | NOT NULL              | 唯一庫存單位代碼                      |
| 4    | 品牌              | brand                | VARCHAR(100)  |                       |                                      |
| 5    | 型號              | model                | VARCHAR(100)  |                       |                                      |
| 6    | 介紹              | description          | TEXT          | NOT NULL              | 商品詳細描述                          |
| 7    | 分類              | category             | VARCHAR(100)  | NOT NULL              | 商品類別                              |
| 8    | 變種              | variant_type         | VARCHAR(100)  | NOT NULL              | 例如顏色、大小等選項                    |
| 9    | 價格              | price                | INT           | NOT NULL              |                                      |
| 10   | 最低價值 ( MAP )  | minimum_advertised_price | INT           | NOT NULL              | 最低廣告價格                          |
| 11   | 促銷價格          | promotional_price    | INT           |                       |                                      |
| 12   | 促銷開始          | promotion_start_date | TIMESTAMP     |                       | 促銷價格開始時間                      |
| 13   | 促銷結束          | promotion_end_date   | TIMESTAMP     |                       | 促銷價格結束時間                      |
| 14   | 庫存數量          | stock_quantity       | INT           | NOT NULL              |                                      |
| 15   | 賣家ID           | seller_id            | VARCHAR(255)  | FOREIGN KEY           | 關聯至賣家資料表                      |
| 16   | 運輸重量          | shipping_weight      | INT           | NOT NULL              |                                      |
| 17   | 圖片URL          | image_url            | VARCHAR(255)  | NOT NULL              | 商品圖片路徑                          |
| 18   | 條碼 (EAN/UPC)   | barcode              | VARCHAR(50)   |                       |                                      |
| 19   | 瀏覽次數         | reviews_count        | INT           | DEFAULT 0             | 商品評論次數                          |
| 20   | 收藏次數         | favorites_count      | INT           | DEFAULT 0             | 商品被收藏次數                        |
| 21   | 加入日期          | date_added           | TIMESTAMP     | DEFAULT CURRENT_TIMESTAMP | 商品加入平台的時間                   |
| 22   | 最後一次更新      | last_updated         | TIMESTAMP     | DEFAULT CURRENT_TIMESTAMP | 商品資訊最後更新的時間               |
| 23   | 狀態              | status               | VARCHAR(50)   |                       | 商品銷售狀態，例如「在售」、「缺貨」等 |

* 評論系統

| 序號 | 欄位中文名稱 | 欄位名稱    | 資料型態   | 約束條件      | 備註         |
|------|-------------|------------|------------|---------------|--------------|
| 29   | 評論ID       | review_id   | SERIAL     | PRIMARY KEY   | 自動生成，唯一識別每個評論 |
| 30   | 商品ID       | product_id  | VARCHAR(255) | FOREIGN KEY | 必須，參照商品表的商品ID  |
| 31   | 用戶ID       | user_id     | VARCHAR(255) | FOREIGN KEY | 必須，參照用戶表的用戶ID  |
| 32   | 評分         | rating      | INT        | NOT NULL     | 用戶對商品的評分       |
| 33   | 評論內容     | comment     | TEXT       | NOT NULL     | 評論的文字內容       |
| 34   | 評論日期     | created_at  | TIMESTAMP  | NOT NULL     | 評論創建的時間       |

*** 

* 倉庫管理

| 序號 | 表名       | 欄位名稱       | 資料型態      | 約束條件                         | 備註                                |
|------|------------|----------------|---------------|-----------------------------------|-------------------------------------|
| 3    | Warehouses | warehouse_id   | INT           | AUTO_INCREMENT PRIMARY KEY        | 自動增長，主鍵                       |
| 4    | Warehouses | warehouse_name | VARCHAR(100)  | NOT NULL                          | 倉庫名稱                            |
| 5    | Warehouses | location       | VARCHAR(255)  | NOT NULL                          | 倉庫位置                            |
| 6    | Warehouses | capacity       | INT           | NOT NULL                          | 倉庫容量                            |
| 7    | Warehouses | manager_id     | INT           |                                   | 倉庫經理的用戶ID                     |
| 8    | Warehouses | contact_info   | VARCHAR(255)  |                                   | 倉庫聯繫信息                        |
| 9    | Warehouses | created_at     | TIMESTAMP     | DEFAULT CURRENT_TIMESTAMP         | 創建時間，預設為當前時間戳            |
| 10   | Warehouses | FOREIGN KEY    | (manager_id)  | REFERENCES Users(user_id)         | 外鍵，參照 Users 表的 user_id 欄位   |

* 庫存管理

| 序號 | 表名      | 欄位名稱        | 資料型態   | 約束條件                                         | 備註                                    |
|------|-----------|----------------|------------|---------------------------------------------------|-----------------------------------------|
| 1    | Inventory | inventory_id   | INT        | AUTO_INCREMENT PRIMARY KEY                        | 自動增長，主鍵                           |
| 2    | Inventory | warehouse_id   | INT        |                                                   | 倉庫ID，參照 Warehouses 表的 warehouse_id |
| 3    | Inventory | product_id     | INT        |                                                   | 產品ID，參照 Products 表的 product_id     |
| 4    | Inventory | stock_quantity | INT        | DEFAULT 0                                         | 初始庫存量設為 0                         |
| 5    | Inventory | reorder_level  | INT        | DEFAULT 10                                        | 最低訂購水平設為 10                      |
| 6    | Inventory | last_updated   | TIMESTAMP  | DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP | 最後更新時間，自動更新                  |
| 7    | Inventory | FOREIGN KEY    | (warehouse_id) | REFERENCES Warehouses(warehouse_id)            | 外鍵，連結到 Warehouses 表                |
| 8    | Inventory | FOREIGN KEY    | (product_id)   | REFERENCES Products(product_id)                | 外鍵，連結到 Products 表                  |

* 供應商管理

| 序號 | 表名      | 欄位名稱       | 資料型態      | 約束條件                             | 備註                          |
|------|-----------|----------------|---------------|---------------------------------------|-------------------------------|
| 1    | Suppliers | supplier_id    | INT           | AUTO_INCREMENT PRIMARY KEY            | 自動增長，主鍵                 |
| 2    | Suppliers | supplier_name  | VARCHAR(100)  | NOT NULL                              | 供應商名稱，必填項目           |
| 3    | Suppliers | contact_info   | VARCHAR(255)  | NOT NULL                              | 供應商聯繫信息，必填項目       |
| 4    | Suppliers | country        | VARCHAR(50)   | NOT NULL                              | 供應商所在國家，必填項目       |
| 5    | Suppliers | rating         | INT           | CHECK (rating BETWEEN 1 AND 5)        | 供應商評分，範圍1到5           |
| 6    | Suppliers | created_at     | TIMESTAMP     | DEFAULT CURRENT_TIMESTAMP             | 創建時間，預設為當前時間戳     |

* 入庫記錄

| 序號 | 表名              | 欄位名稱       | 資料型態      | 約束條件                                 | 備註                                     |
|------|-------------------|----------------|---------------|-------------------------------------------|------------------------------------------|
| 1    | Inbound_Shipments | inbound_id     | INT           | AUTO_INCREMENT PRIMARY KEY                | 自動增長，主鍵                            |
| 2    | Inbound_Shipments | supplier_id    | INT           |                                           | 供應商ID，外鍵參照 Suppliers 表的 supplier_id |
| 3    | Inbound_Shipments | warehouse_id   | INT           |                                           | 倉庫ID，外鍵參照 Warehouses 表的 warehouse_id |
| 4    | Inbound_Shipments | product_id     | INT           |                                           | 產品ID，外鍵參照 Products 表的 product_id    |
| 5    | Inbound_Shipments | quantity       | INT           | NOT NULL                                  | 貨物數量，必填                            |
| 6    | Inbound_Shipments | arrival_date   | DATE          | NOT NULL                                  | 貨物到達日期，必填                        |
| 7    | Inbound_Shipments | status         | ENUM('pending', 'received', 'damaged') | DEFAULT 'pending' | 貨物狀態，預設為 'pending'                 |
| 8    | Inbound_Shipments | FOREIGN KEY    | (supplier_id) | REFERENCES Suppliers(supplier_id)         | 外鍵，連結到 Suppliers 表                 |
| 9    | Inbound_Shipments | FOREIGN KEY    | (warehouse_id)| REFERENCES Warehouses(warehouse_id)       | 外鍵，連結到 Warehouses 表                |
| 10   | Inbound_Shipments | FOREIGN KEY    | (product_id)  | REFERENCES Products(product_id)           | 外鍵，連結到 Products 表                  |

* 出庫記錄

| 序號 | 表名              | 欄位名稱       | 資料型態      | 約束條件                                 | 備註                                     |
|------|-------------------|----------------|---------------|-------------------------------------------|------------------------------------------|
| 1    | Outbound_Shipments | outbound_id   | INT           | AUTO_INCREMENT PRIMARY KEY                | 自動增長，主鍵                            |
| 2    | Outbound_Shipments | order_id      | INT           |                                           | 訂單ID，外鍵參照 Orders 表的 order_id      |
| 3    | Outbound_Shipments | warehouse_id  | INT           |                                           | 倉庫ID，外鍵參照 Warehouses 表的 warehouse_id |
| 4    | Outbound_Shipments | product_id    | INT           |                                           | 產品ID，外鍵參照 Products 表的 product_id    |
| 5    | Outbound_Shipments | quantity      | INT           | NOT NULL                                  | 貨物數量，必填                            |
| 6    | Outbound_Shipments | dispatch_date | DATE          | NOT NULL                                  | 貨物發送日期，必填                        |
| 7    | Outbound_Shipments | status        | ENUM('preparing', 'shipped', 'delivered') | DEFAULT 'preparing' | 貨物狀態，預設為 'preparing'             |
| 8    | Outbound_Shipments | FOREIGN KEY   | (order_id)    | REFERENCES Orders(order_id)               | 外鍵，連結到 Orders 表                    |
| 9    | Outbound_Shipments | FOREIGN KEY   | (warehouse_id)| REFERENCES Warehouses(warehouse_id)       | 外鍵，連結到 Warehouses 表                |
| 10   | Outbound_Shipments | FOREIGN KEY   | (product_id)  | REFERENCES Products(product_id)           | 外鍵，連結到 Products 表                  |

* 訂單商品

| 序號 | 表名   | 欄位名稱       | 資料型態     | 約束條件                         | 備註                                 |
|------|--------|----------------|--------------|-----------------------------------|--------------------------------------|
| 1    | Orders | order_id       | INT          | AUTO_INCREMENT PRIMARY KEY        | 自動增長，主鍵                        |
| 2    | Orders | customer_name  | VARCHAR(100) | NOT NULL                          | 客戶名稱，必填                        |
| 3    | Orders | order_status   | ENUM('pending', 'shipped', 'delivered') | NOT NULL | 訂單狀態，可為 pending, shipped, 或 delivered |
| 4    | Orders | total_amount   | DECIMAL(10,2)| NOT NULL                          | 訂單總金額，必填                      |
| 5    | Orders | created_at     | TIMESTAMP    | DEFAULT CURRENT_TIMESTAMP         | 創建時間，預設為當前時間戳              |

* 物流配送

| 序號 | 表名      | 欄位名稱         | 資料型態      | 約束條件                           | 備註                                           |
|------|-----------|------------------|---------------|-------------------------------------|------------------------------------------------|
| 1    | Shipments | shipment_id      | INT           | AUTO_INCREMENT PRIMARY KEY          | 自動增長，主鍵                                  |
| 2    | Shipments | order_id         | INT           |                                     | 訂單ID，外鍵參照 Orders 表的 order_id            |
| 3    | Shipments | tracking_number  | VARCHAR(50)   | UNIQUE                              | 追蹤號碼，唯一                                  |
| 4    | Shipments | shipment_status  | ENUM('preparing', 'shipped', 'delivered') | NOT NULL  | 出貨狀態，可為 preparing, shipped, 或 delivered |
| 5    | Shipments | estimated_delivery | DATE        |                                     | 預計送達日期                                    |
| 6    | Shipments | actual_delivery  | DATE          |                                     | 實際送達日期                                    |
| 7    | Shipments | carrier          | VARCHAR(50)   |                                     | 運送公司                                        |
| 8    | Shipments | shipping_cost    | DECIMAL(10,2) | DEFAULT 0.00                        | 運費，預設為 0.00                               |
| 9    | Shipments | FOREIGN KEY      | (order_id)    | REFERENCES Orders(order_id)         | 外鍵，連結到 Orders 表                          |

* 倉庫內調撥

| 序號 | 表名                | 欄位名稱           | 資料型態      | 約束條件                                     | 備註                                           |
|------|---------------------|--------------------|---------------|---------------------------------------------|------------------------------------------------|
| 1    | Warehouse_Transfers | transfer_id        | INT           | AUTO_INCREMENT PRIMARY KEY                  | 自動增長，主鍵                                  |
| 2    | Warehouse_Transfers | from_warehouse_id  | INT           |                                             | 從倉庫ID，外鍵參照 Warehouses 表的 warehouse_id  |
| 3    | Warehouse_Transfers | to_warehouse_id    | INT           |                                             | 到倉庫ID，外鍵參照 Warehouses 表的 warehouse_id  |
| 4    | Warehouse_Transfers | product_id         | INT           |                                             | 產品ID，外鍵參照 Products 表的 product_id        |
| 5    | Warehouse_Transfers | quantity           | INT           | NOT NULL                                    | 轉移數量，必填                                  |
| 6    | Warehouse_Transfers | transfer_status    | ENUM('pending', 'in transit', 'completed') | DEFAULT 'pending' | 轉移狀態，預設為 'pending'                       |
| 7    | Warehouse_Transfers | transfer_date      | TIMESTAMP     | DEFAULT CURRENT_TIMESTAMP                   | 轉移日期，預設為當前時間戳                        |
| 8    | Warehouse_Transfers | FOREIGN KEY        | (from_warehouse_id) | REFERENCES Warehouses(warehouse_id)       | 外鍵，連結到 Warehouses 表的 from_warehouse_id   |
| 9    | Warehouse_Transfers | FOREIGN KEY        | (to_warehouse_id)   | REFERENCES Warehouses(warehouse_id)       | 外鍵，連結到 Warehouses 表的 to_warehouse_id     |
| 10   | Warehouse_Transfers | FOREIGN KEY        | (product_id)        | REFERENCES Products(product_id)           | 外鍵，連結到 Products 表的 product_id            |

* 訂單支付

| 序號 | 表名     | 欄位名稱         | 資料型態      | 約束條件                           | 備註                                          |
|------|----------|------------------|---------------|-------------------------------------|-----------------------------------------------|
| 1    | Payments | payment_id       | INT           | AUTO_INCREMENT PRIMARY KEY          | 自動增長，主鍵                                 |
| 2    | Payments | order_id         | INT           |                                     | 訂單ID，外鍵參照 Orders 表的 order_id          |
| 3    | Payments | payment_method   | ENUM('credit_card', 'paypal', 'bank_transfer') | NOT NULL | 支付方式，包括信用卡、PayPal、銀行轉賬          |
| 4    | Payments | payment_status   | ENUM('pending', 'completed', 'failed') | NOT NULL | 支付狀態，可為 pending, completed, 或 failed  |
| 5    | Payments | amount           | DECIMAL(10,2) | NOT NULL                            | 交易金額                                      |
| 6    | Payments | payment_date     | TIMESTAMP     | DEFAULT CURRENT_TIMESTAMP           | 支付日期，預設為當前時間戳                      |
| 7    | Payments | FOREIGN KEY      | (order_id)    | REFERENCES Orders(order_id)         | 外鍵，連結到 Orders 表的 order_id              |

* 退貨與退款

| 序號 | 表名           | 欄位名稱      | 資料型態      | 約束條件                              | 備註                                          |
|------|----------------|---------------|---------------|----------------------------------------|-----------------------------------------------|
| 1    | Returns_Refunds | return_id     | INT           | AUTO_INCREMENT PRIMARY KEY             | 自動增長，主鍵                                 |
| 2    | Returns_Refunds | order_id      | INT           |                                        | 訂單ID，外鍵參照 Orders 表的 order_id          |
| 3    | Returns_Refunds | product_id    | INT           |                                        | 產品ID，外鍵參照 Products 表的 product_id      |
| 4    | Returns_Refunds | reason        | TEXT          |                                        | 退貨原因                                      |
| 5    | Returns_Refunds | status        | ENUM('requested', 'approved', 'rejected') | DEFAULT 'requested' | 退貨狀態，預設為 'requested'                    |
| 6    | Returns_Refunds | refund_amount | DECIMAL(10,2) |                                        | 退款金額                                      |
| 7    | Returns_Refunds | created_at    | TIMESTAMP     | DEFAULT CURRENT_TIMESTAMP              | 創建時間，預設為當前時間戳                      |
| 8    | Returns_Refunds | FOREIGN KEY   | (order_id)    | REFERENCES Orders(order_id)            | 外鍵，連結到 Orders 表的 order_id              |
| 9    | Returns_Refunds | FOREIGN KEY   | (product_id)  | REFERENCES Products(product_id)        | 外鍵，連結到 Products 表的 product_id          |

* 銷售分析

| 序號 | 表名          | 欄位名稱       | 資料型態      | 約束條件                              | 備註                                         |
|------|---------------|----------------|---------------|----------------------------------------|----------------------------------------------|
| 1    | Sales_Analysis | record_id     | INT           | AUTO_INCREMENT PRIMARY KEY             | 自動增長，主鍵                               |
| 2    | Sales_Analysis | date          | DATE          | NOT NULL                               | 銷售日期，必填                               |
| 3    | Sales_Analysis | product_id    | INT           |                                        | 產品ID，外鍵參照 Products 表的 product_id    |
| 4    | Sales_Analysis | category_id   | INT           |                                        | 類別ID，外鍵參照 Categories 表的 category_id |
| 5    | Sales_Analysis | total_sales   | INT           | NOT NULL                               | 總銷售數量，必填                             |
| 6    | Sales_Analysis | revenue       | DECIMAL(10,2) | NOT NULL                               | 總收入，必填                                 |
| 7    | Sales_Analysis | average_price | DECIMAL(10,2) | NOT NULL                               | 平均售價，必填                               |
| 8    | Sales_Analysis | FOREIGN KEY   | (product_id)  | REFERENCES Products(product_id)        | 外鍵，連結到 Products 表的 product_id        |
| 9    | Sales_Analysis | FOREIGN KEY   | (category_id) | REFERENCES Categories(category_id)     | 外鍵，連結到 Categories 表的 category_id     |

