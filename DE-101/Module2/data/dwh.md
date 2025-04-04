```sql
-- CALENDAR DIMENSION
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

TRUNCATE TABLE dwh.calendar_dim;

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

-- CUSTOMER DIMENSION
DROP TABLE IF EXISTS dwh.customer_dim;
CREATE TABLE dwh.customer_dim
(
cust_id       serial NOT NULL,
customer_id   varchar(8) NOT NULL,
customer_name varchar(22) NOT NULL,
CONSTRAINT PK_customer_dim PRIMARY KEY ( cust_id )
);

TRUNCATE TABLE dwh.customer_dim;

INSERT INTO dwh.customer_dim 
SELECT 100+ROW_NUMBER() OVER(), customer_id, customer_name 
FROM (SELECT DISTINCT customer_id, customer_name FROM superstore.orders ) a;

-- GEOGRAPHY DIMENSION
DROP TABLE IF EXISTS dwh.geo_dim;
CREATE TABLE dwh.geo_dim
(
 geo_id      serial NOT NULL,
 country     varchar(13) NOT NULL,
 city        varchar(17) NOT NULL,
 state       varchar(20) NOT NULL,
 postal_code varchar(20) NULL,
 CONSTRAINT PK_geo_dim PRIMARY KEY ( geo_id )
);

TRUNCATE TABLE dwh.geo_dim;

INSERT INTO dwh.geo_dim 
SELECT 100+ROW_NUMBER() OVER(), country, city, state, postal_code 
FROM (SELECT DISTINCT country, city, state, postal_code FROM superstore.orders ) a;

UPDATE dwh.geo_dim
SET postal_code = '05401'
WHERE city = 'Burlington' AND postal_code IS NULL;

#### PRODUCT DIMENSION
DROP TABLE IF EXISTS dwh.product_dim;
CREATE TABLE dwh.product_dim
(
 prod_id      serial NOT NULL,
 product_id   varchar(50) NOT NULL,
 product_name varchar(127) NOT NULL,
 category     varchar(15) NOT NULL,
 sub_category varchar(11) NOT NULL,
 segment      varchar(11) NOT NULL,
 CONSTRAINT PK_product_dim PRIMARY KEY ( prod_id )
);

TRUNCATE TABLE dwh.product_dim;

INSERT INTO dwh.product_dim 
SELECT 100+ROW_NUMBER() OVER() AS prod_id ,product_id, product_name, category, subcategory, segment 
FROM (SELECT DISTINCT product_id, product_name, category, subcategory, segment FROM superstore.orders ) a;

-- SHIPPING DIMENSION
DROP TABLE IF EXISTS dwh.shipping_dim;
CREATE TABLE dwh.shipping_dim
(
 ship_id       serial NOT NULL,
 shipping_mode varchar(14) NOT NULL,
 CONSTRAINT PK_shipping_dim PRIMARY KEY ( ship_id )
);

TRUNCATE TABLE dwh.shipping_dim;

INSERT INTO dwh.shipping_dim 
SELECT 100+ROW_NUMBER() OVER(), ship_mode 
FROM (SELECT distinct ship_mode FROM superstore.orders ) a;

-- METRICS
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