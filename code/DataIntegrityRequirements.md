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
* 登入 :使用者名稱和密碼。程式將使用資料庫的整合安全性，因此這些資料不需要存儲在資料庫中。

| Field 名稱           | Data Type | 唯一 | Not Null | 預設值                    | 格式/限制                            | 說明                     |
|----------------------|-----------|------|----------|---------------------------|-------------------------------------|--------------------------|
| user_id (主鍵)       | BIGINT       | Y    | Y        | AUTO_INCREMENT            | 正整數                               | 每位使用者的唯一識別碼   |
| email                | STRING    | Y    | Y        | 無                        | 必須為有效 Email 格式                | 使用者註冊用 Email       |
| password_hash        | STRING    | N    | Y        | 無                        | 至少 8 字，含大小寫 + 數字 + 特殊符號（經過 bcrypt） | 雜湊後的使用者密碼       |
| Account Enable       | BOOL      | N    | Y        | True                      | 僅允許 True/False                    | 是否啟用帳號             |
| created_at           | TIMESTAMP | N    | Y        | CURRENT_TIMESTAMP         | 系統自動填入                         | 註冊時間                 |
| failed_login_attempts| INT       | N    | Y        | 0                         | 整數（< 5）                          | 登入失敗次數累計         |
| is_verified          | BOOL      | N    | Y        | 無                        | 僅允許 True/False                    | Email 是否完成驗證       |
| updated_at           | TIMESTAMP | N    | Y        | CURRENT_TIMESTAMP ON UPDATE | 自動更新                         | 資料更新時間             |

---

*   登入紀錄 : 使用者登入紀錄。程式將使用資料庫的整合安全性，因此這些資料不需要存儲在資料庫中。

| Field 名稱           | Data Type     | 唯一 | Not Null | 預設值                | 格式/限制                             | 說明                         |
|----------------------|---------------|------|----------|------------------------|----------------------------------------|------------------------------|
| log_id (主鍵)        | BIGINT        | Y    | Y        | AUTO_INCREMENT         | 正整數                                 | 每筆登入紀錄的唯一識別碼     |
| user_id      | INT           | N    | Y       | 無                    | 外鍵參照 `Users` 表                         | 嘗試登入的使用者     |
| email                 | STRING        | N    | Y       | 無                    | 必須為有效 Email 格式                     | 嘗試登入的帳號（即使錯誤）   |
| login_time           | TIMESTAMP     | N    | Y        | CURRENT_TIMESTAMP      | 時間格式                               | 登入嘗試的發生時間           |
| ip_address           | STRING        | N    | Y        | 無                    | IPv4 或 IPv6 格式                     | 登入來源 IP                  |
| user_agent           | STRING(255)          | N    | N        | 無                    | -                                      | 使用者裝置資訊               |
| success              | BOOL          | N    | Y        | 無                    | 僅允許 True / False                   | 是否成功登入                 |
| failure_reason       | STRING(255)    | N    | N        | 無                    | 最長 255 字                            | 登入失敗原因（如密碼錯誤）   |

---

## 商品管理

* Product : 關於買賣家刊登商品相關資料表

| 欄位名稱             | 資料型別         | 唯一 | NOT NULL | 預設值                              | 格式/限制                              | 說明                         |
|----------------------|------------------|------|----------|--------------------------------------|----------------------------------------|------------------------------|
| product_id (主鍵)    | BIGINT (SERIAL)  | Y    | Y        | AUTO_INCREMENT                        | 正整數                                 | 商品唯一識別碼               |
| product_name         | STRING(255)      | N    | Y        | 無                                   | 最長 255 字                            | 商品名稱                     |
| sku                  | STRING(100)      | Y    | Y        | 無                                   | 唯一 SKU 編碼                          | 商品庫存單位識別碼           |
| brand                | STRING(100)      | N    | N        | 無                                   | -                                      | 商品品牌                     |
| model                | STRING(100)      | N    | N        | 無                                   | -                                      | 商品型號                     |
| description          | STRING (255)     | N    | N        | 無                                   |   最長 255 字                            | 商品描述                     |
| category_id          | BIGINT UNSIGNED  | N    | Y        | 無                                   | 外鍵參照 `categories` 表              | 商品所屬分類                 |
| variant_type         | STRING(255)      | N    | N        | 無                                   | 例如：尺寸、顏色                       | 商品變體類型                 |
| price                | DECIMAL(10,2)    | N    | Y        | 無                                   | 金額格式（最多 99999999.99）           | 商品原價                     |
| promotional_price    | DECIMAL(10,2)    | N    | N        | 無                                   | 金額格式                               | 特價價格（如有）             |
| promotion_start_date | TIMESTAMP        | N    | N        | 無                                   | -                                      | 特價開始時間                 |
| promotion_end_date   | TIMESTAMP        | N    | N        | 無                                   | -                                      | 特價結束時間                 |
| stock_quantity       | INT              | N    | Y        | 0                                    | 正整數                                 | 商品目前庫存量               |
| seller_id            | BIGINT UNSIGNED  | N    | N        | 無                                   | 外鍵參照 `sellers` 表                 | 所屬賣家 ID                  |
| shipping_weight      | DECIMAL(10,2)    | N    | N        | 0                                    | 單位：公斤                             | 運送重量                     |
| image_url            | STRING(255)      | N    | N        | 無                                   | URL 格式                               | 商品主圖網址                 |
| barcode              | STRING(50)       | N    | N        | 無                                   | 可為 EAN / UPC                         | 商品條碼                     |
| reviews_count        | INT              | N    | N        | 0                                    | 正整數                                 | 評論總數                     |
| favorites_count      | INT              | N    | N        | 0                                    | 正整數                                 | 被加入最愛次數               |
| date_added           | TIMESTAMP        | N    | N        | CURRENT_TIMESTAMP                    | 系統時間                               | 建立時間                     |
| last_updated         | TIMESTAMP        | N    | N        | CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP | 自動更新時間           | 最後更新時間                 |



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

* Category : 商品分類表

| 欄位名稱           | 資料型別     | 唯一 | NOT NULL | 預設值           | 格式/限制               | 說明                     |
|--------------------|--------------|------|----------|-------------------|--------------------------|--------------------------|
| category_id        | BIGINT       | Y    | Y        | AUTO_INCREMENT    | 正整數                   | 分類唯一識別碼           |
| category_name      | STRING(255)  | Y    | Y        | 無                | 最長 255 字，唯一         | 分類名稱                 |
| category_description | STRING     | N    | N        | 無                | 無長度限制（原為 TEXT）   | 分類描述文字             |

| 欄位名稱           | 建議限制方式                                           | 理由說明                     |
|--------------------|--------------------------------------------------------|------------------------------|
| category_name      | 禁止特殊符號如 `<`, `>`, `"`, `'`，僅允許中英文與數字 | 避免 XSS 與資料亂碼           |
| category_description | HTML escape 或僅允許特定格式                           | 預防惡意 HTML 注入           |


---

* Review : 關於買家購買商品後評論

| 欄位名稱   | 資料型別       | 唯一 | NOT NULL | 預設值              | 格式/限制                  | 說明                     |
|------------|----------------|------|----------|----------------------|----------------------------|--------------------------|
| review_id  | BIGINT         | Y    | Y        | AUTO_INCREMENT       | 正整數                     | 每筆評論的唯一識別碼     |
| product_id | BIGINT         | N    | N        | 無                   | 外鍵參照 `products` 表     | 被評論的商品 ID          |
| user_id    | INT            | N    | N        | 無                   | 外鍵參照 `users` 表        | 發表評論的使用者 ID      |
| rating     | INT            | N    | Y        | 無                   | 建議限制 1～5              | 評分數值（如星數）        |
| comment    | STRING         | N    | Y        | 無                   | 無長度限制（原為 TEXT）    | 評論內容文字             |
| created_at | TIMESTAMP      | N    | Y        | CURRENT_TIMESTAMP    | 自動記錄建立時間           | 評論建立時間             |

| 欄位名稱  | 限制方式                                       | 理由說明                       |
|-----------|----------------------------------------------------|--------------------------------|
| rating    | 限制為 1～5 的整數                                  | 避免超出評分邏輯範圍             |
| comment   | 禁止 `<`, `>`, `"`, `'`, `/` 等符號或進行 HTML escape | 預防 XSS 攻擊                   |

---

