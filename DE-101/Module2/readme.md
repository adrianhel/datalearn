# 2. Базы данных и SQL
## 2.1 Загрузка данных в БД
После установки ***PostgreSQL*** и подключения к БД через ***DBeaver***, приступаем к созданию таблиц и загрузке данных.
##### Создание таблиц и загрузка данных:
- _Таблица [Orders](data/orders.sql)_
- _Таблица [People](data/people.sql)_
- _Таблица [Returns](data/returns.sql)_

## 2.2 SQL запросы
##### 2.2.1 Общий объем продаж (Total Sales)

```sql
SELECT 
    CONCAT('$', ROUND(SUM(sales)))
FROM orders;
  ```
  
##### 2.2.2 Общая прибыль (Total Profit)

```sql
SELECT 
    CONCAT('$', ROUND(SUM(profit)))
FROM orders;
```

##### 2.2.3 Коэффициент прибыли (Profit Ratio)

```sql
SELECT 
    CONCAT(ROUND(SUM(profit) / (SUM(sales)) * 100), '%')
FROM orders;
```

##### 2.2.4 Средняя скидка (Avg. Discount)

```sql
SELECT 
    CONCAT(ROUND(AVG(discount) * 100), '%')
FROM orders;
```

##### 2.2.5 Продажи и прибыль по годам (Sales and Profit by Year)

```sql
SELECT 
    EXTRACT(YEAR FROM order_date) AS year,
    CONCAT('$', ROUND(SUM(sales))) AS sales,
    CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY 1
ORDER BY 1 ASC;
```

##### 2.2.6 Топ-10 Городов по заказам и продажам (Number Orders and Sales by City)

```sql
SELECT 
    city,
    COUNT(DISTINCT order_id) AS number_orders,
    ROUND(SUM(sales)) AS sales
FROM orders
GROUP BY city
ORDER BY 3 DESC
LIMIT 10;
```

##### 2.2.7 Топ-10 Рейтинга клиентов (Customer Ranking)

```sql
SELECT 
    customer_name,
    ROUND(SUM(sales)) AS sales
FROM orders
GROUP BY customer_name
ORDER BY 2 desc
LIMIT 10;
```

##### 2.2.8 Продажи и прибыль по категориям (Sales and Profit by Category)

```sql
SELECT
    category,
    CONCAT('$', ROUND(SUM(sales))) AS sales,
    CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY category
ORDER BY category ASC;
```

##### 2.2.9 Количество продаж по подкатегориям (Count of Sales by Sub-Category)

```sql
SELECT
    subcategory,
    COUNT(sales) AS count
FROM orders
GROUP BY subcategory
ORDER BY 2 DESC;
```

##### 2.2.10 Региональные менеджеры (Regional Managers)

```sql
SELECT 
    p.person AS manager,
    CONCAT('$', ROUND(SUM(sales))) AS sales,
    CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders AS o
INNER JOIN people AS p ON o.region = p.region
GROUP BY p.person
ORDER BY p.person ASC;
```

##### 2.2.11 Продажи и прибыль по сегментам (Sales and Profit by Segment)

```sql
SELECT 
    segment,
    CONCAT('$', ROUND(SUM(sales))) AS sales,
    CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY segment
ORDER BY segment ASC;
```

##### 2.2.12 Продажи и прибыль по штатам (Sales and Profit by State)

```sql
SELECT 
    state,
    CONCAT('$', ROUND(SUM(sales))) AS sales,
    CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY state
ORDER BY state ASC;
```

##### 2.2.13 Продажи и прибыль по регионам (Sales and Profit by Region)

```sql
SELECT 
    region,
    CONCAT('$', ROUND(SUM(sales))) AS sales,
    CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY region
ORDER BY region ASC;
```

##### 2.2.14 Количество возвратов по месяцам (Returned by Month)

```sql
SELECT	
    EXTRACT(YEAR FROM o.order_date) AS year,
    EXTRACT(MONTH FROM o.order_date) AS month,
    COUNT(*) AS cnt
FROM orders AS o
INNER JOIN returns AS r ON o.order_id = r.order_id
GROUP BY 1, 2
ORDER BY 1, 2 ASC;
```

