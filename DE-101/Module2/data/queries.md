## 2.2.1 Общий объем продаж (Total Sales)

### [Назад в Модуль 2 ⤶](/DE-101/Module2/readme.md)

```sql
SELECT
    CONCAT('$', ROUND(SUM(sales)))
FROM orders;
  ```
Ответ: $2297201

## 2.2.2 Общая прибыль (Total Profit)

```sql
SELECT
    CONCAT('$', ROUND(SUM(profit)))
FROM orders;
```
Ответ: $286397

## 2.2.3 Коэффициент прибыли (Profit Ratio)

```sql
SELECT
    CONCAT(ROUND(SUM(profit) / (SUM(sales)) * 100), '%')
FROM orders;
```
Ответ: 12%

## 2.2.4 Средняя скидка (Avg. Discount)

```sql
SELECT
    CONCAT(ROUND(AVG(discount) * 100), '%')
FROM orders;
```
Ответ: 16%

## 2.2.5 Продажи и прибыль по годам (Sales and Profit by Year)

```sql
SELECT
    EXTRACT(YEAR FROM order_date) AS year,
    CONCAT('$', ROUND(SUM(sales))) AS sales,
    CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY 1
ORDER BY 1 ASC;
```
Ответ:

| year |  sales  | profit |
|:-----|:-------:|:------:|
| 2016 | $484247 | $49544 |
| 2017 | $470533 | $61619 |
| 2018 | $609206 | $81795 |
| 2019 | $733215 | $93439 |

## 2.2.6 Топ-10 Городов по заказам и продажам (Number Orders and Sales by City)

```sql
SELECT
    city,
    COUNT(DISTINCT order_id) AS number_orders,
    ROUND(SUM(sales)) AS sales
FROM orders
GROUP BY city
ORDER BY 3 DESC
LIMIT 10;
```
Ответ:

| city          | number_orders |  sales  |
|:--------------|:-------------:|:-------:|
| New York City |      450      | 256368  |
| Los Angeles   |      384      | 175851  |
| Seattle       |      212      | 119541  |
| San Francisco |      265      | 112669  |
| Philadelphia  |      265      | 109077  |
| Houston       |      188      |  64505  |
| Chicago       |      171      |  48540  |
| San Diego     |      88       |  47521  |
| Jacksonville  |      61       |  44713  |
| Springfield   |      73       |  43054  |

## 2.2.7 Продажи и прибыль по категориям (Sales and Profit by Category)

```sql
SELECT
    category,
    CONCAT('$', ROUND(SUM(sales))) AS sales,
    CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY category
ORDER BY category DESC;
```
Ответ:

| category         |  sales   | profit  |
|:-----------------|:--------:|:-------:|
| Technology       | $836154  | $145455 |
| Office Supplies  | $719047  | $122491 |
| Furniture        | $742000  | $18451  |

## 2.2.8 Количество продаж по подкатегориям (Count of Sales by Sub-Category)

```sql
SELECT
    subcategory,
    COUNT(sales) AS count
FROM orders
GROUP BY subcategory
ORDER BY 2 DESC;
```
Ответ:

| subcategory | count |
|:------------|:-----:|
| Binders     | 1523  |
| Paper       | 1370  |
| Furnishings |  957  |   
| Phones      |  889  |
| Storage     |  846  |   
| Art         |  796  |
| Accessories |  775  |
| Chairs      |  617  |
| Appliances  |  466  |
| Labels      |  364  |
| Tables      |  319  |
| Envelopes   |  254  |
| Bookcases   |  228  |
| Fasteners   |  217  |
| Supplies    |  190  |
| Machines    |  115  |
| Copiers     |  68   |

## 2.2.9 Региональные менеджеры (Regional Managers)

```sql
SELECT
    p.person AS manager,
    CONCAT('$', ROUND(SUM(sales))) AS sales,
    CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders AS o
INNER JOIN people AS p ON o.region = p.region
GROUP BY p.person
ORDER BY p.person ASC;
```
Ответ:

| manager           |  sales  | profit  |
|:------------------|:-------:|:-------:|
| Anna Andreadi     | $725458 | $108418 |
| Cassandra Brandow | $391722 | $46749  |
| Chuck Magee       | $678781 | $91523  |
| Kelly Williams    | $501240 | $39706  |

## 2.2.10 Продажи и прибыль по сегментам (Sales and Profit by Segment)

```sql
SELECT
    segment,
    CONCAT('$', ROUND(SUM(sales))) AS sales,
    CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY segment
ORDER BY segment ASC;
```
Ответ:

| segment     |  sales   | profit  |
|:------------|:--------:|:-------:|
| Consumer    | $1161401 | $134119 |
| Corporate   | $706146  | $91979  |
| Home Office | $429653  | $60299  |

## 2.2.11 Продажи и прибыль по штатам (Sales and Profit by State)

```sql
SELECT
    state,
    CONCAT('$', ROUND(SUM(sales))) AS sales,
    CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY state
ORDER BY state ASC;
```
Ответ: 

| state                |  sales  | profit  |
|:---------------------|:-------:|:-------:|
| Alabama              | $19511  |  $5787  |
| Arizona              | $35282  | $-3428  |
| Arkansas             | $11678  |  $4009  |
| California           | $457688 | $76381  |
| Colorado             | $32108  | $-6528  |
| Connecticut          | $13384  |  $3511  |
| Delaware             | $27451  |  $9977  |
| District of Columbia |  $2865  |  $1060  |
| Florida              | $89474  | $-3399  |
| Georgia              | $49096  | $16250  |
| Idaho                |  $4382  |  $827   |
| Illinois             | $80166  | $-12608 |
| Indiana              | $53555  | $18383  |
| Iowa                 |  $4580  |  $1184  |
| Kansas               |  $2914  |  $836   |
| Kentucky             | $36592  | $11200  |
| Louisiana            |  $9217  |  $2196  |
| Maine                |  $1271  |  $454   |
| Maryland             | $23706  |  $7031  |
| Massachusetts        | $28634  |  $6786  |
| Michigan             | $76270  | $24463  |
| Minnesota            | $29863  | $10823  |
| Mississippi          | $10771  |  $3173  |
| Missouri             | $22205  |  $6436  |
| Montana              |  $5589  |  $1833  |
| Nebraska             |  $7465  |  $2037  |
| Nevada               | $16729  |  $3317  |
| New Hampshire        |  $7293  |  $1707  |
| New Jersey           | $35764  |  $9773  |
| New Mexico           |  $4784  |  $1157  |
| New York             | $310876 | $74039  |
| North Carolina       | $55603  | $-7491  |
| North Dakota         |  $920   |  $230   |
| Ohio                 | $78258  | $-16971 |
| Oklahoma             | $19683  |  $4854  |
| Oregon               | $17431  | $-1190  |
| Pennsylvania         | $116512 | $-15560 |
| Rhode Island         | $22628  |  $7286  |
| South Carolina       |  $8482  |  $1769  |
| South Dakota         |  $1316  |  $395   |
| Tennessee            | $30662  | $-5342  |
| Texas                | $170188 | $-25729 |
| Utah                 | $11220  |  $2547  |
| Vermont              |  $8929  |  $2245  |
| Virginia             | $70637  | $18598  |
| Washington           | $138641 | $33403  |
| West Virginia        |  $1210  |  $186   |
| Wisconsin            | $32115  |  $8402  |
| Wyoming              |  $1603  |  $100   |

## 2.2.12 Продажи и прибыль по регионам (Sales and Profit by Region)

```sql
SELECT
    region,
    CONCAT('$', ROUND(SUM(sales))) AS sales,
    CONCAT('$', ROUND(SUM(profit))) AS profit
FROM orders
GROUP BY region
ORDER BY region ASC;
```
Ответ:

| region  |  sales  | profit  |
|:--------|:-------:|:-------:|
| Central | $501240 | $39706  |
| East    | $678781 | $91523  |
| South   | $391722 | $46749  |
| West    | $725458 | $108418 |

## 2.2.13 Количество возвратов по годам (Returned by Years)

```sql
SELECT
    EXTRACT(YEAR FROM o.order_date) AS year,
    COUNT(*) AS count_returns
FROM orders AS o
JOIN returns AS r ON o.order_id = r.order_id
GROUP BY 1
ORDER BY 1 ASC;
```
Ответ:

| month |  count_returns  |
|:------|:---------------:|
| 2016  |       599       |
| 2017  |       583       |
| 2018  |       789       |
| 2019  |      1255       |
___