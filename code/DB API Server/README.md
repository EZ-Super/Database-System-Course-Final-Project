## TIMESTAMP 支援格式
* `2024-04-10T18:30:00`
* `2024-04-10T00:00:00`

## Username 、商店名稱 支援符號
* `0~9`
* `a~z`
* `A~Z`
* `_,-,@`
* 允許長度 5~20

## mail 支援符號
* `0~9`
* `a~z`
* `A~Z`
* `!#$%&'*+-/=?^_`{|}~"，並且不可為首尾字母`

## 密碼規格
* `0~9`
* `a~z`
* `A~Z`
* `# % $ @`
1.密碼設定長度至少為8個字元的字串。
2.無法使用重複、過於簡單且易於猜測或與帳號相同的字母或數字 (例如：aaa、abc、123...)。
3.必須包含一個特殊字元

## Phone 支援符號
* `0~9`
* 手機號碼開頭不可為 `0` 以外的數字
* 格式 09********

## 地址支援符號
* `0~9`
* 中文字符
* 長度不可超過 `255`

## 折扣率支援符號
* `0~9`
* 數值介於 `1~100`

## 商店描述、描述支援符號
* `0~9`
* `a~z`
* `A~Z`
* `_,-,@`
* emoji
* `!#$%&'*+-/=?^_`{|}~" `
* 長度不可大於 `255`

## 優惠券代碼 支援符號
* `0~9`
* `a~z`
* `A~Z`
* `_,-,@`
* 長度介於 `10 ~ 30`

## 訊息內容 支援符號
* `0~9`
* `a~z`
* `A~Z`
* `_,-,@`
* emoji
* `!#$%&'*+-/=?^_`{|}~" `

## 用戶管理 - 謝延偵

* users

| 列名              | 欄位名稱             | 資料型態                | 約束條件                          | 備註                              |
|----------------------|---------------------|------------------|-------------------------------------|----------------------------------------------------------------------|
| 使用者ID             | user_id             | SERIAL           | PRIMARY KEY                         | 自動生成，唯一識別每個用戶                                          |
| 使用者名稱            | username      | VARCHAR(255)     | NOT NULL, UNIQUE                     | 用戶登入帳號                                   |
| 電子郵件             | email             | VARCHAR(255)     | NOT NULL, UNIQUE                           | 用戶的電子郵件地址                                                  |
| 電話號碼               | phone        |VARCHAR(20)     |                                     | 用戶的聯絡電話                                                      |
| 註冊日期         |registration_date            | TIMESTAMP WITH TIME ZONE     |DEFAULT NOW()            | 用戶建立帳號的時間                                      |
| 是否啟用         |is_active      | BOOLEAN          |DEFAULT TRUE              | 帳戶是否啟用                                                        |
| 會員等級ID     | membership_level_id        | INTEGER          |REFERENCES membership_levels(level_id)      | 用戶所屬的會員等級                         |
| 運送地址      |shipping_address  | VARCHAR(255)     |                                     |預設的商品運送地址                                          |
| 帳單地址         | billing_address          | VARCHAR(255)         |        |  預設的帳單寄送地址                     |

* sellers

| 列名              | 欄位名稱             | 資料型態                | 約束條件                          | 備註                              |
|----------------------|---------------------|------------------|-------------------------------------|----------------------------------------------------------------------|
| 賣家ID |seller_id  | SERIAL                    | PRIMARY KEY                | 自動生成，唯一識別每個賣家                     |
| 使用者ID    |  user_id | INTEGER                   | REFERENCES users(user_id), NOT NULL | 關聯到用戶表的ID |
| 商店名稱     |  store_name | VARCHAR(255)              | NOT NULL                  | 賣家的商店名稱                               |
| 商店描述  | store_description | TEXT                      |                           | 賣家商店的詳細描述                           |
| 銀行帳戶 | bank_account| VARCHAR(255)              |                           | 賣家的銀行帳戶資訊                           |

* membership_levels

| 列名              | 欄位名稱             | 資料型態                | 約束條件                          | 備註                              |
|----------------------|---------------------|------------------|-------------------------------------|----------------------------------------------------------------------|
| 等級ID |level_id | SERIAL       | PRIMARY KEY       | 會員等級的唯一識別碼               |
| 等級名稱 |   level_name | VARCHAR(255) | NOT NULL         | 會員等級的名稱，如：金級、銀級      |
| 所需點數 |    required_points  | INTEGER      | NOT NULL         | 升級到此等級所需的點數             |
| 折扣率 |   discount_rate | NUMERIC(5,2) | NOT NULL         | 會員等級的折扣比率，如：0.9 表示9折 |

* point

| 列名              | 欄位名稱             | 資料型態                | 約束條件                          | 備註                              |
|----------------------|---------------------|------------------|-------------------------------------|----------------------------------------------------------------------|
| 點數ID  |point_id | SERIAL                    | PRIMARY KEY                | 自動生成，唯一識別每筆點數紀錄                 |
| 使用者ID |  user_id| INTEGER                   | REFERENCES users(user_id), NOT NULL | 關聯到用戶表的ID |
| 獲得點數 |  points_earned| INTEGER                   |                           | 用戶獲得或消費的點數量                       |
| 交易日期   |  transaction_date | TIMESTAMP WITH TIME ZONE  | DEFAULT NOW()             | 點數交易發生的時間                           |
| 描述  |   description | VARCHAR(255)              |                           | 點數交易的說明，如：購物得點、活動獎勵         |

- coupons
    - coupon_type_enum AS ENUM ('single_use', 'multi_use_limited');

| 列名              | 欄位名稱             | 資料型態                | 約束條件                          | 備註                              |
|----------------------|---------------------|------------------|-------------------------------------|----------------------------------------------------------------------|
| 優惠券ID | coupon_id  | SERIAL                    | PRIMARY KEY                | 自動生成，唯一識別每張優惠券                   |
| 優惠券代碼|    coupon_code | VARCHAR(255)              | NOT NULL, UNIQUE           | 優惠券的唯一代碼，供用戶輸入使用               |
| 折扣金額 |   discount_amount | NUMERIC(10,2)             | NOT NULL                  | 優惠券提供的折扣金額                         |
| 過期日期   |  expiry_date  | DATE                      |                           | 優惠券的有效期限                             |
| 使用次數限制   |   usage_limit | INTEGER                   |                           | 優惠券可使用的最大次數                       |
| 是否啟用     |   is_active | BOOLEAN                   | DEFAULT TRUE              | 優惠券是否可用                               |
| 優惠券類型    |   coupon_type | coupon_type_enum          | NOT NULL, DEFAULT 'single_use' | 優惠券使用類型 |


* messages

| 列名              | 欄位名稱             | 資料型態                | 約束條件                          | 備註                              |
|----------------------|---------------------|------------------|-------------------------------------|----------------------------------------------------------------------|
| 訊息ID | message_id | SERIAL                    | PRIMARY KEY                | 自動生成，唯一識別每則訊息                     |
| 發送者ID  |    sender_id  | INTEGER                   | REFERENCES users(user_id), NOT NULL | 關聯到發送訊息的用戶ID |
| 接收者ID |    receiver_id | INTEGER                   | REFERENCES users(user_id), NOT NULL | 關聯到接收訊息的用戶ID |
| 訊息內容 |    message_content   | TEXT                      | NOT NULL                  | 訊息的文字內容                               |
| 發送時間|    send_time  | TIMESTAMP WITH TIME ZONE  | DEFAULT NOW()             | 訊息發送的時間                               |
| 是否已讀 |    is_read  | BOOLEAN                   | DEFAULT FALSE             | 訊息是否已經被接收者閱讀                     |

* notifications

| 列名              | 欄位名稱             | 資料型態                | 約束條件                          | 備註                              |
|----------------------|---------------------|------------------|-------------------------------------|----------------------------------------------------------------------|
| 通知ID |notification_id| SERIAL                    | PRIMARY KEY                | 自動生成，唯一識別每則通知                     |
| 使用者ID   |    user_id  | INTEGER                   | REFERENCES users(user_id), NOT NULL | 關聯到接收通知的用戶ID |
| 通知類型|    notification_type | VARCHAR(50)               | NOT NULL                  | 通知的類型，如：訂單狀態、系統公告             |
| 標題  |    subject | VARCHAR(255)              | NOT NULL                  | 通知的標題                                   |
| 內容    |    message| TEXT                      | NOT NULL                  | 通知的詳細內容                               |
| 發送時間|    sent_at | TIMESTAMP WITH TIME ZONE  | DEFAULT NOW()             | 通知發送的時間                               |
| 是否已讀 |    is_read | BOOLEAN                   | DEFAULT FALSE             | 通知是否已經被用戶閱讀                       |
| 相關實體   |   related_entity | VARCHAR(50)               |                           | 通知關聯的實體類型，如：訂單、商品             |
| 相關實體ID |   related_entity_id| INTEGER                   |                           | 通知關聯的實體ID                             |

***

## DataBase Authentication

* AD_server 資料表結構

| 中文欄位名稱         | 欄位名稱            | 資料型態         | 約束條件                            | 備註                                                                 |
|----------------------|---------------------|------------------|-------------------------------------|----------------------------------------------------------------------|
| 使用者ID             | user_id             | SERIAL           | PRIMARY KEY                         | 自動生成，唯一識別每個用戶                                          |
| 帳號 (sAMAccountName)| sAMAccountName      | VARCHAR(255)     | NOT NULL UNIQUE                     | 用戶登入帳號 (AD 備用名稱)                                          |
| 電子郵件             | mail                | VARCHAR(255)     | UNIQUE                              | 用戶的電子郵件地址                                                  |
| 部門                 | department          | VARCHAR(255)     |                                     | 用戶所屬部門                                                        |
| 職稱                 | title               | VARCHAR(255)     |                                     | 用戶的職稱                                                          |
| 啟用帳戶             | accountEnabled      | BOOLEAN          | NOT NULL DEFAULT TRUE               | 帳戶是否啟用                                                        |
| 密碼上次設定時間     | pwdLastSet          | BIGINT           |                                     | 密碼上次變更時間 (NT 時代的 LARGE 格式時間戳)                       |
| 組織單位             | organizationalUnit  | VARCHAR(255)     |                                     | 用戶所屬的組織單位 (OU)                                              |
| 管理員ID             | manager             | INTEGER          | REFERENCES users(user_id)           | 上級主管的使用者ID (外鍵參照 users 表)                              |
| 帳戶建立時間         | whenCreated         | TIMESTAMP WITH TIME ZONE | DEFAULT NOW()                | 建立帳號的時間                                                      |
| 上次修改時間         | whenChanged         | TIMESTAMP WITH TIME ZONE | DEFAULT NOW()                | 上次變更資料的時間                                                  |
| 使用者帳戶控制碼     | userAccountControl  | INTEGER          | DEFAULT 512                         | AD 控制定義碼 (例如 512 代表帳號啟用)                               |
| 當前 JWT             | jwt                 | TEXT             |                                     | 當前登入的 JWT 令牌 (建議長度 TEXT，保留彈性)                       |
| 登陸狀態             | loginStatus         | VARCHAR(50)      | DEFAULT '未登錄'                    | 目前登入狀態，例如：已登入、未登入、鎖定                             |
  
* login_log

| 中文欄位名稱 | 欄位名稱   | 資料型態                    | 約束條件                         | 備註 |
|--------------|------------|-----------------------------|----------------------------------|------|
| 登入ID       | login_id   | SERIAL                      | PRIMARY KEY                      | 自動遞增主鍵 |
| 使用者ID     | user_id    | INTEGER                     | REFERENCES users(user_id)        | 外鍵，對應使用者表 |
| 登入時間     | login_time | TIMESTAMP WITH TIME ZONE    | NOT NULL DEFAULT NOW()           | 登入紀錄時間 |
| IP位址       | ip_address | VARCHAR(45)                 | NOT NULL                        | 登入IP位址|
|登入成功      | success    | Boolean                     | NOT NULL                        | 登入狀態| 
|JWT Token    | jwt         | TEXT                       | NOT NULL                        | JWT|
 

## DataBase Shop

### 商品管理

* products

| 列名               | 欄位名稱           | 資料型態            | 約束條件                                      | 備註                    |
|-------------------|------------------|-------------------|---------------------------------------------|------------------------|
| 產品ID             | product_id       | SERIAL            | PRIMARY KEY                                 | 自動增長，主鍵          |
| 產品名稱           | product_name     | VARCHAR(255)      | NOT NULL                                    | 產品名稱，不可為空      |
| 庫存單位代碼       | sku              | VARCHAR(100)      | NOT NULL, UNIQUE                            | 唯一庫存單位代碼，不可為空 |
| 品牌               | brand            | VARCHAR(100)      |                                             |                         |
| 型號               | model            | VARCHAR(100)      |                                             |                         |
| 介紹               | description      | TEXT              |                                             |                         |
| 分類ID             | category_id      | INTEGER           | REFERENCES categories(category_id) NOT NULL | 分類ID，外鍵參照         |
| 變體型             | variant_type     | VARCHAR(255)      |                                             |                         |
| 價格               | price            | NUMERIC(10,2)     | NOT NULL                                    | 價格，不可為空          |
| 促銷價格           | promotional_price| INTEGER           |                                             |                         |
| 促銷開始日期       | promotion_start_date | TIMESTAMP WITH |                                             | 促銷開始時間            |
| 促銷結束日期       | promotion_end_date   | TIMESTAMP WITH |                                             | 促銷結束時間            |
| 庫存數量           | stock_quantity   | INTEGER           | NOT NULL DEFAULT 0                          | 庫存數量，不可為空，預設為0 |
| 賣家ID             | seller_id        | INTEGER           | REFERENCES sellers(seller_id)               | 賣家ID，外鍵參照         |
| 運輸重量           | shipping_weight  | NUMERIC(10,2)     | DEFAULT 0                                   | 運輸重量，預設為0        |
| 圖片URL            | image_url        | VARCHAR(255)      |                                             | 產品圖片的網址          |
| 條碼               | barcode          | VARCHAR(50)       |                                             | 產品條碼                |
| 瀏覽數量           | reviews_count    | INTEGER           | DEFAULT 0                                   | 評論數量，預設為0        |
| 收藏數量           | favorites_count  | INTEGER           | DEFAULT 0                                   | 收藏數量，預設為0        |
| 加入日期           | date_added       | TIMESTAMP WITH    | DEFAULT NOW()                               | 產品加入日期，預設為當前時間 |
| 最後更新日期       | last_updated     | TIMESTAMP WITH    | DEFAULT NOW()                               | 最後更新日期，預設為當前時間 |

* reviews

| 中文欄位名稱 | 欄位名稱   | 資料型態                  | 約束條件                          | 備註         |
|--------------|------------|---------------------------|-----------------------------------|--------------|
| 評論ID       | review_id  | SERIAL                    | PRIMARY KEY                       |              |
| 商品ID       | product_id | INTEGER                   | REFERENCES products(product_id)   | 關聯商品     |
| 使用者ID     | user_id    | INTEGER                   | REFERENCES users(user_id)         | 關聯使用者   |
| 評分         | rating     | INTEGER                   | NOT NULL                          | 評論分數     |
| 評論內容     | comment    | TEXT                      | NOT NULL                          | 評論文字     |
| 創建時間     | created_at | TIMESTAMP WITH TIME ZONE  | NOT NULL DEFAULT NOW()            | 發表時間戳記 |

* categories

| 中文欄位名稱 | 欄位名稱           | 資料型態       | 約束條件             | 備註     |
|--------------|--------------------|----------------|----------------------|----------|
| 分類ID       | category_id        | SERIAL         | PRIMARY KEY          |          |
| 分類名稱     | category_name      | VARCHAR(255)   | NOT NULL UNIQUE      | 唯一分類 |
| 分類描述     | category_description | TEXT          |                      |          |


***


### 物流倉儲 - 鍾文瑀

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

### 物流倉儲 - 鍾文瑀

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

***


## 商品管理 - 楊峻朋
* products 資料表結構說明

| 欄位名稱             | 資料型別   | 說明                       |
|----------------------|------------|----------------------------|
| product_id           | int        | 商品唯一編號，主鍵        |
| product_name         | string     | 商品名稱，必填            |
| sku                  | string     | 庫存單位，唯一識別碼      |
| brand                | string     | 品牌名稱                  |
| model                | string     | 型號                      |
| description          | string     | 商品描述                  |
| category_id          | int        | 所屬分類 ID，外鍵         |
| variant_type         | string     | 商品變體型態（如顏色、尺寸）|
| price                | decimal    | 商品價格，必填            |
| promotional_price    | decimal    | 促銷價格（可為空）        |
| promotion_start_date | timestamp  | 促銷開始時間              |
| promotion_end_date   | timestamp  | 促銷結束時間              |
| stock_quantity       | int        | 商品庫存數量（預設為 0）  |
| seller_id            | int        | 販售者 ID，外鍵           |
| shipping_weight      | decimal    | 運送重量（預設為 0）      |
| image_url            | string     | 商品圖片 URL              |
| barcode              | string     | 條碼                      |
| reviews_count        | int        | 評論數（預設為 0）        |
| favorites_count      | int        | 收藏數（預設為 0）|



* reviews 資料表結構說明

| 欄位名稱   | 資料型別  | 說明                              |
|------------|-----------|-----------------------------------|
| review_id  | int       | 評論唯一編號，主鍵，自動遞增     |
| product_id | int       | 所屬商品 ID，外鍵                 |
| user_id    | int       | 發表評論的使用者 ID，外鍵        |
| rating     | int       | 評分（通常為 1～5），必填         |
| comment    | string    | 評論內容，必填                    |
| created_at | timestamp | 建立時間（預設為目前時間）        |


* categories 資料表結構說明

| 欄位名稱            | 資料型別 | 說明                           |
|---------------------|----------|--------------------------------|
| category_id         | int      | 分類唯一編號，主鍵，自動遞增 |
| category_name       | string   | 分類名稱，唯一，必填         |
| category_description| string   | 分類描述（可選）             |

## AD - 楊峻朋
* users 資料表說明

| 欄位名稱           | 資料型別 | 說明                                  |
|--------------------|----------|---------------------------------------|
| user_id            | int      | 使用者唯一編號，主鍵，自動遞增       |
| sAMAccountName     | string   | 使用者帳號（AD 帳號），唯一，不可為空 |
| mail               | string   | 使用者電子郵件，唯一，可為空         |
| accountEnabled     | boolean  | 帳號啟用狀態（預設為 `true`）        |
| pwdLastSet         | int      | 密碼最後設定時間（例如 Unix timestamp）|
| whenCreated        | timestamp| 帳號建立時間（預設為現在）           |
| whenChanged        | timestamp| 帳號上次修改時間（預設為現在）       |
| userAccountControl | int      | 使用者帳號控制碼，預設為 `512`       |
| jwt                | string   | JWT 權杖，可為空                      |
| loginStatus        | string   | 登入狀態（預設為 `'未登入'`）        |
| password_hash      | string   | 密碼雜湊，必填                        |
| login_ip           | string   | 最近登入 IP 位址                      |


* login_logs 資料表說明

| 欄位名稱   | 資料型別 | 說明                                |
|------------|----------|-------------------------------------|
| login_id   | int      | 登入紀錄唯一編號，主鍵              |
| user_id    | int      | 對應 `users.user_id` 的外鍵         |
| login_time | timestamp| 登入時間（預設為現在）              |
| ip_address | string   | 登入的 IP 位址                      |
| success    | boolean  | 是否登入成功                        |
| jwt        | string   | 登入使用的 JWT 權杖                 |



