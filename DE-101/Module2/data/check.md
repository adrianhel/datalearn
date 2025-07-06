##### Проверка нашего датасета

### [Назад в Модуль 2 ⤶](/DE-101/Module2/readme.md)

- получилось 9994 строк?

```sql
SELECT COUNT(*) FROM dwh.sales_fact sf
INNER JOIN dwh.shipping_dim s on sf.ship_id = s.ship_id
INNER JOIN dwh.geo_dim g on sf.geo_id = g.geo_id
INNER JOIN dwh.product_dim p on sf.prod_id = p.prod_id
INNER JOIN dwh.customer_dim cd on sf.cust_id = cd.cust_id;
```