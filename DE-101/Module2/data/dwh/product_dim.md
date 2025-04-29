#### PRODUCT DIMENSION

#### [Назад в Модуль 2 ⤶](/DE-101/Module2/readme.md)

- создание таблицы

```sql
DROP TABLE IF EXISTS dwh.product_dim;
CREATE TABLE dwh.product_dim
(
 prod_id      serial NOT NULL, --мы создаем суррогатный ключ
 product_id   varchar(50) NOT NULL,  --находится в таблице `ORDERS`
 product_name varchar(127) NOT NULL,
 category     varchar(15) NOT NULL,
 sub_category varchar(11) NOT NULL,
 segment      varchar(11) NOT NULL,
 CONSTRAINT PK_product_dim PRIMARY KEY ( prod_id )
);
```

- очистка таблицы

```sql
TRUNCATE TABLE dwh.product_dim;
```

- вставка данных

```sql
INSERT INTO dwh.product_dim 
SELECT 100+ROW_NUMBER() OVER() AS prod_id ,product_id, product_name, category, subcategory, segment 
FROM (SELECT DISTINCT product_id, product_name, category, subcategory, segment FROM staging.orders) a;
```

- проверка таблицы

```sql
SELECT * FROM dwh.product_dim cd;
```