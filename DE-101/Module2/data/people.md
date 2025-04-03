```sql
--PEOPLE

DROP TABLE IF EXISTS superstore.people;
CREATE TABLE superstore.people(
   Person VARCHAR(17) NOT NULL PRIMARY KEY
  ,Region VARCHAR(7) NOT NULL
);

INSERT INTO superstore.people(Person,Region) 
VALUES ('Anna Andreadi','West'),
       ('Chuck Magee','East'),
       ('Kelly Williams','Central'),
       ('Cassandra Brandow','South');
```