
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
| 17   | 倉庫地            | stock_location       | VARCHAR(100)  | NOT NULL              | 商品庫存位置                          |
| 18   | 圖片URL          | image_url            | VARCHAR(255)  | NOT NULL              | 商品圖片路徑                          |
| 19   | 條碼 (EAN/UPC)   | barcode              | VARCHAR(50)   |                       |                                      |
| 20   | 瀏覽次數         | reviews_count        | INT           | DEFAULT 0             | 商品評論次數                          |
| 21   | 收藏次數         | favorites_count      | INT           | DEFAULT 0             | 商品被收藏次數                        |
| 22   | 加入日期          | date_added           | TIMESTAMP     | DEFAULT CURRENT_TIMESTAMP | 商品加入平台的時間                   |
| 23   | 最後一次更新      | last_updated         | TIMESTAMP     | DEFAULT CURRENT_TIMESTAMP | 商品資訊最後更新的時間               |
| 24   | 狀態              | status               | VARCHAR(50)   |                       | 商品銷售狀態，例如「在售」、「缺貨」等 |

* 評論系統

| 序號 | 欄位中文名稱 | 欄位名稱    | 資料型態   | 約束條件      | 備註         |
|------|-------------|------------|------------|---------------|--------------|
| 29   | 評論ID       | review_id   | SERIAL     | PRIMARY KEY   | 自動生成，唯一識別每個評論 |
| 30   | 商品ID       | product_id  | VARCHAR(255) | FOREIGN KEY | 必須，參照商品表的商品ID  |
| 31   | 用戶ID       | user_id     | VARCHAR(255) | FOREIGN KEY | 必須，參照用戶表的用戶ID  |
| 32   | 評分         | rating      | INT        | NOT NULL     | 用戶對商品的評分       |
| 33   | 評論內容     | comment     | TEXT       | NOT NULL     | 評論的文字內容       |
| 34   | 評論日期     | created_at  | TIMESTAMP  | NOT NULL     | 評論創建的時間       |

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



