
## DataBase Authentication
* Account table
  


## DataBase Shop
### 物流倉儲

* shipments

| 列名            | 欄位名稱           | 資料型態      | 約束條件                      | 備註                           |
|----------------|--------------------|---------------|-------------------------------|--------------------------------|
| 物流ID          | shipment_id        | SERIAL        | PRIMARY KEY                   | 自動增長，主鍵                  |
| 訂單編號        | order_id           | INTEGER       | REFERENCES orders(order_id)   | 參照 Orders 表的 order_id       |
| 追蹤編號        | tracking_number    | VARCHAR(50)   | UNIQUE NOT NULL               | 唯一，不可為空                  |
| 物流狀態        | shipment_status    | ENUM('preparing', 'shipped', 'delivered') | NOT NULL | 出貨狀態，不可為空            |
| 預計送達日期    | estimated_delivery | DATE          |                               | 預計送達日期                    |
| 實際送達日期    | actual_delivery    | DATE          |                               | 實際送達日期                    |
| 物流承運商      | carrier            | VARCHAR(50)   |                               | 物流承運商名稱                  |
| 運費            | shipping_cost      | NUMERIC(10,2) | DEFAULT 0.00                  | 運費，預設值為 0.00             |

* warehouse_transfers.csv

| 列名              | 欄位名稱         | 資料型態                | 約束條件                          | 備註                          |
|-------------------|------------------|-------------------------|-----------------------------------|-------------------------------|
| 調撥ID            | transfer_id      | SERIAL                  | PRIMARY KEY                       | 自動增長，主鍵                 |
| 來源倉庫ID        | from_warehouse_id| INTEGER                 | REFERENCES warehouses(warehouse_id)| 來源倉庫，外鍵指向倉庫表       |
| 目標倉庫ID        | to_warehouse_id  | INTEGER                 | REFERENCES warehouses(warehouse_id)| 目標倉庫，外鍵指向倉庫表       |
| 產品ID            | product_id       | INTEGER                 | REFERENCES products(product_id)    | 產品ID，外鍵指向產品表         |
| 調撥數量          | quantity         | INTEGER                 | NOT NULL                           | 轉移的產品數量，不可為空       |
| 調撥狀態          | transfer_status  | ENUM('pending', 'in transit', 'completed') | DEFAULT 'pending' | 轉移的狀態，預設為 'pending' |
| 調撥日期          | transfer_date    | TIMESTAMP WITH TIME ZONE | DEFAULT NOW()                      | 轉移的日期，預設為當前時間     |

* Payments

| 列名          | 欄位名稱         | 資料型態                 | 約束條件                        | 備註                              |
|---------------|------------------|--------------------------|---------------------------------|-----------------------------------|
| 付款ID        | payment_id       | SERIAL                   | PRIMARY KEY                     | 自動增長，主鍵                     |
| 訂單編號      | order_id         | INTEGER                  | REFERENCES orders(order_id)     | 訂單ID，外鍵參照訂單表             |
| 付款方式      | payment_method   | ENUM('credit_card', 'paypal', 'bank_transfer') | NOT NULL | 付款方式，不可為空                 |
| 付款狀態      | payment_status   | ENUM('pending', 'completed', 'failed') | NOT NULL | 付款狀態，不可為空                 |
| 付款金額      | amount           | NUMERIC(10,2)            | NOT NULL                        | 付款金額，不可為空                 |
| 付款日期      | payment_date     | TIMESTAMP WITH TIME ZONE | DEFAULT NOW()                   | 付款日期，預設為當前時間            |

* Warehouses

| 列名          | 欄位名稱         | 資料型態                | 約束條件                          | 備註                             |
|---------------|------------------|-------------------------|-----------------------------------|----------------------------------|
| 倉庫ID        | warehouse_id     | SERIAL                  | PRIMARY KEY                       | 自動增長，主鍵                    |
| 倉庫名稱      | warehouse_name   | VARCHAR(100)            | NOT NULL                          | 倉庫名稱，不可為空                |
| 倉庫地址      | location         | VARCHAR(255)            | NOT NULL                          | 倉庫位置，不可為空                |
| 倉庫容量      | capacity         | INTEGER                 | NOT NULL                          | 倉庫容量，不可為空                |
| 管理員ID      | manager_id       | INTEGER                 | REFERENCES users(user_id)         | 管理員ID，外鍵參照用戶表的 user_id |
| 聯絡資訊      | contact_info     | VARCHAR(255)            |                                   | 倉庫聯絡資訊                      |
| 創建時間      | created_at       | TIMESTAMP WITH TIME ZONE| DEFAULT NOW()                     | 倉庫創建時間，預設為當前時間      |

* inventory.csv

| 列名            | 欄位名稱       | 資料型態                | 約束條件                          | 備註                              |
|-----------------|----------------|-------------------------|-----------------------------------|-----------------------------------|
| 庫存ID          | inventory_id   | SERIAL                  | PRIMARY KEY                       | 自動增長，主鍵                     |
| 倉庫ID          | warehouse_id   | INTEGER                 | REFERENCES warehouses(warehouse_id)| 倉庫ID，外鍵參照倉庫表             |
| 產品ID          | product_id     | INTEGER                 | REFERENCES products(product_id)    | 產品ID，外鍵參照產品表             |
| 庫存數量        | stock_quantity | INTEGER                 | DEFAULT 0                         | 庫存數量，預設值為0                |
| 再訂購水平      | reorder_level  | INTEGER                 | DEFAULT 10                        | 再訂購水平，預設值為10             |
| 最後更新時間    | last_updated   | TIMESTAMP WITH TIME ZONE| DEFAULT NOW()                     | 最後更新時間，預設為當前時間       |

* suppliers.csv

| 列名          | 欄位名稱       | 資料型態                | 約束條件                          | 備註                             |
|---------------|----------------|-------------------------|-----------------------------------|----------------------------------|
| 供應商ID      | supplier_id    | SERIAL                  | PRIMARY KEY                       | 自動增長，主鍵                    |
| 供應商名稱    | supplier_name  | VARCHAR(100)            | NOT NULL                          | 供應商名稱，不可為空              |
| 聯絡資訊      | contact_info   | VARCHAR(255)            |                                   | 供應商聯絡信息                    |
| 國家          | country        | VARCHAR(50)             |                                   | 供應商所在國家                    |
| 評級          | rating         | INTEGER                 | CHECK (rating BETWEEN 1 AND 5)    | 供應商評級，介於1至5之間          |
| 創建時間      | created_at     | TIMESTAMP WITH TIME ZONE| DEFAULT NOW()                     | 創建時間，預設為當前時間          |

* inbound_shipments.csv

| 列名          | 欄位名稱       | 資料型態                | 約束條件                          | 備註                              |
|---------------|----------------|-------------------------|-----------------------------------|----------------------------------|
| 入庫ID        | inbound_id     | SERIAL                  | PRIMARY KEY                       | 自動增長，主鍵                    |
| 供應商ID      | supplier_id    | INTEGER                 | REFERENCES suppliers(supplier_id) | 供應商ID，外鍵參照供應商表         |
| 倉庫ID        | warehouse_id   | INTEGER                 | REFERENCES warehouses(warehouse_id)| 倉庫ID，外鍵參照倉庫表             |
| 產品ID        | product_id     | INTEGER                 | REFERENCES products(product_id)    | 產品ID，外鍵參照產品表             |
| 入庫數量      | quantity       | INTEGER                 | NOT NULL                           | 入庫數量，不可為空                |
| 到貨日期      | arrival_date   | DATE                    | NOT NULL                           | 到貨日期，不可為空                |
| 入庫狀態      | status         | ENUM('pending', 'received', 'damaged') | DEFAULT 'pending' | 入庫狀態，預設為 'pending'     |

* outbound_shipments.csv

| 列名          | 欄位名稱       | 資料型態                | 約束條件                          | 備註                              |
|---------------|----------------|-------------------------|-----------------------------------|----------------------------------|
| 出庫ID        | outbound_id    | SERIAL                  | PRIMARY KEY                       | 自動增長，主鍵                    |
| 訂單編號      | order_id       | INTEGER                 | REFERENCES orders(order_id)       | 訂單ID，外鍵參照訂單表             |
| 倉庫ID        | warehouse_id   | INTEGER                 | REFERENCES warehouses(warehouse_id)| 倉庫ID，外鍵參照倉庫表             |
| 產品ID        | product_id     | INTEGER                 | REFERENCES products(product_id)    | 產品ID，外鍵參照產品表             |
| 出庫數量      | quantity       | INTEGER                 | NOT NULL                           | 出庫數量，不可為空                |
| 發貨日期      | dispatch_date  | DATE                    | NOT NULL                           | 發貨日期，不可為空                |
| 出庫狀態      | status         | ENUM('preparing', 'shipped', 'delivered') | DEFAULT 'preparing' | 出庫狀態，預設為 'preparing'     |

* return_refunds.csv

| 列名          | 欄位名稱       | 資料型態                | 約束條件                          | 備註                              |
|---------------|----------------|-------------------------|-----------------------------------|----------------------------------|
| 退款ID        | refund_id      | SERIAL                  | PRIMARY KEY                       | 自動增長，主鍵                    |
| 訂單編號      | order_id       | INTEGER                 | REFERENCES orders(order_id)       | 訂單ID，外鍵參照訂單表             |
| 產品ID        | product_id     | INTEGER                 | REFERENCES products(product_id)   | 產品ID，外鍵參照產品表             |
| 原因/退款原因  | reason         | TEXT                    |                                   | 退款原因                           |
| 退款狀態      | status         | ENUM('requested', 'approved', 'rejected', 'processed') | DEFAULT 'requested' | 退款狀態，預設為 'requested' |
| 退款金額      | refund_amount  | NUMERIC(10,2)           |                                   | 退款金額                           |
| 創建時間      | created_at     | TIMESTAMP WITH TIME ZONE| DEFAULT NOW()                     | 創建時間，預設為當前時間          |

* Sales_Analysis

| 列名          | 欄位名稱       | 資料型態                | 約束條件                          | 備註                              |
|---------------|----------------|-------------------------|-----------------------------------|----------------------------------|
| 銷售紀錄ID    | record_id      | SERIAL                  | PRIMARY KEY                       | 自動增長，主鍵                    |
| 日期          | date           | DATE                    | NOT NULL                          | 銷售數據的日期，不可為空          |
| 產品ID        | product_id     | INTEGER                 | REFERENCES products(product_id)   | 產品ID，外鍵參照產品表            |
| 分類ID        | category_id    | INTEGER                 | REFERENCES categories(category_id)| 分類ID，外鍵參照分類表            |
| 總銷售量      | total_sales    | NUMERIC(10,2)           | NOT NULL                          | 總銷售量，不可為空                |
| 總收入        | revenue        | NUMERIC(10,2)           | NOT NULL                          | 總收入，不可為空                  |
| 平均價格      | average_price  | NUMERIC(10,2)           | NOT NULL                          | 平均銷售價格，不可為空            |

* inventory_analytics.csv

| 列名          | 欄位名稱           | 資料型態                | 約束條件                          | 備註                              |
|---------------|--------------------|-------------------------|-----------------------------------|----------------------------------|
| 紀錄ID        | record_id          | SERIAL                  | PRIMARY KEY                       | 自動增長，主鍵                    |
| 日期          | date               | DATE                    | NOT NULL                          | 分析數據的日期，不可為空          |
| 倉庫ID        | warehouse_id       | INTEGER                 | REFERENCES warehouses(warehouse_id)| 倉庫ID，外鍵參照倉庫表             |
| 產品ID        | product_id         | INTEGER                 | REFERENCES products(product_id)   | 產品ID，外鍵參照產品表             |
| 初始庫存      | starting_stock     | INTEGER                 | NOT NULL                          | 初始庫存，不可為空                |
| 結束庫存      | ending_stock       | INTEGER                 | NOT NULL                          | 結束庫存，不可為空                |
| 銷售數量      | sold_quantity      | INTEGER                 | NOT NULL                          | 該期間內銷售的產品數量，不可為空  |
| 入庫數量      | received_quantity  | INTEGER                 | NOT NULL                          | 該期間內收到的產品數量，不可為空  |

* order_conversion_stats.csv

| 列名              | 欄位名稱         | 資料型態                | 約束條件                          | 備註                              |
|-------------------|------------------|-------------------------|-----------------------------------|----------------------------------|
| 記錄ID            | record_id        | SERIAL                  | PRIMARY KEY                       | 自動增長，主鍵                    |
| 日期              | date             | DATE                    | NOT NULL                          | 統計數據的日期，不可為空          |
| 訪問用戶數/瀏覽    | total_visits     | INTEGER                 | NOT NULL                          | 總訪問次數，不可為空              |
| 總訂單數          | total_orders     | INTEGER                 | NOT NULL                          | 總訂單數，不可為空                |
| 轉換率            | conversion_rate  | NUMERIC(5,2)            | NOT NULL                          | 訂單轉化率，不可為空，保留兩位小數|

* shipment_performance.csv

| 列名              | 欄位名稱             | 資料型態                | 約束條件                          | 備註                              |
|-------------------|----------------------|-------------------------|-----------------------------------|----------------------------------|
| 記錄ID            | record_id            | SERIAL                  | PRIMARY KEY                       | 自動增長，主鍵                    |
| 日期              | date                 | DATE                    | NOT NULL                          | 統計數據的日期，不可為空          |
| 物流承運商        | carrier              | VARCHAR(50)             | NOT NULL                          | 物流承運商名稱，不可為空          |
| 總發貨量          | total_shipments      | INTEGER                 | NOT NULL                          | 總發貨量，不可為空                |
| 準時送達數量      | on_time_deliveries   | INTEGER                 | NOT NULL                          | 準時送達的貨物數量，不可為空      |
| 平均送達時長      | average_delivery_time| NUMERIC(5,2)            | NOT NULL                          | 平均送達時長，不可為空，保留兩位小數 |


* customer_feedback_stats.csv

| 列名          | 欄位名稱       | 資料型態                | 約束條件                          | 備註                              |
|---------------|----------------|-------------------------|-----------------------------------|----------------------------------|
| 產品ID        | product_id     | INTEGER                 | PRIMARY KEY REFERENCES products(product_id) | 主鍵，外鍵參照產品表            |
| 平均評級      | average_rating | NUMERIC(3,2)            | NOT NULL                          | 平均評級，保留兩位小數            |
| 4-5 星評論數  | star_4_5_reviews | INTEGER               | NOT NULL                          | 4至5星評論的數量                 |
| 1-3 星評論數  | star_1_3_reviews | INTEGER               | NOT NULL                          | 1至3星評論的數量                 |








