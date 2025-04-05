#### GEOGRAPHY DIMENSION

#### [Назад в Модуль 2 ⤶](/DE-101/Module2/readme.md)

- создание таблицы

```sql
DROP TABLE IF EXISTS dwh.geo_dim;
CREATE TABLE dwh.geo_dim
(
 geo_id      serial NOT NULL,
 country     varchar(13) NOT NULL,
 city        varchar(17) NOT NULL,
 state       varchar(20) NOT NULL,
 postal_code varchar(20) NULL,       --не может быть integer, мы теряем первый 0
 CONSTRAINT PK_geo_dim PRIMARY KEY ( geo_id )
);
```

- очистка таблицы

```sql
TRUNCATE TABLE dwh.geo_dim;
```

- генерация `geo_id` и вставка из `orders`

```sql
INSERT INTO dwh.geo_dim 
SELECT 100+ROW_NUMBER() OVER(), country, city, state, postal_code 
FROM (SELECT DISTINCT country, city, state, postal_code FROM staging.orders ) a;
```

- проверка качества данных

```sql
SELECT DISTINCT country, city, state, postal_code FROM dwh.geo_dim
WHERE country IS NULL OR city IS NULL OR postal_code IS NULL;
```

- город Burlington, Vermont не имеет почтового индекса, добавляем его

```sql
UPDATE dwh.geo_dim
SET postal_code = '05401'
WHERE city = 'Burlington' AND postal_code IS NULL;
```

- также обновляем файл источника данных

```sql
UPDATE superstore.orders
SET postal_code = '05401'
WHERE city = 'Burlington' AND postal_code IS NULL;
```

- проверим город Burlington

```sql
SELECT * FROM dwh.geo_dim
WHERE city = 'Burlington';
```