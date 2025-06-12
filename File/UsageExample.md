# 使用者查詢賣場


![image](https://github.com/user-attachments/assets/09f9278c-3890-41b9-9f52-90426cdf9116)

# 依賣場名稱查詢  
![image](https://github.com/user-attachments/assets/a8328098-005e-4681-9b38-5bf0ca862227)

# 賣家查詢訂單明細
![image](https://github.com/user-attachments/assets/5934b074-0f2f-42b8-9483-a7cca0e64c7d)

# 倉儲人員查詢訂單
![image](https://github.com/user-attachments/assets/a7a84442-2672-4214-9f01-7442e1b32d72)

# 系統管理員查找帳號
![image](https://github.com/user-attachments/assets/09dc1e25-0bf4-4994-bd88-206bc2312322)

# 財務人員查看退款的訂單
![image](https://github.com/user-attachments/assets/087029ba-3859-4299-b4cb-85608f23d4db)

# 行銷人員查詢銷售分析
![image](https://github.com/user-attachments/assets/b4677665-4677-464b-9851-528ba28a0242)

# 客服人員查詢評價
![image](https://github.com/user-attachments/assets/6ac932cd-96f8-46e3-a652-fb57ed551158)

# 賣家查詢自己賣場銷售前三名
```sql
SELECT
    s.seller_id,
    s.store_name,
    ranked_products.product_id,
    ranked_products.product_name,
    ranked_products.total_sales,
    ranked_products.revenue,
    ranked_products.average_price
FROM (
    SELECT
        p.seller_id,
        p.product_id,
        p.product_name,
        sa.total_sales,
        sa.revenue,
        sa.average_price,
        ROW_NUMBER() OVER (PARTITION BY p.seller_id ORDER BY sa.total_sales DESC) AS sales_rank
    FROM seller_products_view AS p
    JOIN sales_analysis AS sa ON p.product_id = sa.product_id AND p.seller_id = 2
    WHERE sa.total_sales > 0
) AS ranked_products
JOIN sellers AS s ON s.seller_id = ranked_products.seller_id
WHERE ranked_products.sales_rank <= 3;
```
![image](https://github.com/user-attachments/assets/5abf9cb8-232b-4616-a554-7dbfa7b51260)



# 賣家依造賣場銷售平均價格前三名排序
```sql
SELECT
    s.seller_id,
    s.store_name,
    ranked_products.product_id,
    ranked_products.product_name,
    ranked_products.total_sales,
    ranked_products.revenue,
    ranked_products.average_price
FROM (
    SELECT
        p.seller_id,
        p.product_id,
        p.product_name,
        sa.total_sales,
        sa.revenue,
        sa.average_price,
        ROW_NUMBER() OVER (PARTITION BY p.seller_id ORDER BY sa.average_price DESC) AS sales_rank
    FROM seller_products_view AS p
    JOIN sales_analysis AS sa ON p.product_id = sa.product_id AND p.seller_id = 2
    WHERE sa.total_sales > 0
) AS ranked_products
JOIN sellers AS s ON s.seller_id = ranked_products.seller_id
WHERE ranked_products.sales_rank <= 3;
```
![image](https://github.com/user-attachments/assets/52ca9213-5414-4f86-b176-463c4e50a267)



# 賣家查找自己庫存小於3 的商品
```sql
SELECT 
    p.product_id,
    p.product_name,
    i.ending_stock,
    p.seller_id
FROM seller_inventory_analytics_view  AS i
JOIN seller_products_view  AS p ON i.product_id = p.product_id
WHERE  p.seller_id = 4 AND
i.ending_stock < 3
ORDER BY i.ending_stock ASC;
```
![image](https://github.com/user-attachments/assets/11001939-0d58-4e38-81d8-0510c513c68f)



