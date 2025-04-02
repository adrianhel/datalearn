```sql
DROP TABLE IF EXISTS calendar_dim;
CREATE TABLE calendar_dim
(
 order_date date NOT NULL,
 ship_date  date NOT NULL,
 year       int4range NOT NULL,
 quarter    varchar(5) NOT NULL,
 month      int4range NOT NULL,
 week       int4range NOT NULL,
 week_day   int4range NOT NULL,
 CONSTRAINT PK_1 PRIMARY KEY ( order_date, ship_date )
);

DROP TABLE IF EXISTS customer_dim;
CREATE TABLE customer_dim
(
 customer_id   serial NOT NULL,
 customer_name varchar(27) NOT NULL,
 segment       varchar(11) NOT NULL,
 CONSTRAINT PK_6 PRIMARY KEY ( customer_id )
);

DROP TABLE IF EXISTS geography_dim;
CREATE TABLE geography_dim
(
 geo_id      serial NOT NULL,
 country     varchar(13) NOT NULL,
 city        varchar(17) NOT NULL,
 "state"     varchar(11) NOT NULL,
 region      varchar(7) NOT NULL,
 postal_code int4range NOT NULL,
 CONSTRAINT PK_3 PRIMARY KEY ( geo_id )
);

DROP TABLE IF EXISTS product_dim;
CREATE TABLE product_dim
(
 product_id   serial NOT NULL,
 category     varchar(15) NOT NULL,
 subcategory  varchar(11) NOT NULL,
 segment      varchar(11) NOT NULL,
 product_name varchar(127) NOT NULL,
 CONSTRAINT PK_5 PRIMARY KEY ( product_id )
);

DROP TABLE IF EXISTS shipping_dim;
CREATE TABLE shipping_dim
(
 ship_id   serial NOT NULL,
 ship_mode varchar(14) NOT NULL,
 CONSTRAINT PK_4 PRIMARY KEY ( ship_id )
);

DROP TABLE IF EXISTS sales_fact;
CREATE TABLE sales_fact
(
 row_id      int4range NOT NULL,
 order_id    varchar(14) NOT NULL,
 sales       numeric(9,4) NOT NULL,
 quantity    int4range NOT NULL,
 discount    numeric(4,2) NOT NULL,
 profit      numeric(21,16) NOT NULL,
 order_date  date NOT NULL,
 ship_date   date NOT NULL,
 ship_id     int NOT NULL,
 geo_id      int NOT NULL,
 product_id  int NOT NULL,
 customer_id serial NOT NULL,
 CONSTRAINT PK_2 PRIMARY KEY ( row_id ),
 CONSTRAINT FK_1 FOREIGN KEY ( order_date, ship_date ) REFERENCES "public".calendar_dim ( order_date, ship_date ),
 CONSTRAINT FK_2 FOREIGN KEY ( ship_id ) REFERENCES "public".shipping_dim ( ship_id ),
 CONSTRAINT FK_3 FOREIGN KEY ( geo_id ) REFERENCES "public".geography_dim ( geo_id ),
 CONSTRAINT FK_4 FOREIGN KEY ( product_id ) REFERENCES "public".product_dim ( product_id ),
 CONSTRAINT FK_5 FOREIGN KEY ( customer_id ) REFERENCES "public".customer_dim ( customer_id )
);

CREATE INDEX FK_1 ON "public".sales_fact
(
 order_date,
 ship_date
);

CREATE INDEX FK_2 ON "public".sales_fact
(
 ship_id
);

CREATE INDEX FK_3 ON "public".sales_fact
(
 geo_id
);

CREATE INDEX FK_4 ON "public".sales_fact
(
 product_id
);
```