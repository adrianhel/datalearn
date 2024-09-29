# 2. Базы данных и SQL
## 2.1 Загрузка данных в БД
После установки ***PostgreSQL*** и подключения к БД через ***DBeaver***, приступаем к созданию таблиц и загрузке данных.
##### Создание таблиц и загрузка данных:
- _Таблица [Orders](https://github.com/adrianhel/datalearn/raw/main/DE-101/Module2/data/orders.sql)_
- _Таблица [People](https://github.com/adrianhel/datalearn/raw/main/DE-101/Module2/data/people.sql)_
- _Таблица [Returns](https://github.com/adrianhel/datalearn/raw/main/DE-101/Module2/data/returns.sql)_

## 2.2 SQL запросы
##### Общий объем продаж (Total Sales)

```sql
SELECT 
	CONCAT('$', ROUND(SUM(sales)))
FROM orders;
  ```
  
##### Общая прибыль (Total Profit)

```sql
SELECT 
	CONCAT('$', ROUND(SUM(profit)))
FROM orders;
```

##### Коэффициент прибыли (Profit Ratio)

```sql
SELECT 
	CONCAT(ROUND(SUM(profit) / (SUM(sales)) * 100), '%')
FROM orders;
```

##### Средняя скидка (Avg. Discount)

```sql
SELECT 
	CONCAT(ROUND(AVG(discount) * 100), '%')
FROM orders;
```

##### Продажи и прибыль по годам (Sales and Profit by Year)

```sql
SELECT 
	EXTRACT(YEAR FROM order_date) AS year,
	CONCAT('$', ROUND(SUM(sales))) AS sales,
	CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY 1
ORDER BY 1 ASC;
```

##### Топ-10 городов по продажам и прибыли (Number Orders and Sales by City)

```sql
SELECT 
	city,
	COUNT(DISTINCT order_id) as number_orders,
	ROUND(SUM(sales)) AS sales
FROM orders
GROUP BY city
ORDER BY 3 DESC
LIMIT 10;
```

##### Продажи и прибыль по категориям (Sales and Profit by Category)

```sql
SELECT
	category,
	CONCAT('$', ROUND(SUM(sales))) AS sales,
	CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY category
ORDER BY category ASC;
```

##### Количество продаж по подкатегориям (Count of Sales by Sub-Category)

```sql
SELECT
	subcategory,
	COUNT(sales) AS count
FROM orders
GROUP BY subcategory
ORDER BY 2 DESC;
```

##### Региональные менеджеры (Regional Managers)

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

##### Продажи и прибыль по сегментам (Sales and Profit by Segment)

```sql
SELECT 
	segment,
	CONCAT('$', ROUND(SUM(sales))) AS sales,
	CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY segment
ORDER BY segment ASC;
```

##### Продажи и прибыль по штатам (Sales and Profit by State)

```sql
SELECT 
	state,
	CONCAT('$', ROUND(SUM(sales))) AS sales,
	CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY state
ORDER BY state ASC;
```

##### Продажи и прибыль по регионам (Sales and Profit by Region)

```sql
SELECT 
	region,
	CONCAT('$', ROUND(SUM(sales))) AS sales,
	CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY region
ORDER BY region ASC;
```




