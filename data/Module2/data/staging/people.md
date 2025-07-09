### PEOPLE

#### [Назад в Модуль 2 ⤶](/data/Module2/readme.md)

- создание таблицы

```sql
DROP TABLE IF EXISTS staging.people;
CREATE TABLE staging.people(
   Person VARCHAR(17) NOT NULL PRIMARY KEY
  ,Region VARCHAR(7) NOT NULL
);
```
- вставка данных

```sql
INSERT INTO staging.people(Person,Region) 
VALUES ('Anna Andreadi','West'),
       ('Chuck Magee','East'),
       ('Kelly Williams','Central'),
       ('Cassandra Brandow','South');
```