# 2. Базы данных и SQL
## 2.1 Загрузка данных в БД
После установки ***PostgreSQL*** и подключения к БД через ***DBeaver***, приступаем к созданию таблиц и загрузке данных.
##### Создание таблиц:
- _таблица_ `Orders`
```sql
DROP TABLE IF EXISTS orders;
CREATE TABLE orders(Row_ID        INTEGER NOT NULL PRIMARY KEY,
                    Order_ID      VARCHAR(14) NOT NULL,
                    Order_Date    DATE NOT NULL,
                    Ship_Date     DATE NOT NULL,
                    Ship_Mode     VARCHAR(14) NOT NULL,
                    Customer_ID   VARCHAR(8) NOT NULL,
                    Customer_Name VARCHAR(22) NOT NULL,
                    Segment       VARCHAR(11) NOT NULL,
                    Country       VARCHAR(13) NOT NULL,
                    City          VARCHAR(17) NOT NULL,
                    State         VARCHAR(20) NOT NULL,
                    Postal_Code   INTEGER,
                    Region        VARCHAR(7) NOT NULL,
                    Product_ID    VARCHAR(15) NOT NULL,
                    Category      VARCHAR(15) NOT NULL,
                    SubCategory   VARCHAR(11) NOT NULL,
                    Product_Name  VARCHAR(127) NOT NULL,
                    Sales         NUMERIC(9,4) NOT NULL,
                    Quantity      INTEGER NOT NULL,
                    Discount      NUMERIC(4,2) NOT NULL,
                    Profit        NUMERIC(21,16) NOT NULL
);
```
- _таблица_ `People`
```sql
DROP TABLE IF EXISTS people;
CREATE TABLE people(Person VARCHAR(17) NOT NULL PRIMARY KEY,
                    Region VARCHAR(7)  NOT NULL
);
```
- _таблица_ `Returns`
```sql
DROP TABLE IF EXISTS returns;
CREATE TABLE returns(Returned VARCHAR(10) NOT NULL PRIMARY KEY,
                     Order_id VARCHAR(25) NOT NULL
);
```
##### Загрузка данных:
- _для [Orders](https://github.com/adrianhel/datalearn/raw/main/DE-101/Module2/data/orders.sql)_
- _для [People](https://github.com/adrianhel/datalearn/raw/main/DE-101/Module2/data/people.sql)_
- _для [Returns](https://github.com/adrianhel/datalearn/raw/main/DE-101/Module2/data/returns.sql)_

## 2.2 SQL запросы
Условно разделим запросы на три категории:
1. **Ключевые метрики**
  - _Общий объем продаж (Total Sales)_
  - _Общая прибыль (Total Profit)_
  - _Коэффициент прибыли (Profit Ratio)_
  - _Прибыль по заказам (Profit per Order)_
  - _Продажи по клиентам (Sales per Customer)_
  - _Средняя скидка (Avg. Discount)_
  - _Ежемесячные продажи по сегментам (Monthly Sales by Segment)_
  - _Ежемесячные продажи по категориям товаров (Monthly Sales by Product Category)_
2. **Продуктовые метрики**
  - _Продажи по категориям товаров с течением времени (Sales by Product Category over time)_
3. **Анализ клиентов**
  - _Продажи и прибыль по клиентам (Sales and Profit by Customer)_
  - _Рейтинг клиентов (Customer Ranking)_
  - _Продажи по регионам (Sales per region)_
