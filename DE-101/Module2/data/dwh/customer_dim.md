#### CUSTOMER DIMENSION

#### [Назад в Модуль 2 ⤶](/DE-101/Module2/readme.md)

- создание таблицы

```sql
DROP TABLE IF EXISTS dwh.customer_dim;
CREATE TABLE dwh.customer_dim
(
cust_id       serial NOT NULL,
customer_id   varchar(8) NOT NULL, --id не может быть NULL
customer_name varchar(22) NOT NULL,
CONSTRAINT PK_customer_dim PRIMARY KEY ( cust_id )
);
```
- очистка таблицы

```sql
TRUNCATE TABLE dwh.customer_dim;
```
- вставка данных

```sql
INSERT INTO dwh.customer_dim 
SELECT 100+ROW_NUMBER() OVER(), customer_id, customer_name 
FROM (SELECT DISTINCT customer_id, customer_name FROM staging.orders) a;
```

- проверка таблицы

```sql
SELECT * FROM dwh.customer_dim cd; 
```