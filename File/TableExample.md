## 用戶管理 - 謝延偵

* MembershipLevels

| level_id | level_name | required_points | discount_rate |
|----------|------------|-----------------|---------------|
| 1 | 銅魚 | 0 | 2.00 |
| 2 | 銀魚 | 1000 | 5.00 |
| 3 | 金魚 | 3000 | 10.00 |
| 4 | 銅魚 | 0 | 2.00 |
| 5 | 銀魚 | 1000 | 5.00 |
| 6 | 金魚 | 3000 | 10.00 |
| 7 | 銅魚 | 0 | 2.00 |
| 8 | 銀魚 | 1000 | 5.00 |
| 9 | 金魚 | 3000 | 10.00 |
| 10 | 銅魚 | 0 | 2.00 |

* Users

| user_id | username | email | phone | registration_date | is_active | membership_level_id | shipping_address | billing_address |
|---------|----------|-------|-------|-------------------|-----------|-------------------|------------------|-----------------|
| 1 | 陳小明 | chen.xiaoming@gmail.com | 0912345678 | 2023-01-15 10:30:00+08 | TRUE | 2 | 台北市大安區忠孝東路四段112號 | 台北市大安區忠孝東路四段112號 |
| 2 | 林雅婷 | lin.yating@yahoo.com.tw | 0923456789 | 2023-02-20 14:22:15+08 | TRUE | 1 | 新北市板橋區文化路二段88號 | 新北市板橋區文化路二段88號 |
| 3 | 王大偉 | david.wang@hotmail.com | 0934567890 | 2023-03-08 09:45:30+08 | TRUE | 3 | 台中市西區台灣大道二段285號 | 台中市西區台灣大道二段285號 |
| 4 | 張美玲 | meiling.chang@outlook.com | 0945678901 | 2023-04-12 16:18:45+08 | FALSE | 1 | 高雄市前金區中正四路105號 | 高雄市前金區中正四路105號 |
| 5 | 李志豪 | lee.zhihao@gmail.com | 0956789012 | 2023-05-25 11:33:20+08 | TRUE | 3 | 台南市東區長榮路三段66號 | 台南市東區長榮路三段66號 |
| 6 | 黃淑芬 | huang.shufen@yahoo.com.tw | 0967890123 | 2023-06-30 13:55:10+08 | TRUE | 1 | 桃園市中壢區中正路199號 | 桃園市中壢區中正路199號 |
| 7 | 劉建國 | liu.jianguo@gmail.com | 0978901234 | 2023-07-14 08:27:35+08 | TRUE | 3 | 新竹市東區光復路二段321號 | 新竹市東區光復路二段321號 |
| 8 | 蔡雨晴 | tsai.yuqing@hotmail.com | 0989012345 | 2023-08-19 15:42:18+08 | TRUE | 2 | 彰化市中山路一段177號 | 彰化市中山路一段177號 |
| 9 | 楊文華 | yang.wenhua@outlook.com | 0990123456 | 2023-09-22 12:08:42+08 | TRUE | 1 | 嘉義市西區文化路255號 | 嘉義市西區文化路255號 |
| 10 | 吳玉芳 | wu.yufang@gmail.com | 0901234567 | 2023-10-05 17:15:55+08 | TRUE | 2 | 宜蘭市中山路五段133號 | 宜蘭市中山路五段133號 |

* Sellers

| seller_id | user_id | store_name | store_description | bank_account |
|-----------|---------|------------|-------------------|--------------|
| 1 | 3 | 大偉3C數位館 | 專售手機、平板、電腦週邊商品，品質保證，價格實惠 | 700-0001234567890 |
| 2 | 5 | 志豪文創小舖 | 手作文創商品、客製化禮品、文具用品專賣店 | 822-0009876543210 |
| 3 | 7 | 建國運動用品 | 各式運動器材、健身用品、戶外用品專業販售 | 012-3456789012345 |
| 4 | 1 | 小明美食天地 | 精選台灣在地美食、伴手禮、有機農產品 | 808-0002468013579 |
| 5 | 10 | 玉芳居家生活 | 居家裝飾、收納用品、廚房用具一應俱全 | 703-0001357924680 |
| 6 | 2 | 雅婷時尚服飾 | 女性服飾、配件、包包，追求時尚與品質並重 | 700-0009630741852 |
| 7 | 8 | 雨晴寵物天堂 | 寵物用品、飼料、玩具，寵物健康照護專家 | 822-0007410852963 |
| 8 | 4 | 美玲花藝工作室 | 鮮花花束、盆栽、園藝用品，美化您的生活空間 | 012-9876543210987 |
| 9 | 6 | 淑芬手工烘焙 | 手工餅乾、蛋糕、麵包，天然食材無添加 | 808-0001472583690 |
| 10 | 9 | 文華書香閣 | 二手書籍、文學作品、學習教材，知識的寶庫 | 703-0008529637410 |

* Points

| point_id | user_id | points_earned | transaction_date | description |
|----------|---------|---------------|------------------|-------------|
| 1 | 1 | 150 | 2024-01-10 14:30:25+08 | 購物 |
| 2 | 2 | 80 | 2024-01-15 09:45:12+08 | 活動 |
| 3 | 3 | 320 | 2024-01-20 16:22:38+08 | 購物 |
| 4 | 4 | 50 | 2024-01-25 11:15:45+08 | 簽到 |
| 5 | 5 | 250 | 2024-02-01 13:55:20+08 | 活動 |
| 6 | 6 | 120 | 2024-02-08 10:33:15+08 | 購物 |
| 7 | 7 | -200 | 2024-02-14 15:18:30+08 | 折抵消費 |
| 8 | 8 | 90 | 2024-02-20 08:42:55+08 | 簽到 |
| 9 | 9 | 180 | 2024-02-28 17:25:10+08 | 活動 |
| 10 | 10 | -100 | 2024-03-05 12:38:42+08 | 折抵消費 |

* Coupons

| coupon_id | coupon_code | discount_amount | expiry_date | usage_limit | is_active | coupon_type |
|-----------|-------------|-----------------|-------------|-------------|-----------|-------------|
| 1 | WELCOME100 | 100.00 | 2024-12-31 | NULL | TRUE | single_use |
| 2 | SPRING2024 | 50.00 | 2024-06-30 | 1000 | TRUE | multi_use_limited |
| 3 | VIP200 | 200.00 | 2024-10-31 | NULL | TRUE | single_use |
| 4 | SUMMER20 | 20.00 | 2024-08-31 | 500 | TRUE | multi_use_limited |
| 5 | BIRTHDAY150 | 150.00 | 2024-12-31 | NULL | TRUE | single_use |
| 6 | FRIEND80 | 80.00 | 2024-09-30 | 300 | TRUE | multi_use_limited |
| 7 | WEEKEND30 | 30.00 | 2024-07-31 | 200 | FALSE | multi_use_limited |
| 8 | NEWUSER50 | 50.00 | 2024-12-31 | NULL | TRUE | single_use |
| 9 | GOLD300 | 300.00 | 2024-11-30 | NULL | TRUE | single_use |
| 10 | FLASH25 | 25.00 | 2024-06-15 | 100 | TRUE | multi_use_limited |

* Messages

| message_id | sender_id | receiver_id | message_content | send_time | is_read |
|------------|-----------|-------------|-----------------|-----------|---------|
| 1 | 1 | 3 | 你好，想詢問這款手機還有現貨嗎？ | 2024-03-01 10:15:30+08 | TRUE |
| 2 | 3 | 1 | 有的，目前還有3台黑色的，需要的話可以直接下單 | 2024-03-01 10:22:45+08 | TRUE |
| 3 | 2 | 6 | 這件洋裝有其他顏色嗎？ | 2024-03-02 14:33:20+08 | FALSE |
| 4 | 5 | 8 | 請問寵物飼料有提供試吃包嗎？ | 2024-03-03 09:45:15+08 | TRUE |
| 5 | 8 | 5 | 當然有，我們有小包裝的試吃包，可以先試試看 | 2024-03-03 09:52:30+08 | TRUE |
| 6 | 4 | 9 | 想買幾本程式設計的書，有推薦的嗎？ | 2024-03-04 16:18:45+08 | TRUE |
| 7 | 9 | 4 | 我推薦Python入門和JavaScript實戰這兩本 | 2024-03-04 16:25:12+08 | FALSE |
| 8 | 7 | 10 | 這個收納盒的尺寸是多少？ | 2024-03-05 11:40:55+08 | TRUE |
| 9 | 10 | 7 | 長30cm、寬20cm、高15cm，很適合放書桌上 | 2024-03-05 11:48:20+08 | TRUE |
| 10 | 6 | 1 | 謝謝你的蛋糕食譜分享，做出來很成功！ | 2024-03-06 13:25:38+08 | FALSE |

* Notifications

| notification_id | user_id | notification_type | subject | message | sent_at | is_read | related_entity | related_entity_id |
|-----------------|---------|-------------------|---------|---------|---------|---------|----------------|-------------------|
| 1 | 1 | order | 訂單確認通知 | 您的訂單 #12345 已確認，預計3-5個工作天送達 | 2024-03-01 11:30:15+08 | TRUE | order | 12345 |
| 2 | 2 | promotion | 限時優惠活動 | 春季服飾特賣會開始了！全館8折起 | 2024-03-02 09:00:00+08 | FALSE | promotion | 101 |
| 3 | 3 | message | 新訊息通知 | 您收到了來自陳小明的新訊息 | 2024-03-01 10:22:50+08 | TRUE | message | 2 |
| 4 | 4 | system | 密碼修改通知 | 您的密碼已成功修改，若非本人操作請聯繫客服 | 2024-03-03 15:45:20+08 | TRUE | user | 4 |
| 5 | 5 | points | 點數獲得通知 | 恭喜您獲得250點數！繼續加油！ | 2024-02-01 13:55:25+08 | TRUE | points | 5 |
| 6 | 6 | order | 配送通知 | 您的訂單正在配送中，預計今日下午送達 | 2024-03-04 08:30:45+08 | FALSE | order | 67890 |
| 7 | 7 | review | 評價提醒 | 別忘了為您購買的商品留下評價喔！ | 2024-03-05 10:15:30+08 | FALSE | order | 11111 |
| 8 | 8 | coupon | 優惠券到期提醒 | 您的優惠券將於明天到期，記得使用喔！ | 2024-03-06 12:00:00+08 | TRUE | coupon | 7 |
| 9 | 9 | membership | 會員升級通知 | 恭喜您升級為銀魚會員！享受更多專屬優惠 | 2024-03-07 14:22:18+08 | TRUE | user | 9 |
| 10 | 10 | promotion | 生日優惠 | 生日快樂！專屬生日優惠券已送到您的帳戶 | 2024-03-08 00:00:00+08 | FALSE | coupon | 5 |

## 商品管理 - 楊峻朋

* Product

| product_id | product_name     | sku       | brand     | model       | price   | promotional_price | category_id | seller_id | reviews_count | favorites_count | date_added          | last_updated         |
|------------|------------------|-----------|-----------|-------------|---------|-------------------|-------------|-----------|----------------|------------------|----------------------|----------------------|
| 1          | 無線滑鼠         | WM-001    | Logitech  | M185        | 599.00  | 499.00            | 1           | 1         | 23             | 15               | 2025-05-30 10:00:00  | 2025-05-30 10:00:00  |
| 2          | 機械式鍵盤       | KB-002    | Keychron  | K2 V2       | 2890.00 | 2590.00           | 1           | 2         | 120            | 78               | 2025-05-30 10:01:00  | 2025-05-30 10:01:00  |
| 3          | 藍牙耳機         | BT-003    | Sony      | WF-1000XM4  | 7990.00 | NULL              | 2           | 1         | 55             | 40               | 2025-05-30 10:02:00  | 2025-05-30 10:02:00  |
| 4          | 曲面電競螢幕     | MON-004   | Samsung   | C27F398     | 6990.00 | 6490.00           | 3           | 3         | 10             | 7                | 2025-05-30 10:03:00  | 2025-05-30 10:03:00  |
| 5          | 筆電支架         | ST-005    | MOFT      | Z Sit-Stand | 1380.00 | NULL              | 4           | 2         | 8              | 12               | 2025-05-30 10:04:00  | 2025-05-30 10:04:00  |
| 6          | 外接固態硬碟     | SSD-006   | SanDisk   | Extreme V2  | 3290.00 | 2990.00           | 5           | 4         | 34             | 22               | 2025-05-30 10:05:00  | 2025-05-30 10:05:00  |
| 7          | Type-C 集線器    | HUB-007   | UGREEN    | 9-in-1      | 990.00  | 890.00            | 6           | 1         | 19             | 18               | 2025-05-30 10:06:00  | 2025-05-30 10:06:00  |
| 8          | 攝影腳架         | TRIP-008  | Manfrotto | MK290DUA3   | 4690.00 | NULL              | 7           | 5         | 4              | 6                | 2025-05-30 10:07:00  | 2025-05-30 10:07:00  |
| 9          | 行動電源         | PB-009    | Anker     | PowerCore   | 1190.00 | 990.00            | 8           | 1         | 45             | 51               | 2025-05-30 10:08:00  | 2025-05-30 10:08:00  |
| 10         | 智慧手錶         | WT-010    | Apple     | Watch SE    | 8990.00 | 8490.00           | 9           | 2         | 65             | 58               | 2025-05-30 10:09:00  | 2025-05-30 10:09:00  |

* category

| category_id | category_name     | category_description                    |
|-------------|-------------------|------------------------------------------|
| 1           | 電腦週邊          | 滑鼠、鍵盤、耳機等常用配件               |
| 2           | 音訊設備          | 各類耳機、喇叭、麥克風等聲音相關設備     |
| 3           | 顯示器            | 各種尺寸與功能的顯示器與螢幕             |
| 4           | 筆電配件          | 筆電支架、散熱墊、包包等相關產品         |
| 5           | 儲存設備          | SSD、隨身碟、記憶卡與備份設備             |
| 6           | 轉接與集線設備    | USB Hub、轉接頭與連接線等               |
| 7           | 攝影器材          | 腳架、雲台、燈光與攝影周邊               |
| 8           | 行動裝置配件      | 行動電源、充電線與手機架                 |
| 9           | 穿戴裝置          | 智慧手錶、手環與健康監控設備             |
| 10          | 智慧家電          | 家用物聯網設備、智慧插座、溫濕度感測器   |

* review

| review_id | product_id | user_id | rating | comment                                             | created_at          |
|-----------|------------|---------|--------|------------------------------------------------------|---------------------|
| 1         | 1          | 101     | 5      | 滑鼠非常靈敏，握感舒適，超值！                         | 2025-05-30 09:01:00 |
| 2         | 2          | 102     | 4      | 鍵盤手感不錯，燈效很炫，但有點重                         | 2025-05-30 09:02:00 |
| 3         | 3          | 103     | 5      | 耳機降噪效果超棒，聽音樂很有沉浸感                       | 2025-05-30 09:03:00 |
| 4         | 4          | 104     | 3      | 螢幕解析度不錯，但底座不太穩                             | 2025-05-30 09:04:00 |
| 5         | 5          | 105     | 4      | 筆電支架輕巧又方便，角度調整剛好                         | 2025-05-30 09:05:00 |
| 6         | 6          | 106     | 5      | 傳輸速度超快！非常適合備份資料                           | 2025-05-30 09:06:00 |
| 7         | 7          | 107     | 4      | 集線器支援功能多，但插孔間距有點擠                       | 2025-05-30 09:07:00 |
| 8         | 8          | 108     | 5      | 腳架很穩，出外拍照必備                                  | 2025-05-30 09:08:00 |
| 9         | 9          | 109     | 2      | 行動電源容量不如預期，使用兩次就沒電了                   | 2025-05-30 09:09:00 |
| 10        | 10         | 110     | 5      | Apple Watch 很實用，健康追蹤功能很準                     | 2025-05-30 09:10:00 |


## 驗證伺服器

* users 

| user_id | email               | password_hash        | account_enable | created_at          | failed_login_attempts | is_verified | updated_at          |
|---------|---------------------|-----------------------|----------------|----------------------|------------------------|-------------|----------------------|
| 101     | alice@example.com   | $2b$12$aliceHash      | true           | 2025-05-01 09:00:00 | 0                      | true        | 2025-05-01 09:00:00  |
| 102     | bob@example.com     | $2b$12$bobHash        | true           | 2025-05-02 09:30:00 | 2                      | true        | 2025-05-28 13:00:00  |
| 103     | charlie@example.com | $2b$12$charlieHash    | false          | 2025-05-03 08:50:00 | 5                      | true        | 2025-05-29 10:00:00  |
| 104     | diana@example.com   | $2b$12$dianaHash      | false          | 2025-05-04 11:20:00 | 0                      | false       | 2025-05-30 08:00:00  |
| 105     | ethan@example.com   | $2b$12$ethanHash      | true           | 2025-05-05 07:45:00 | 1                      | true        | 2025-05-30 07:45:00  |
| 106     | fiona@example.com   | $2b$12$fionaHash      | true           | 2025-05-06 12:15:00 | 0                      | true        | 2025-05-06 12:15:00  |
| 107     | george@example.com  | $2b$12$georgeHash     | true           | 2025-05-07 10:10:00 | 0                      | false       | 2025-05-30 08:17:00  |
| 108     | hannah@example.com  | $2b$12$hannahHash     | true           | 2025-05-08 09:55:00 | 0                      | true        | 2025-05-08 09:55:00  |
| 109     | ian@example.com     | $2b$12$ianHash        | true           | 2025-05-09 14:20:00 | 4                      | true        | 2025-05-29 18:00:00  |
| 110     | jess@example.com    | $2b$12$jessHash       | true           | 2025-05-10 16:40:00 | 0                      | false       | 2025-05-10 16:40:00  |



* login_logs

| log_id | user_id | email                  | login_time          | ip_address      | user_agent                        | success | failure_reason           |
|--------|---------|------------------------|----------------------|------------------|-----------------------------------|---------|--------------------------|
| 1      | 101     | alice@example.com      | 2025-05-30 08:00:01 | 192.168.1.10     | Mozilla/5.0 (Windows NT 10.0)     | true    | NULL                     |
| 2      | 102     | bob@example.com        | 2025-05-30 08:03:17 | 172.16.0.5       | Chrome/90.0 (Mac OS X 10.15)      | false   | Wrong password           |
| 3      | 103     | charlie@example.com    | 2025-05-30 08:05:22 | 203.0.113.12     | Safari/13.1 (iPhone; iOS 14.4)    | true    | NULL                     |
| 4      | 104     | diana@example.com      | 2025-05-30 08:07:10 | 10.0.0.8         | Firefox/88.0 (Ubuntu 20.04)       | false   | Account locked           |
| 5      | 105     | ethan@example.com      | 2025-05-30 08:09:35 | 192.168.0.33     | Edge/92.0 (Windows 10)            | true    | NULL                     |
| 6      | 101     | alice@example.com      | 2025-05-30 08:11:50 | 192.168.1.10     | Mozilla/5.0 (Windows NT 10.0)     | false   | Wrong password           |
| 7      | 106     | fiona@example.com      | 2025-05-30 08:15:02 | 198.51.100.25    | Chrome/91.0 (Linux x86_64)        | true    | NULL                     |
| 8      | 107     | george@example.com     | 2025-05-30 08:17:47 | 203.0.113.33     | Safari/14.0 (MacOS)               | false   | Invalid email format     |
| 9      | 108     | hannah@example.com     | 2025-05-30 08:19:12 | 10.10.0.11       | Mozilla/5.0 (Android 11; Mobile)  | true    | NULL                     |
| 10     | 109     | ian@example.com        | 2025-05-30 08:21:30 | 172.16.100.50    | Edge/93.0 (Windows 11)            | false   | Too many attempts        |

