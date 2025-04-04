#### SHIPPING DIMENSION

- создание таблицы

```sql
DROP TABLE IF EXISTS dwh.shipping_dim;
CREATE TABLE dwh.shipping_dim
(
 ship_id       serial NOT NULL,
 shipping_mode varchar(14) NOT NULL,
 CONSTRAINT PK_shipping_dim PRIMARY KEY ( ship_id )
);
```

- очистка таблицы

```sql
TRUNCATE TABLE dwh.shipping_dim;
```

- создание `ship_id` и вставка `ship_mode` из `orders`

```sql
INSERT INTO dwh.shipping_dim 
SELECT 100+ROW_NUMBER() OVER(), ship_mode 
FROM (SELECT distinct ship_mode FROM superstore.orders ) a;
```

- проверка таблицы

```sql
SELECT * FROM dwh.shipping_dim sd;
```