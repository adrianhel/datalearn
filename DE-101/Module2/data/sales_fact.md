#### METRICS

#### [Назад в модуль 2 ⤶](/DE-101/Module2/readme.md)

- создание таблицы

```sql
DROP TABLE IF EXISTS dwh.sales_fact ;
CREATE TABLE dwh.sales_fact
(
 sales_id      serial NOT NULL,
 cust_id       integer NOT NULL,
 order_date_id integer NOT NULL,
 ship_date_id  integer NOT NULL,
 prod_id       integer NOT NULL,
 ship_id       integer NOT NULL,
 geo_id        integer NOT NULL,
 order_id      varchar(25) NOT NULL,
 sales         numeric(9,4) NOT NULL,
 profit        numeric(21,16) NOT NULL,
 quantity      int4 NOT NULL,
 discount      numeric(4,2) NOT NULL,
 CONSTRAINT PK_sales_fact PRIMARY KEY ( sales_id ));
 ```

- вставка данных

```sql
INSERT INTO dwh.sales_fact 
SELECT
	 100+ROW_NUMBER() OVER() AS sales_id
	 ,cust_id
	 ,to_char(order_date,'yyyymmdd')::int AS order_date_id
	 ,to_char(ship_date,'yyyymmdd')::int AS ship_date_id
	 ,p.prod_id
	 ,s.ship_id
	 ,geo_id
	 ,o.order_id
	 ,sales
	 ,profit
	 ,quantity
	 ,discount
FROM superstore.orders o 
INNER JOIN dwh.shipping_dim s ON o.ship_mode = s.shipping_mode
INNER JOIN dwh.geo_dim g ON o.postal_code = g.postal_code::INTEGER AND o.country = g.country AND o.city = g.city 
    AND o.state = g.state
INNER JOIN dwh.product_dim p ON o.product_name = p.product_name AND o.segment = p.segment 
    AND o.subcategory = p.sub_category AND o.category = p.category AND o.product_id = p.product_id 
INNER JOIN dwh.customer_dim cd ON cd.customer_id = o.customer_id AND cd.customer_name=o.customer_name 
```

- Проверка нашего датасета, получилось 9994 строк?

```sql
SELECT COUNT(*) FROM dwh.sales_fact sf
INNER JOIN dwh.shipping_dim s on sf.ship_id = s.ship_id
INNER JOIN dwh.geo_dim g on sf.geo_id = g.geo_id
INNER JOIN dwh.product_dim p on sf.prod_id = p.prod_id
INNER JOIN dwh.customer_dim cd on sf.cust_id = cd.cust_id;
```