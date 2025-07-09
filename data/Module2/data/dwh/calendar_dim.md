#### CALENDAR DIMENSION

#### [Назад в Модуль 2 ⤶](/data/Module2/readme.md)

- создание таблицы

```sql
DROP TABLE IF EXISTS dwh.calendar_dim;
CREATE TABLE dwh.calendar_dim
(
date_id     serial NOT NULL,
year        int NOT NULL,
quarter     int NOT NULL,
month       int NOT NULL,
week        int NOT NULL,
date        date NOT NULL,
week_day    varchar(20) NOT NULL,
leap        varchar(20) NOT NULL,
CONSTRAINT PK_calendar_dim PRIMARY KEY ( date_id )
);
```

- очистка таблицы

```sql
TRUNCATE TABLE dwh.calendar_dim;
```

- вставка данных

```sql
INSERT INTO dwh.calendar_dim 
SELECT
to_char(date,'yyyymmdd')::int as date_id,  
       EXTRACT('year' FROM date)::int as year,
       EXTRACT('quarter' FROM date)::int as quarter,
       EXTRACT('month' FROM date)::int as month,
       EXTRACT('week' FROM date)::int as week,
       date::date,
       to_char(date, 'dy') as week_day,
       EXTRACT('day' FROM
               (date + interval '2 month - 1 day')
              ) = 29
       as leap
  FROM generate_series(date '2000-01-01',
                       date '2030-01-01',
                       interval '1 day')
       as t(date);
```

- проверка таблицы

```sql
SELECT * FROM dwh.calendar_dim; 
```