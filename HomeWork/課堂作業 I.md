## Customer - 謝延偵

| customerNo | customerName | customerStreet | customerCity | customerState | customerZipCode | custTelNo | custFaxNo | DOB | maritalStatus | creditRating |
|------------|--------------|----------------|--------------|---------------|-----------------|-----------|-----------|-----|---------------|--------------|
| C001 | 蒲熠星 | 中正路88號 | 斗六市 | 雲林縣 | 640 | 05-5512345 | 05-5512346 | 2000-01-01 | 已婚 | AAA |
| C002 | 郭文韜 | 中正路77號 | 虎尾鎮 | 雲林縣 | 632 | 05-6331234 | 05-6331235 | 2000-01-02 | 未婚 | AA |
| C003 | 石凱 | 中正路66號 | 斗南鎮 | 雲林縣 | 630 | 05-5971234 | 05-5971235 | 2000-01-03 | 已婚 | A |
| C004 | 黃子弘凡 | 中正路55號 | 西螺鎮 | 雲林縣 | 648 | 05-5861234 | 05-5861235 | 2000-01-04 | 未婚 | BBB |
| C005 | 何運晨 | 中正路44號 | 北港鎮 | 雲林縣 | 651 | 05-7831234 | 05-7831235 | 2000-01-05 | 已婚 | BB |

## Employee - 謝延偵

| employeeNo | title | firstName | middleName | lastName | address | workTelExt | homeTelNo | empEmailAddress | socialSecurityNumber | DOB | position | sex | salary | dateStarted |
|------------|-------|-----------|------------|----------|---------|------------|-----------|-----------------|----------------------|-----|----------|-----|--------|-------------|
| E001 | 經理 | 陳 | 志 | 明 | 雲林縣斗六市中正路33號 | 123 | 05-5523456 | chen@example.com | A123456789 | 1980-05-15 | 銷售經理 | 男 | 85000 | 2015-06-01 |
| E002 | 專員 | 林 | 晏 | 慈 | 雲林縣虎尾鎮中正路22號 | 124 | 05-6323456 | lin@example.com | B234567890 | 1988-08-30 | 行政專員 | 女 | 45000 | 2018-03-15 |
| E003 | 主管 | 王 | 建 | 國 | 雲林縣斗南鎮中正路11號 | 125 | 05-5973456 | wang@example.com | C345678901 | 1975-12-15 | 財務主管 | 男 | 65000 | 2010-09-01 |
| E004 | 專員 | 黃 | 美 | 玲 | 雲林縣西螺鎮中正路1號 | 126 | 05-5863456 | huang@example.com | D456789012 | 1990-03-30 | 客服專員 | 女 | 42000 | 2020-01-15 |
| E005 | 技術員 | 李 | 小 | 明 | 雲林縣北港鎮中正路99號 | 127 | 05-7833456 | lee@example.com | E567890123 | 1985-07-15 | IT技術員 | 男 | 50000 | 2017-05-01 |

## Order - 謝延偵

| orderNo | orderDate | deliveryDate | orderStatus | customerNo |
|---------|-----------|--------------|-------------|------------|
| ORD001 | 2025-01-10 | 2025-01-18 | 已完成 | C001 |
| ORD002 | 2025-01-25 | 2025-02-05 | 已完成 | C002 |
| ORD003 | 2025-02-15 | 2025-02-23 | 已完成 | C003 |
| ORD004 | 2025-03-01 | 2025-03-10 | 運送中 | C004 |
| ORD005 | 2025-03-18 | 2025-03-28 | 處理中 | C001 |

## Supplier - 楊峻朋
 
| supplierNo | supplierName   | supplierStreet     | supplierCity | supplierState | supplierZipCode | suppTelNo     | suppFaxNo     | suppEmailAddress         | suppWebAddress           | contactName | contactTelNo  | contactFaxNo  | contactEmailAddress      | paymentTerms |
|------------|----------------|--------------------|--------------|----------------|------------------|----------------|----------------|---------------------------|--------------------------|--------------|----------------|----------------|----------------------------|---------------|
| S001       | Alpha Supplies | 123 Alpha St.      | New York     | NY             | 10001            | 212-555-1001   | 212-555-1002   | alpha@supplies.com        | www.alphasupplies.com    | John Doe     | 212-555-2001   | 212-555-2002   | john.doe@alphasup.com     | Net 30        |
| S002       | Beta Traders   | 456 Beta Ave.      | Los Angeles  | CA             | 90001            | 310-555-2001   | 310-555-2002   | contact@betatraders.com   | www.betatraders.com      | Jane Smith   | 310-555-3001   | 310-555-3002   | jane.smith@beta.com       | Net 45        |
| S003       | Gamma Goods    | 789 Gamma Blvd.    | Chicago      | IL             | 60601            | 312-555-3001   | 312-555-3002   | info@gammagoods.com       | www.gammagoods.com       | Alice Wang   | 312-555-4001   | 312-555-4002   | alice.wang@gamma.com      | COD           |
| S004       | Delta Corp     | 321 Delta Road     | Houston      | TX             | 77001            | 713-555-4001   | 713-555-4002   | support@deltacorp.com     | www.deltacorp.com        | Bob Chen     | 713-555-5001   | 713-555-5002   | bob.chen@delta.com        | Net 60        |
| S005       | Epsilon Global | 987 Epsilon Way    | San Jose     | CA             | 95112            | 408-555-5001   | 408-555-5002   | sales@epsilonglobal.com   | www.epsilonglobal.com    | Emily Davis  | 408-555-6001   | 408-555-6002   | emily.davis@epsilon.com   | Prepaid       |

# Transaction - 楊峻朋

| transactionNo | transactionDate | transactionDescription | unitPrice | unitsOrdered | unitsReceived | unitsSold | unitsWastage | productNo | purchaseOrderNo |
|---------------|------------------|--------------------------|-----------|----------------|----------------|-------------|----------------|------------|------------------|
| T001          | 2024-05-01       | Initial stock delivery   | 10.50     | 100            | 100            | 0           | 0              | P001       | PO001            |
| T002          | 2024-05-02       | Replenishment            | 10.50     | 50             | 50             | 0           | 0              | P001       | PO002            |
| T003          | 2024-05-03       | Sold to client A         | 15.00     | 0              | 0              | 30          | 0              | P001       | PO001            |
| T004          | 2024-05-04       | Damaged goods report     | 0.00      | 0              | 0              | 0           | 5              | P001       | PO002            |
| T005          | 2024-05-05       | Sold to client B         | 15.00     | 0              | 0              | 20          | 0              | P001       | PO002            |

## ProductCategory - 楊峻朋
| categoryNo | categoryDescription     |
|------------|--------------------------|
| C001       | Electronic Components    |
| C002       | Office Supplies          |
| C003       | Packaging Materials      |
| C004       | Safety Equipment         |
| C005       | Cleaning Products        |
