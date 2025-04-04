### PEOPLE

#### [Назад в модуль 2 ⤶](/DE-101/Module2/readme.md)

- создание таблицы
```sql
DROP TABLE IF EXISTS superstore.people;
CREATE TABLE superstore.people(
   Person VARCHAR(17) NOT NULL PRIMARY KEY
  ,Region VARCHAR(7) NOT NULL
);
```
- вставка данных

```sql
INSERT INTO superstore.people(Person,Region) 
VALUES ('Anna Andreadi','West'),
       ('Chuck Magee','East'),
       ('Kelly Williams','Central'),
       ('Cassandra Brandow','South');
```