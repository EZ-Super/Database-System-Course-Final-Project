
![shop2 - 複製](https://github.com/user-attachments/assets/9bf450d2-8037-4f5f-ad76-9c0a0e9eb727)


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


# 資料完整性需求

## 登入相關
* users：用於登入的用戶資料
  
| Field 名稱           | Data Type | 唯一 | Not Null | 預設值                    | 格式/限制                            | 說明                     |
|----------------------|-----------|------|----------|---------------------------|-------------------------------------|--------------------------|
| user_id (主鍵)       | INT UNSIGNED       | Y    | Y        | AUTO_INCREMENT            | 正整數                               | 每位使用者的唯一識別碼   |
| email                | STRING    | Y    | Y        | 無                        | 必須為有效 Email 格式                | 使用者註冊用 Email       |
| password_hash        | STRING    | N    | Y        | 無                        | 至少 8 字，含大小寫 + 數字 + 特殊符號（經過 bcrypt） | 雜湊後的使用者密碼       |
| Account Enable       | BOOL      | N    | Y        | True                      | 僅允許 True/False                    | 是否啟用帳號             |
| created_at           | TIMESTAMP | N    | Y        | CURRENT_TIMESTAMP         | 系統自動填入                         | 註冊時間                 |
| failed_login_attempts| INT       | N    | Y        | 0                         | 整數（< 5）                          | 登入失敗次數累計         |
| is_verified          | BOOL      | N    | Y        | 無                        | 僅允許 True/False                    | Email 是否完成驗證       |
| updated_at           | TIMESTAMP | N    | Y        | CURRENT_TIMESTAMP ON UPDATE | 自動更新                         | 資料更新時間             |

| 欄位名稱               |   限制方式                                                  | 理由說明                           |
|------------------------|---------------------------------------------------------------|------------------------------------|
| `email`                | 使用正規表達式驗證 Email 格式，禁止空白與非法字符            | 避免錯誤 Email 或注入              |
| `password_hash`        | 僅接受經 bcrypt 雜湊的密碼字串，不可為明文                    | 防止密碼洩漏與提升安全性            |
| `Account Enable`       | 僅允許布林值（True / False）                                 | 防止非預期類型輸入                  |
| `failed_login_attempts`| 限制最大值（如 < 5），自動鎖定邏輯應實作於應用層              | 防止暴力破解                        |
| `is_verified`          | 僅允許布林值（True / False）                                 | 明確判斷 Email 驗證狀態             |
| `created_at`           | 僅允許系統生成                                                | 防止偽造帳號註冊時間                |
| `updated_at`           | 僅允許系統自動更新                                            | 確保資料異動紀錄一致                |


---

*  login_logs：程式將使用資料庫的整合安全性，因此這些資料不需要存儲在資料庫中。

| Field 名稱           | Data Type     | 唯一 | Not Null | 預設值                | 格式/限制                             | 說明                         |
|----------------------|---------------|------|----------|------------------------|----------------------------------------|------------------------------|
| log_id (主鍵)        | INT UNSIGNED       | Y    | Y        | AUTO_INCREMENT         | 正整數                                 | 每筆登入紀錄的唯一識別碼     |
| user_id      | INT  UNSIGNED         | N    | Y       | 無                    | 外鍵參照 `Users` 表                         | 嘗試登入的使用者     |
| email                 | STRING        | N    | Y       | 無                    | 必須為有效 Email 格式                     | 嘗試登入的帳號（即使錯誤）   |
| login_time           | TIMESTAMP     | N    | Y        | CURRENT_TIMESTAMP      | 時間格式                               | 登入嘗試的發生時間           |
| ip_address           | STRING        | N    | Y        | 無                    | IPv4 或 IPv6 格式                     | 登入來源 IP                  |
| user_agent           | STRING(255)          | N    | N        | 無                    | -                                      | 使用者裝置資訊               |
| success              | BOOL          | N    | Y        | 無                    | 僅允許 True / False                   | 是否成功登入                 |
| failure_reason       | STRING(255)    | N    | N        | 無                    | 最長 255 字                            | 登入失敗原因（如密碼錯誤）   |

| 欄位名稱         |   限制方式                                                | 理由說明                           |
|------------------|-------------------------------------------------------------|------------------------------------|
| `email`          | 驗證合法 Email 格式，拒絕 HTML 特殊字元與 JS 字串           | 防止 XSS 與帳號偽造                |
| `ip_address`     | 僅允許合法 IPv4 / IPv6 格式                                  | 防止非預期輸入與錯誤紀錄           |
| `user_agent`     | 過濾特殊符號，  最大長度限制（255）                       | 防止過長 UA 被用作攻擊向量         |
| `failure_reason` | 限制最大長度、過濾換行與腳本內容                            | 避免將錯誤回報資訊作為 XSS 輸入    |
| `success`        | 僅允許布林值（True / False）                                | 防止邏輯錯誤與資料污染             |


---

## 商品管理

* Product

| 欄位名稱             | 資料型別         | 唯一 | NOT NULL | 預設值                              | 格式/限制                              | 說明                         |
|----------------------|------------------|------|----------|--------------------------------------|----------------------------------------|------------------------------|
| product_id (主鍵)    | INT UNSIGNED  | Y    | Y        | AUTO_INCREMENT                        | 正整數                                 | 商品唯一識別碼               |
| product_name         | STRING(255)      | N    | Y        | 無                                   | 最長 255 字                            | 商品名稱                     |
| sku                  | STRING(100)      | Y    | Y        | 無                                   | 唯一 SKU 編碼                          | 商品庫存單位識別碼           |
| brand                | STRING(100)      | N    | N        | 無                                   | -                                      | 商品品牌                     |
| model                | STRING(100)      | N    | N        | 無                                   | -                                      | 商品型號                     |
| description          | STRING (255)     | N    | N        | 無                                   |   最長 255 字                            | 商品描述                     |
| category_id          | INT UNSIGNED   | N    | Y        | 無                                   | 外鍵參照 `categories` 表              | 商品所屬分類                 |
| variant_type         | STRING(255)      | N    | N        | 無                                   | 例如：尺寸、顏色                       | 商品變體類型                 |
| price                | DECIMAL(10,2)    | N    | Y        | 無                                   | 金額格式（最多 99999999.99）           | 商品原價                     |
| promotional_price    | DECIMAL(10,2)    | N    | N        | 無                                   | 金額格式                               | 特價價格（如有）             |
| promotion_start_date | TIMESTAMP        | N    | N        | 無                                   | -                                      | 特價開始時間                 |
| promotion_end_date   | TIMESTAMP        | N    | N        | 無                                   | -                                      | 特價結束時間                 |
| seller_id            | INT UNSIGNED  | N    | N        | 無                                   | 外鍵參照 `sellers` 表                 | 所屬賣家 ID                  |
| shipping_weight      | DECIMAL(10,2)    | N    | N        | 0                                    | 單位：公斤                             | 運送重量                     |
| image_url            | STRING(255)      | N    | N        | 無                                   | URL 格式                               | 商品主圖網址                 |
| barcode              | STRING(50)       | N    | N        | 無                                   | 可為 EAN / UPC                         | 商品條碼                     |
| favorites_count      | INT              | N    | N        | 0                                    | 正整數                                 | 被加入最愛次數               |
| date_added           | TIMESTAMP        | N    | N        | CURRENT_TIMESTAMP                    | 系統時間                               | 建立時間                     |
| last_updated         | TIMESTAMP        | N    | N        | CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP | 自動更新時間           | 最後更新時間                 |

* 欄位限制

| 欄位名稱         | 限制方式                                                   | 理由說明                                 | 
|------------------|----------------------------------------------------------------|------------------------------------------|
| `product_name`   | 僅允許中英文、數字、空格與基本符號，禁止 `< > " ' / \`         | 預防 XSS 攻擊、UI 錯誤顯示                | 
| `sku`            | 僅允許英數字、破折號、底線（`^[A-Z0-9_-]{1,100}$`）             | 保持一致性、防止資料污染與 SQL 注入       | 
| `brand`          | 限制輸入長度、禁止特殊 HTML 符號                              | 防止 XSS 或亂碼                          | 
| `model`          | 同上                                                          | 同上                                     | 
| `description`    | 可允許部分 HTML，但需進行白名單過濾或完整 HTML escape          | 防止 `<script>` 等惡意代碼               | 
| `variant_type`   | 限制為 Enum 或白名單值（如：Size, Color）                     | 防止使用者任意輸入，資料保持一致          | 
| `image_url`      | 僅允許合法 URL 格式（如 `https://...`），禁止嵌入腳本          | 避免圖片變成 iframe XSS 載體              | 
| `barcode`        | 僅允許數字 / 英數，固定長度（如 EAN-13）                      | 保證掃碼裝置可用、避免輸入亂碼            | 

---

* Category

| 欄位名稱           | 資料型別     | 唯一 | NOT NULL | 預設值           | 格式/限制               | 說明                     |
|--------------------|--------------|------|----------|-------------------|--------------------------|--------------------------|
| category_id        | INT   UNSIGNED     | Y    | Y        | AUTO_INCREMENT    | 正整數                   | 分類唯一識別碼           |
| category_name      | STRING(255)  | Y    | Y        | 無                | 最長 255 字，唯一         | 分類名稱                 |
| category_description | STRING     | N    | N        | 無                | 無長度限制（原為 TEXT）   | 分類描述文字             |

* 欄位限制

| 欄位名稱           | 限制方式                                           | 理由說明                     |
|--------------------|--------------------------------------------------------|------------------------------|
| `category_name `     | 禁止特殊符號如 `<`, `>`, `"`, `'`，僅允許中英文與數字 | 避免 XSS 與資料亂碼           |
| `category_description` | HTML escape 或僅允許特定格式                           | 預防惡意 HTML 注入           |


---

* Review

| 欄位名稱   | 資料型別       | 唯一 | NOT NULL | 預設值              | 格式/限制                  | 說明                     |
|------------|----------------|------|----------|----------------------|----------------------------|--------------------------|
| review_id  | INT  UNSIGNED       | Y    | Y        | AUTO_INCREMENT       | 正整數                     | 每筆評論的唯一識別碼     |
| product_id | INT   UNSIGNED      | N    | N        | 無                   | 外鍵參照 `products` 表     | 被評論的商品 ID          |
| user_id    | INT    UNSIGNED        | N    | N        | 無                   | 外鍵參照 `users` 表        | 發表評論的使用者 ID      |
| rating     | INT            | N    | Y        | 無                   | 限制 1～5              | 評分數值（如星數）        |
| comment    | STRING         | N    | Y        | 無                   | 無長度限制（原為 TEXT）    | 評論內容文字             |
| created_at | TIMESTAMP      | N    | Y        | CURRENT_TIMESTAMP    | 自動記錄建立時間           | 評論建立時間             |

* 欄位限制

| 欄位名稱  | 限制方式                                       | 理由說明                       |
|-----------|----------------------------------------------------|--------------------------------|
| `rating`    | 限制為 1～5 的整數                                  | 避免超出評分邏輯範圍             |
| `comment`   | 禁止 `<`, `>`, `"`, `'`, `/` 等符號或進行 HTML escape | 預防 XSS 攻擊                   |

---

## 用戶管理

* membership_levels

| 欄位名稱       | 資料型別      | 唯一 | NOT NULL | 預設值                                  | 格式/限制                      | 說明                     |
|----------------|---------------|------|----------|------------------------------------------|--------------------------------|--------------------------|
| level_id       | INT   UNSIGNED        | Y    | Y        | AUTO_INCREMENT                           | 正整數                         | 會員等級的唯一識別碼     |
| level_name     | STRING(255)   | N    | Y        | 無                                       | 最長 255 字                    | 會員等級名稱             |
| required_points| INT           | N    | Y        | 無                                       | 非負整數                       | 達到該等級所需的點數     |
| discount_rate  | DECIMAL(5,2)  | N    | Y        | 無                                       | 介於 0.00 ~ 100.00             | 該等級享有的折扣百分比   |
| created_at     | TIMESTAMP     | N    | N        | CURRENT_TIMESTAMP                        | 系統建立時間                   | 等級建立時間             |
| updated_at     | TIMESTAMP     | N    | N        | CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP | 自動更新時間            | 等級資料更新時間         |

* 欄位限制

| 欄位名稱        | 限制方式                                           | 理由說明                       |
|-----------------|--------------------------------------------------------|--------------------------------|
| `level_name`      | 禁止特殊符號如 `<`, `>`, `"`, `'`                      | 避免 XSS、畫面顯示錯誤         |
| `required_points` | 限制為非負整數（如 `>= 0`）                            | 避免錯誤資料影響升級邏輯       |
| `discount_rate`   | 限制為 0.00～100.00 的浮點數                           | 避免過高或負數折扣             |

---

* users : 用戶相關資料

| 欄位名稱             | 資料型別     | 唯一 | NOT NULL | 預設值                | 格式/限制                          | 說明                         |
|----------------------|--------------|------|----------|------------------------|------------------------------------|------------------------------|
| user_id              | INT   UNSIGNED       | Y    | Y        | AUTO_INCREMENT         | 正整數                             | 使用者唯一識別碼             |
| username             | STRING(255)  | Y    | Y        | 無                     | 最長 255 字，唯一，不可為空         | 使用者名稱（登入帳號）       |
| email                | STRING(255)  | Y    | Y        | 無                     | 必須為有效 Email 格式              | 使用者 Email                  |
| phone                | STRING(20)   | N    | N        | 無                     | 手機格式，如 `09xxxxxxxx`          | 使用者電話號碼（可選填）     |
| registration_date    | TIMESTAMP    | N    | N        | CURRENT_TIMESTAMP      | 自動填入                           | 註冊時間                     |
| is_active            | BOOLEAN      | N    | N        | TRUE                   | 僅允許 TRUE / FALSE                | 是否啟用帳號                 |
| membership_level_id  | INT  UNSIGNED        | N    | N        | 無                     | 外鍵參照 `membership_levels` 表    | 對應會員等級                 |
| shipping_address     | STRING(255)  | N    | N        | 無                     | 最長 255 字                        | 收貨地址                     |
| billing_address      | STRING(255)  | N    | N        | 無                     | 最長 255 字                        | 帳單地址                     |

* 欄位限制

| 欄位名稱         | 限制方式                                                   | 理由說明                           |
|------------------|----------------------------------------------------------------|------------------------------------|
| `username`         | 僅允許中英文、數字、底線（禁止 `< > " ' /`）                    | 避免 XSS，確保帳號格式一致         |
| `email`            | 必須為合法 Email 格式                                           | 避免垃圾帳號或非通知用 Email       |
| `phone`            | 格式如 `^09\d{8}$` 或國際電話格式驗證                          | 確保電話格式正確                   |
| `shipping_address` | 禁止 `<`, `>`, `"` 等特殊符號，或使用輸出轉義                  | 避免前端渲染誤判、XSS              |
| `billing_address`  | 同上                                                           | 同上                               |

---

* sellers
  
| 欄位名稱         | 資料型別     | 唯一 | NOT NULL | 預設值                                  | 格式/限制                    | 說明                         |
|------------------|--------------|------|----------|------------------------------------------|------------------------------|------------------------------|
| seller_id        | INT  UNSIGNED        | Y    | Y        | AUTO_INCREMENT                           | 正整數                       | 賣家唯一識別碼               |
| user_id          | INT   UNSIGNED       | Y    | Y        | 無                                       | 外鍵參照 `users` 表          | 對應使用者帳號（1 對 1）     |
| store_name       | STRING(255)  | N    | Y        | 無                                       | 最長 255 字                  | 商店名稱                     |
| store_description| STRING       | N    | N        | 無                                       | 無長度限制（原為 TEXT）       | 商店描述                     |
| bank_account     | STRING(255)  | N    | N        | 無                                       | 字串（格式驗證）          | 收款帳戶資訊                 |
| created_at       | TIMESTAMP    | N    | N        | CURRENT_TIMESTAMP                        | 自動填入                     | 商店建立時間                 |
| updated_at       | TIMESTAMP    | N    | N        | CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP | 自動更新時間          | 最後更新時間                 |

| 欄位名稱         | 限制方式                                                   | 理由說明                         |
|------------------|----------------------------------------------------------------|----------------------------------|
| `store_name`       | 禁止特殊符號 `<`, `>`, `"`, `'`, `/`，僅允許中英數與基本符號     | 避免 XSS 與顯示錯誤               |
| `store_description`| 須 HTML escape 或白名單格式過濾                                  | 避免 `<script>` 等攻擊內容注入   |
| `bank_account`     | 限制為純數字或銀行帳號格式（例：`XXX-XXXXXXXXX`）               | 避免亂碼或惡意內容寫入            |

---

* points

| 欄位名稱         | 資料型別     | 唯一 | NOT NULL | 預設值                | 格式/限制                      | 說明                           |
|------------------|--------------|------|----------|------------------------|--------------------------------|--------------------------------|
| point_id         | INT   UNSIGNED       | Y    | Y        | AUTO_INCREMENT         | 正整數                         | 每筆點數紀錄的唯一識別碼       |
| user_id          | INT    UNSIGNED      | N    | Y        | 無                     | 外鍵參照 `users` 表            | 對應的使用者 ID                |
| points_earned    | INT          | N    | N        | 無                     | 限制為整數，可為負值（扣點） | 當次增加或扣除的點數           |
| transaction_date | TIMESTAMP    | N    | N        | CURRENT_TIMESTAMP      | 自動填入                       | 點數交易發生的時間             |
| description      | STRING(255)  | N    | N        | 無                     | 最長 255 字                    | 點數來源或用途說明             |

| 欄位名稱      | 限制方式                                           | 理由說明                         |
|---------------|--------------------------------------------------------|----------------------------------|
| `points_earned` | 限制為整數（可允許負數），不允許小數                   | 避免計算誤差與無效點數邏輯       |
| `description`   | 禁止 `<`, `>`, `"`, `'` 等特殊符號，或進行 HTML escape | 避免 XSS 攻擊與錯誤顯示          |

---

* coupon

| 欄位名稱        | 資料型別       | 唯一 | NOT NULL | 預設值           | 格式/限制                                       | 說明                           |
|-----------------|----------------|------|----------|------------------|------------------------------------------------|--------------------------------|
| coupon_id       | INT  UNSIGNED          | Y    | Y        | AUTO_INCREMENT   | 正整數                                           | 每張優惠券的唯一識別碼         |
| coupon_code     | STRING(255)    | Y    | Y        | 無               | 最長 255 字，唯一                               | 優惠券代碼                     |
| discount_amount | DECIMAL(10,2)  | N    | Y        | 無               | 正數，  `>= 0.00`                            | 折扣金額                       |
| expiry_date     | DATE           | N    | N        | 無               | 合法日期格式                                     | 優惠券到期日                   |
| usage_limit     | INT            | N    | N        | 無               | 整數，  `>= 0`                               | 可使用次數上限（NULL = 無限制）|
| is_active       | BOOLEAN        | N    | N        | TRUE             | 僅允許 TRUE / FALSE                             | 優惠券是否啟用                 |
| coupon_type     | ENUM           | N    | Y        | `'single_use'`   | 僅限 `'single_use'` 或 `'multi_use_limited'`    | 優惠券類型                     |
| created_at      | TIMESTAMP      | N    | N        | CURRENT_TIMESTAMP| 系統時間                                         | 建立時間                       |

| 欄位名稱          | 限制方式                                                  | 理由說明                         |
|-------------------|---------------------------------------------------------------|----------------------------------|
| `coupon_code`     | 僅允許英數與基本符號（ 正規表示式）                        | 防止 XSS 與代碼格式錯誤          |
| `discount_amount` | 限制為大於等於 0 的浮點數                                     | 避免負數造成加價邏輯錯誤         |
| `expiry_date`     | 限制為合法日期格式， 不得為過去時間（應用層邏輯）          | 保證活動邏輯正確                 |
| `usage_limit`     | 限制為整數 `>= 0`                                              | 避免負數導致邏輯錯亂             |
| `coupon_type`     | 限制為 ENUM 白名單值                                           | 避免注入與資料錯誤               |

---

* message

| 欄位名稱          | 資料型別   | 唯一 | NOT NULL | 預設值                | 格式/限制                     | 說明                         |
|-------------------|------------|------|----------|------------------------|-------------------------------|------------------------------|
| message_id        | INT UNSIGNED       | Y    | Y        | AUTO_INCREMENT         | 正整數                        | 訊息唯一識別碼               |
| sender_id         | INT  UNSIGNED      | N    | Y        | 無                     | 外鍵參照 `users(user_id)`     | 發送者 ID                    |
| receiver_id       | INT  UNSIGNED      | N    | Y        | 無                     | 外鍵參照 `users(user_id)`     | 接收者 ID                    |
| message_content   | STRING     | N    | Y        | 無                     | 無長度限制（原為 TEXT）       | 訊息內容                     |
| send_time         | TIMESTAMP  | N    | N        | CURRENT_TIMESTAMP      | 自動填入時間                  | 訊息傳送時間                 |
| is_read           | BOOLEAN    | N    | N        | FALSE                  | 僅允許 TRUE / FALSE           | 是否已讀                     |

| 欄位名稱          | 限制方式                                                  | 理由說明                         |
|-------------------|---------------------------------------------------------------|----------------------------------|
| `message_content` | 僅允許文字或部分格式內容，需進行 HTML escape 或白名單過濾     | 預防 XSS（如 <script> 注入）     |

---

* notifications

| 欄位名稱             | 資料型別     | 唯一 | NOT NULL | 預設值                | 格式/限制                        | 說明                               |
|----------------------|--------------|------|----------|------------------------|-----------------------------------|------------------------------------|
| notification_id      | INT  UNSIGNED        | Y    | Y        | AUTO_INCREMENT         | 正整數                            | 通知唯一識別碼                     |
| user_id              | INT  UNSIGNED        | N    | Y        | 無                     | 外鍵參照 `users(user_id)`         | 接收通知的使用者 ID                |
| notification_type    | STRING(50)   | N    | Y        | 無                     | 最長 50 字                        | 通知類型（如 order, promo, system）|
| subject              | STRING(255)  | N    | Y        | 無                     | 最長 255 字                       | 通知主題                           |
| message              | STRING       | N    | Y        | 無                     | 無長度限制（原為 TEXT）           | 通知內容                           |
| sent_at              | TIMESTAMP    | N    | N        | CURRENT_TIMESTAMP      | 系統自動填入                      | 通知發送時間                       |
| is_read              | BOOLEAN      | N    | N        | FALSE                  | TRUE / FALSE                      | 是否已讀                           |
| related_entity       | STRING(50)   | N    | N        | 無                     | 例如 'order', 'product', 'coupon' | 關聯實體名稱（模組）               |
| related_entity_id    | INT  UNSIGNED        | N    | N        | 無                     | 整數                              | 關聯實體的 ID                      |

| 欄位名稱            | 限制方式                                                   | 理由說明                         |
|---------------------|----------------------------------------------------------------|----------------------------------|
| `notification_type` | 僅允許英文、小寫字串， 使用 ENUM 或白名單                  | 避免錯誤值與資料不一致           |
| `subject`           | 禁止 `<`, `>`, `"`, `'`，可使用正規表示式限制內容格式           | 避免 XSS 與顯示錯誤              |
| `message`           |   HTML escape 或白名單過濾                                  | 避免 `<script>` 等惡意注入       |
| `related_entity`    |  使用 ENUM 或固定白名單值                                   | 避免模組名稱錯誤或注入           |


## 物流倉儲

* warehouses 

| 欄位名稱         | 資料型別    | 唯一 | NOT NULL | 預設值           | 格式/限制                       | 說明                         |
|------------------|-------------|------|----------|------------------|----------------------------------|------------------------------|
| `warehouse_id`   | INT  UNSIGNED         | Y    | Y        | AUTO_INCREMENT   |正整數                            | 倉庫唯一識別碼               |
| `warehouse_name` | STRING(100) | N    | Y        | 無               | 最長 100 字                     | 倉庫名稱                     |
| `location`       | STRING(255) | N    | Y        | 無               | 最長 255 字                     | 倉庫地址 / 位置              |
| `capacity`       | INT         | N    | Y        | 無               |  限制 ≥ 0                    | 倉儲容量                     |
| `manager_id`     | INT UNSIGNED | N    | N        | 無               | 外鍵參照 `Users(user_id)`       | 倉庫負責人 ID（UUID 格式）    |
| `contact_info`   | STRING(255) | N    | N        | 無               | 最長 255 字                     | 聯絡方式（電話/Email）       |
| `created_at`     | TIMESTAMP   | N    | N        | CURRENT_TIMESTAMP | 系統時間                        | 建立時間                     |

| 欄位名稱          |  限制方式                                                    | 理由說明                         |
|-------------------|------------------------------------------------------------------|----------------------------------|
| `warehouse_name`  | 禁止 `<`, `>`, `"`, `'` 等特殊符號，限長度                       | 避免 XSS、避免名稱格式錯誤        |
| `location`        |  禁止 HTML 標籤，必要時進行 escape 處理                      | 避免位置欄位出現惡意內容或亂碼    |
| `capacity`        | 限制為整數 ≥ 0                                                  | 避免負數容量導致邏輯錯誤          |
| `contact_info`    | 限制為 Email 或電話格式，可使用正規表示式驗證                    | 確保聯絡資訊可用、避免 HTML 注入  |

---

* inventory 

| 欄位名稱         | 資料型別    | 唯一 | NOT NULL | 預設值           | 格式/限制                             | 說明                             |
|------------------|-------------|------|----------|------------------|----------------------------------------|----------------------------------|
| `inventory_id`   | INT   UNSIGNED     | Y    | Y        | AUTO_INCREMENT    | 正整數                              | 庫存記錄的唯一識別碼             |
| `warehouse_id`   | INT   UNSIGNED     | N    | N        | 無               | 外鍵參照 `Warehouses(warehouse_id)`    | 所屬倉庫識別碼（UUID）            |
| `product_id`     | INT   UNSIGNED       | N    | N        | 無               | 外鍵參照 `Products(product_id)`        | 對應商品識別碼（UUID）            |
| `stock_quantity` | INT         | N    | N        | 0                | 整數， 限制為 `>= 0`               | 當前庫存數量                      |
| `reorder_level`  | INT         | N    | N        | 10               | 整數， 限制為 `>= 0`               | 庫存補貨臨界值                    |
| `last_updated`   | TIMESTAMP   | N    | N        | CURRENT_TIMESTAMP | 自動更新時間                          | 最後更新庫存時間                  |


| 欄位名稱         |  限制方式                            | 理由說明                         |
|------------------|-----------------------------------------|----------------------------------|
| `stock_quantity` | 限制為整數且 ≥ 0                        | 避免負庫存邏輯錯誤               |
| `reorder_level`  | 限制為整數且 ≥ 0                        | 避免負值導致自動補貨邏輯錯亂     |


---

* Suppliers

| 欄位名稱        | 資料型別     | 唯一 | NOT NULL | 預設值           | 格式/限制                     | 說明                         |
|-----------------|--------------|------|----------|------------------|--------------------------------|------------------------------|
| `supplier_id`   | INT   UNSIGNED       | Y    | Y        | AUTO_INCREMENT   | 正整數                         | 供應商唯一識別碼             |
| `supplier_name` | STRING(100)  | N    | Y        | 無               | 最長 100 字                    | 供應商名稱                   |
| `contact_info`  | STRING(255)  | N    | N        | 無               | 電話、Email 或聯絡人格式       | 聯絡方式                     |
| `country`       | STRING(50)   | N    | N        | 無               | 國名、國碼或 ISO 代碼等         | 國家地區                     |
| `rating`        | INT          | N    | N        | 無               | 整數，限制為 1～5              | 評分等級                     |
| `created_at`    | TIMESTAMP    | N    | N        | CURRENT_TIMESTAMP | 自動填入系統時間               | 建立時間                     |

| 欄位名稱        |  限制方式                                             | 理由說明                         |
|-----------------|----------------------------------------------------------|----------------------------------|
| `supplier_name` | 禁止 `<`, `>`, `"`, `'` 等特殊符號， 限長度與格式      | 避免 XSS 與顯示錯誤               |
| `contact_info`  | 限制為電話或 Email 格式（可使用正規表示式）              | 確保聯絡資訊正確可用              |
| `country`       | 限制為 ISO 國碼或白名單格式                               | 避免亂碼或輸入不一致的國家名稱    |
| `rating`        | 僅允許整數，且需介於 1～5 之間                            | 符合評價邏輯，防止過高/過低分數     |


---

* Inbound_Shipments

| 欄位名稱        | 資料型別  | 唯一 | NOT NULL | 預設值     | 格式/限制                                    | 說明                         |
|-----------------|-----------|------|----------|------------|-----------------------------------------------|------------------------------|
| `inbound_id`    | INT  UNSIGNED     | Y    | Y        | AUTO_INCREMENT | 正整數                                 | 入庫記錄唯一識別碼           |
| `supplier_id`   | INT  UNSIGNED     | N    | N        | 無         | 外鍵參照 `Suppliers(supplier_id)`            | 供應商 ID                    |
| `warehouse_id`  | INT   UNSIGNED    | N    | N        | 無         | 外鍵參照 `Warehouses(warehouse_id)`          | 倉庫 ID                      |
| `product_id`    | INT  UNSIGNED     | N    | N        | 無         | 外鍵參照 `Products(product_id)`              | 商品 ID                      |
| `quantity`      | INT       | N    | Y        | 無         |  限制為整數且 ≥ 0                         | 入庫數量                     |
| `arrival_date`  | DATE      | N    | Y        | 無         | 合法日期格式                                 | 預計或實際到貨日期           |
| `status`        | ENUM      | N    | N        | `'pending'`| 僅限 `'pending'`, `'received'`, `'damaged'`  | 入庫狀態                     |

| 欄位名稱       |  限制方式                                             | 理由說明                         |
|----------------|----------------------------------------------------------|----------------------------------|
| `quantity`     | 限制為整數且 ≥ 0                                         | 避免負值造成入庫邏輯錯誤         |
| `arrival_date` | 必須為合法日期格式， 不可早於系統日期（邏輯控制）     | 避免異常過期或未來日期錯誤       |
| `status`       | 限制為 ENUM 白名單值                                     | 避免狀態錯誤或注入                |

---

* Outbound_Shipments

| 欄位名稱        | 資料型別  | 唯一 | NOT NULL | 預設值       | 格式/限制                                       | 說明                         |
|-----------------|-----------|------|----------|--------------|------------------------------------------------|------------------------------|
| `outbound_id`   | INT  UNSIGNED     | Y    | Y        | AUTO_INCREMENT | 正整數                                      | 出貨記錄唯一識別碼           |
| `order_id`      | INT  UNSIGNED     | N    | N        | 無           | 外鍵參照 `Orders(order_id)`                    | 所屬訂單 ID                  |
| `warehouse_id`  | INT  UNSIGNED     | N    | N        | 無           | 外鍵參照 `Warehouses(warehouse_id)`            | 出貨倉庫 ID                  |
| `product_id`    | INT   UNSIGNED    | N    | N        | 無           | 外鍵參照 `Products(product_id)`                | 出貨商品 ID                  |
| `quantity`      | INT       | N    | Y        | 無           |  限制為整數且 ≥ 0                           | 出貨數量                     |
| `dispatch_date` | DATE      | N    | Y        | 無           | 合法日期格式                                   | 實際出貨日期                 |
| `status`        | ENUM      | N    | N        | `'preparing'`| 僅限 `'preparing'`, `'shipped'`, `'delivered'` | 出貨狀態                     |

| 欄位名稱        |  限制方式                                             | 理由說明                         |
|-----------------|----------------------------------------------------------|----------------------------------|
| `quantity`      | 限制為整數且 ≥ 0                                         | 避免負值造成邏輯錯誤             |
| `dispatch_date` | 限制為合法日期格式                                       | 避免錯誤出貨時間紀錄             |
| `status`        | 限制為 ENUM 白名單值                                     | 避免狀態錯誤、注入或資料異常     |

---



* Order_Items

| 欄位名稱        | 資料型別      | 唯一 | NOT NULL | 預設值           | 格式/限制                                   | 說明                     |
|-----------------|---------------|------|----------|------------------|----------------------------------------------|--------------------------|
| `order_item_id` | INT   UNSIGNED        | Y    | Y        | AUTO_INCREMENT   | 正整數                                       | 訂單項目唯一識別碼       |
| `order_id`      | INT  UNSIGNED         | N    | N        | 無               | 外鍵參照 `Orders(order_id)`                  | 所屬訂單 ID              |
| `product_id`    | INT   UNSIGNED        | N    | N        | 無               | 外鍵參照 `Products(product_id)`              | 所屬商品 ID              |
| `quantity`      | INT           | N    | Y        | 無               |  限制為整數且 ≥ 1                         | 訂購數量                 |
| `price`         | DECIMAL(10,2) | N    | Y        | 無               |  限制為浮點數且 ≥ 0                        | 單項商品價格             |

| 欄位名稱     |  限制方式                               | 理由說明                     |
|--------------|--------------------------------------------|------------------------------|
| `quantity`   | 限制為整數且 ≥ 1                            | 避免負數與零造成邏輯錯誤     |
| `price`      | 限制為浮點數且 ≥ 0                          | 避免價格為負或錯誤計算       |

---

* Shipments

| 欄位名稱            | 資料型別      | 唯一 | NOT NULL | 預設值     | 格式/限制                                          | 說明                         |
|---------------------|---------------|------|----------|------------|-----------------------------------------------------|------------------------------|
| `shipment_id`       | INT    UNSIGNED       | Y    | Y        | AUTO_INCREMENT | 正整數                                          | 出貨記錄唯一識別碼           |
| `order_id`          | INT   UNSIGNED        | N    | N        | 無         | 外鍵參照 `Orders(order_id)`                        | 對應訂單 ID                  |
| `tracking_number`   | STRING(50)    | Y    | N        | 無         | 限英數字，避免特殊字元                             | 物流追蹤編號                 |
| `shipment_status`   | ENUM          | N    | Y        | 無         | 僅限 `preparing`, `shipped`, `delivered`            | 出貨狀態                     |
| `estimated_delivery`| DATE          | N    | N        | 無         | 合法日期格式                                       | 預計送達日                   |
| `actual_delivery`   | DATE          | N    | N        | 無         | 合法日期格式                                       | 實際送達日                   |
| `carrier`           | STRING(50)    | N    | N        | 無         | 如：DHL, FedEx, 自訂運輸商                          | 承運商名稱                   |
| `shipping_cost`     | DECIMAL(10,2) | N    | N        | 0.00       | 金額格式， 限制為 ≥ 0.00                        | 運費                         |

| 欄位名稱           |  限制方式                                           | 理由說明                         |
|--------------------|--------------------------------------------------------|----------------------------------|
| `tracking_number`  | 僅允許英數字，禁止 `<`, `>`, `"`, `'` 等特殊符號       | 避免 XSS 與排版錯誤              |
| `shipment_status`  | 僅允許 ENUM 白名單值                                    | 防止狀態錯誤或注入               |
| `estimated_delivery` / `actual_delivery` | 限合法日期格式，邏輯順序應合理     | 避免邏輯錯誤與資料矛盾           |
| `carrier`          | 禁止特殊符號、 限長度                               | 避免 XSS 或資料庫混亂            |
| `shipping_cost`    | 限制為 ≥ 0 的浮點數                                     | 避免負數導致錯誤計算             |

---

* Warehouse_Transfers

| 欄位名稱             | 資料型別  | 唯一 | NOT NULL | 預設值             | 格式/限制                                                    | 說明                         |
|----------------------|-----------|------|----------|---------------------|----------------------------------------------------------------|------------------------------|
| `transfer_id`        | INT   UNSIGNED    | Y    | Y        | AUTO_INCREMENT      | 正整數                                                       | 調撥記錄唯一識別碼           |
| `from_warehouse_id`  | INT  UNSIGNED     | N    | N        | 無                  | 外鍵參照 `Warehouses(warehouse_id)`                            | 調出倉庫 ID                  |
| `to_warehouse_id`    | INT   UNSIGNED    | N    | N        | 無                  | 外鍵參照 `Warehouses(warehouse_id)`                            | 調入倉庫 ID                  |
| `product_id`         | INT UNSIGNED      | N    | N        | 無                  | 外鍵參照 `Products(product_id)`                                | 商品 ID                      |
| `quantity`           | INT       | N    | Y        | 無                  |  限制為整數且 ≥ 1                                          | 調撥數量                     |
| `transfer_status`    | ENUM      | N    | N        | `'pending'`         | 僅限 `pending`, `in_transit`, `completed`                     | 調撥狀態                     |
| `transfer_date`      | TIMESTAMP | N    | N        | CURRENT_TIMESTAMP   | 系統時間自動填入                                              | 建立時間                     |

| 欄位名稱             |  限制方式                                            | 理由說明                         |
|----------------------|---------------------------------------------------------|----------------------------------|
| `quantity`           | 限制為整數且 ≥ 1                                        | 避免負值或零造成邏輯錯誤         |
| `transfer_status`    | 僅允許 ENUM 白名單值                                    | 避免資料注入與邏輯錯亂           |
| `from_warehouse_id` / `to_warehouse_id` | 不可相同                             | 須由應用層防止自我調撥            |

---



## 數據分析

* Sales_Analysis

| 欄位名稱         | 資料型別      | 唯一 | NOT NULL | 預設值           | 格式/限制                                     | 說明                         |
|------------------|---------------|------|----------|------------------|------------------------------------------------|------------------------------|
| `record_id`      | INT  UNSIGNED         | Y    | Y        | AUTO_INCREMENT   | 正整數                                         | 分析紀錄唯一識別碼           |
| `date`           | DATE          | N    | Y        | 無               | 合法日期格式                                   | 統計日期                     |
| `product_id`     | INT  UNSIGNED         | N    | N        | 無               | 外鍵參照 `Products(product_id)`                | 所屬商品 ID                  |
| `category_id`    | INT  UNSIGNED         | N    | N        | 無               | 外鍵參照 `Categories(category_id)`             | 所屬分類 ID                  |
| `total_sales`    | INT           | N    | Y        | 無               | 限制為整數且 ≥ 0                               | 銷售數量                     |
| `revenue`        | DECIMAL(10,2) | N    | Y        | 無               | 限制為浮點數且 ≥ 0                             | 銷售總金額                   |
| `average_price`  | DECIMAL(10,2) | N    | Y        | 無               | 限制為浮點數且 ≥ 0                             | 平均單價                     |

| 欄位名稱         |  限制方式                           | 理由說明                       |
|------------------|----------------------------------------|--------------------------------|
| `date`           | 限制為合法日期格式                     | 避免日期格式錯誤               |
| `total_sales`    | 限制為整數且 ≥ 0                       | 避免負值或異常統計              |
| `revenue`        | 限制為浮點數且 ≥ 0                     | 避免金額為負導致分析錯誤        |
| `average_price`  | 限制為浮點數且 ≥ 0                     | 避免資料統計失真                 |

---

* Inventory_Analytics

| 欄位名稱         | 資料型別 | 唯一 | NOT NULL | 預設值         | 格式/限制                                      | 說明                         |
|------------------|----------|------|----------|----------------|------------------------------------------------|------------------------------|
| `record_id`      | INT UNSIGNED     | Y    | Y        | AUTO_INCREMENT | 正整數                                         | 分析紀錄唯一識別碼           |
| `date`           | DATE     | N    | Y        | 無             | 合法日期格式                                   | 統計日期                     |
| `warehouse_id`   | INT  UNSIGNED    | N    | N        | 無             | 外鍵參照 `Warehouses(warehouse_id)`            | 倉庫 ID                      |
| `product_id`     | INT UNSIGNED     | N    | N        | 無             | 外鍵參照 `Products(product_id)`                | 商品 ID                      |
| `starting_stock` | INT      | N    | Y        | 無             | 限制為整數且 ≥ 0                               | 起始庫存量                   |
| `ending_stock`   | INT      | N    | Y        | 無             | 限制為整數且 ≥ 0                               | 結束庫存量                   |
| `sold_units`     | INT      | N    | Y        | 無             | 限制為整數且 ≥ 0                               | 銷售數量                     |
| `received_units` | INT      | N    | Y        | 無             | 限制為整數且 ≥ 0                               | 補貨數量                     |

| 欄位名稱         |  限制方式                   | 理由說明                     |
|------------------|--------------------------------|------------------------------|
| `date`           | 限制為合法日期格式             | 避免格式錯誤與資料錯亂       |
| `starting_stock` | 限制為整數且 ≥ 0               | 避免負庫存錯誤               |
| `ending_stock`   | 限制為整數且 ≥ 0               | 避免邏輯錯誤                 |
| `sold_units`     | 限制為整數且 ≥ 0               | 確保正確銷售計算             |
| `received_units` | 限制為整數且 ≥ 0               | 確保補貨量正確               |

---

* Order_Conversion_Stats

| 欄位名稱          | 資料型別      | 唯一 | NOT NULL | 預設值         | 格式/限制                          | 說明                            |
|-------------------|---------------|------|----------|----------------|-----------------------------------|---------------------------------|
| `record_id`       | INT   UNSIGNED        | Y    | Y        | AUTO_INCREMENT | 正整數                            | 統計紀錄唯一識別碼              |
| `date`            | DATE          | N    | Y        | 無             | 合法日期格式                      | 統計日期                        |
| `total_visits`    | INT           | N    | Y        | 無             | 整數，應 ≥ 0                      | 當日網站訪問次數                |
| `total_orders`    | INT           | N    | Y        | 無             | 整數，應 ≥ 0                      | 當日訂單數量                    |
| `conversion_rate` | DECIMAL(5,2)  | N    | Y        | 無             | 0~100，保留兩位小數              | 訂單轉換率（例如 4.35 代表 4.35%） |

| 欄位名稱          |  限制方式                         | 理由說明                        |
|-------------------|--------------------------------------|---------------------------------|
| `date`            | 限制為合法日期格式                   | 避免格式錯誤與資料錯亂          |
| `total_visits`    | 限制為整數且 ≥ 0                     | 避免邏輯錯誤                    |
| `total_orders`    | 限制為整數且 ≥ 0                     | 避免異常統計                    |
| `conversion_rate` | 限制為 0~100 的浮點數，保留兩位小數  | 避免無效百分比或除以零錯誤      |

---

* Shipment_Performance

| 欄位名稱               | 資料型別     | 唯一 | NOT NULL | 預設值         | 格式/限制                              | 說明                            |
|------------------------|--------------|------|----------|----------------|-----------------------------------------|---------------------------------|
| `record_id`            | INT  UNSIGNED        | Y    | Y        | AUTO_INCREMENT | 正整數                                  | 紀錄唯一識別碼                  |
| `date`                 | DATE         | N    | Y        | 無             | 合法日期格式                            | 統計日期                        |
| `carrier`              | STRING(50)   | N    | Y        | 無             | 最長 50 字， 限制字元類型            | 承運商名稱                      |
| `total_shipments`      | INT          | N    | Y        | 無             | 整數 ≥ 0                                | 當日總出貨數量                  |
| `on_time_deliveries`   | INT          | N    | Y        | 無             | 整數 ≥ 0                                | 準時送達筆數                    |
| `late_deliveries`      | INT          | N    | Y        | 無             | 整數 ≥ 0                                | 延遲送達筆數                    |
| `average_delivery_time`| INT          | N    | Y        | 無             | 整數 ≥ 0，單位為「小時」                | 平均送達時間（小時）            |

| 欄位名稱                |  限制方式                                  | 理由說明                          |
|-------------------------|-----------------------------------------------|-----------------------------------|
| `date`                  | 限制為合法日期格式                            | 避免格式錯誤與資料錯亂            |
| `carrier`               | 限制長度並禁止 `<`, `>`, `'`, `"` 等特殊字元 | 防止 XSS 與資料亂碼               |
| `total_shipments`       | 限制為整數且 ≥ 0                              | 避免負值導致邏輯錯誤              |
| `on_time_deliveries`    | 限制為整數且 ≥ 0                              | 避免統計錯誤                      |
| `late_deliveries`       | 限制為整數且 ≥ 0                              | 避免統計錯誤                      |
| `average_delivery_time` | 限制為整數且 ≥ 0                              | 避免統計錯誤（不可為負時間）      |

---

* Customer_Feedback_Stats

| 欄位名稱           | 資料型別      | 唯一 | NOT NULL | 預設值         | 格式/限制                                | 說明                            |
|--------------------|---------------|------|----------|----------------|-------------------------------------------|---------------------------------|
| `record_id`        | INT   UNSIGNED        | Y    | Y        | AUTO_INCREMENT | 正整數                                    | 統計紀錄唯一識別碼              |
| `date`             | DATE          | N    | Y        | 無             | 合法日期格式                              | 統計日期                        |
| `product_id`       | INT   UNSIGNED        | N    | N        | 無             | 外鍵參照 `Products(product_id)`            | 所屬商品 ID                     |
| `total_reviews`    | INT           | N    | Y        | 無             | 整數且 ≥ 0                                | 評論總數                        |
| `average_rating`   | DECIMAL(3,2)  | N    | Y        | 無             | 數值範圍 為 1.00 ~ 5.00                | 平均評分（星數）                |
| `positive_reviews` | INT           | N    | Y        | 無             | 整數且 ≥ 0                                | 好評數量                        |
| `negative_reviews` | INT           | N    | Y        | 無             | 整數且 ≥ 0                                | 差評數量                        |

| 欄位名稱           |  限制方式                              | 理由說明                     |
|--------------------|-------------------------------------------|------------------------------|
| `date`             | 限制為合法日期格式                        | 避免時間欄位錯誤             |
| `total_reviews`    | 限制為整數且 ≥ 0                          | 避免統計錯誤                 |
| `average_rating`   | 限制為浮點數，範圍 為 1.00 至 5.00     | 確保評分合理                 |
| `positive_reviews` | 限制為整數且 ≥ 0                          | 避免負值導致邏輯錯誤         |
| `negative_reviews` | 限制為整數且 ≥ 0                          | 避免負值導致邏輯錯誤         |


## 訂單管理

* orders

| 欄位名稱       | 資料型別      | 唯一 | NOT NULL | 預設值                            | 格式/限制                                                              | 說明                       |
|----------------|---------------|------|----------|------------------------------------|------------------------------------------------------------------------|----------------------------|
| `order_id`     | INT     UNSIGNED      | Y    | Y        | AUTO_INCREMENT                     | 正整數                                                                 | 訂單唯一識別碼             |
| `customer_id`  | INT    UNSIGNED       | N    | Y        | 無                                 | 外鍵參照 `users(user_id)`                                               | 購買者 ID                  |
| `order_status` | ENUM          | N    | Y        | 無                                 | 僅允許 `pending`, `shipped`, `delivered`, `cancelled`,`return`                  | 訂單狀態                   |
| `total_amount` | DECIMAL(10,2) | N    | Y        | 無                                 | 必須為正數，兩位小數                                                  | 訂單總金額                 |
| `created_at`   | TIMESTAMP     | N    | N        | CURRENT_TIMESTAMP                  | 系統時間自動填入                                                       | 建立時間                   |
| `updated_at`   | TIMESTAMP     | N    | N        | CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP | 系統時間自動更新                                       | 最後更新時間               |
| `shipping_fee` | DECIMAL(10,2) | N    | N        | 0.00                               | 金額格式，兩位小數                                                     | 運費                       |
| `discount`     | DECIMAL(10,2) | N    | N        | 0.00                               | 金額格式，兩位小數                                                     | 折扣金額                   |
| `payment_id`   | INT   UNSIGNED        | N    | N        | 無                                 | 外鍵參照 `payments(payment_id)`                                        | 對應付款紀錄 ID            |
| `shipping_id`  | INT  UNSIGNED         | N    | N        | 無                                 | 外鍵參照 `shipments(shipment_id)`                                      | 對應物流紀錄 ID            |
| `coupon_id`    | INT   UNSIGNED        | N    | N        | 無                                 | 外鍵參照 `coupons(coupon_id)`                                          | 使用的優惠券 ID            |

| 欄位名稱       |  限制方式                                         | 理由說明                        |
|----------------|------------------------------------------------------|---------------------------------|
| `order_status` | ENUM 限制白名單：'pending', 'shipped', 'delivered', 'cancelled' | 避免錯誤或惡意狀態注入         |
| `total_amount` | 限制為浮點數且 ≥ 0                                   | 避免負值導致邏輯錯誤            |
| `shipping_fee` | 限制為浮點數且 ≥ 0                                   | 防止運費為負                    |
| `discount`     | 限制為浮點數且 ≥ 0 且 ≤ `total_amount`               | 避免錯誤折扣超額                |
| `created_at`   | 僅允許系統填入，不接受外部輸入                       | 防止偽造時間                    |
| `updated_at`   | 僅允許系統自動更新                                   | 避免修改歷史紀錄                |

---

* order_items

| 欄位名稱        | 資料型別      | 唯一 | NOT NULL | 預設值         | 格式/限制                                      | 說明                          |
|-----------------|---------------|------|----------|----------------|------------------------------------------------|-------------------------------|
| `order_item_id` | INT    UNSIGNED       | Y    | Y        | AUTO_INCREMENT | 正整數                                         | 訂單項目唯一識別碼            |
| `order_id`      | INT   UNSIGNED        | N    | N        | 無             | 外鍵參照 `orders(order_id)`                    | 所屬訂單 ID                    |
| `product_id`    | INT   UNSIGNED        | N    | Y        | 無             | 外鍵參照 `products(product_id)`                | 商品 ID                        |
| `quantity`      | INT           | N    | Y        | 無             | 整數且 ≥ 1                                     | 訂購數量                       |
| `unit_price`    | DECIMAL(10,2) | N    | Y        | 無             | 正數，小數點後 2 位                            | 商品單價                       |
| `subtotal`      | DECIMAL(10,2) | N    | Y        | 計算欄位       | 自動計算：`quantity * unit_price`              | 該商品項目小計                |


| 欄位名稱     |  限制方式                           | 理由說明                      |
|--------------|----------------------------------------|-------------------------------|
| `quantity`   | 限制為整數且 ≥ 1                       | 確保購買數量合理               |
| `unit_price` | 限制為浮點數且 ≥ 0.01                  | 防止免費或錯誤價格             |
| `subtotal`   | 設為運算欄位，不允許手動輸入           | 避免使用者竄改價格             |

---

* payments

| 欄位名稱         | 資料型別      | 唯一 | NOT NULL | 預設值             | 格式/限制                                                           | 說明                         |
|------------------|---------------|------|----------|---------------------|----------------------------------------------------------------------|------------------------------|
| `payment_id`     | INT  UNSIGNED         | Y    | Y        | AUTO_INCREMENT      | 正整數                                                             | 付款記錄唯一識別碼           |
| `order_id`       | INT  UNSIGNED         | N    | N        | 無                  | 外鍵參照 `Orders(order_id)`                                          | 所屬訂單 ID                  |
| `payment_method` | ENUM          | N    | Y        | 無                  | 僅允許 `credit_card`, `paypal`, `bank_transfer`, `cash_on_delivery` | 付款方式                     |
| `payment_status` | ENUM          | N    | Y        | 無                  | 僅允許 `pending`, `completed`, `failed`                             | 付款狀態                     |
| `amount`         | DECIMAL(10,2) | N    | Y        | 無                  | 金額格式， 限制 ≥ 0                                              | 付款金額                     |
| `payment_date`   | TIMESTAMP     | N    | N        | CURRENT_TIMESTAMP   | 自動填入系統時間                                                    | 付款時間                     |

| 欄位名稱         |  限制方式                                            | 理由說明                     |
|------------------|---------------------------------------------------------|------------------------------|
| `payment_method` | 僅允許 ENUM 白名單值                                    | 避免注入與資料混亂           |
| `payment_status` | 僅允許 ENUM 白名單值                                    | 避免狀態異常或錯誤邏輯       |
| `amount`         | 限制為浮點數且 ≥ 0                                       | 避免負數與異常付款金額       |

---



* Returns_Refunds

| 欄位名稱        | 資料型別      | 唯一 | NOT NULL | 預設值             | 格式/限制                                                        | 說明                         |
|-----------------|---------------|------|----------|---------------------|-------------------------------------------------------------------|------------------------------|
| `return_id`     | INT  UNSIGNED         | Y    | Y        | AUTO_INCREMENT      | 正整數                                                          | 退貨/退款記錄唯一識別碼      |
| `order_id`      | INT   UNSIGNED        | N    | N        | 無                  | 外鍵參照 `Orders(order_id)`                                       | 所屬訂單 ID                  |
| `product_id`    | INT  UNSIGNED         | N    | N        | 無                  | 外鍵參照 `Products(product_id)`                                   | 所屬商品 ID                  |
| `reason`        | STRING        | N    | N        | 無                  |  長度限制，並防止特殊符號                                     | 退貨/退款原因文字描述        |
| `status`        | ENUM          | N    | N        | `'requested'`       | 僅允許 `requested`, `approved`, `rejected`, `processed`           | 處理狀態                     |
| `refund_amount` | DECIMAL(10,2) | N    | N        | 無                  |  限制為浮點數且 ≥ 0                                           | 退款金額（可為部分退款）     |
| `created_at`    | TIMESTAMP     | N    | N        | CURRENT_TIMESTAMP   | 系統時間自動填入                                                | 建立時間                     |

| 欄位名稱        |  限制方式                                                | 理由說明                     |
|-----------------|-------------------------------------------------------------|------------------------------|
| `reason`        | 禁止 `<`, `>`, `"`, `'` 等特殊符號， 轉義或濾除         | 避免 XSS 注入與亂碼           |
| `status`        | 僅允許 ENUM 白名單值                                        | 防止資料注入與狀態錯誤        |
| `refund_amount` | 限制為浮點數且 ≥ 0                                          | 避免負值導致錯誤金額處理      |
