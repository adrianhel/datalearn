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



