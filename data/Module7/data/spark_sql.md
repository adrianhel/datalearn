## 7.9.1 Spark SQL: обработка структурированных данных

### [Назад в Модуль 7 ⤶](/data/Module7/readme.md)

**Структурированные данные** — данные, обладающие чёткой схемой (schema), определяющей типы и имена столбцов 
(таблицы баз данных, CSV-файлы, Parquet-файлы).  
**Schema** — описание структуры данных, включающее типы и имена столбцов.  
**DataFrame** — распределённая коллекция данных, организованных в виде именованных столбцов, аналогичная таблице 
в реляционной базе данных. DataFrame реализует «ленивые» вычисления и поддерживает множество оптимизаций.  
**Dataset** — типизированная абстракция, предоставляющая статическую типизацию и мощные средства трансформации 
данных (доступна в Scala и Java).  
**SQLContext** / **SparkSession** — точка входа для работы с Spark SQL. С версии Spark 2.0 основной 
интерфейс — `SparkSession`.  

## 7.9.2 Архитектура и принципы работы Spark SQL
Spark SQL поддерживает два основных способа работы с данными:  
1. Выполнение запросов на языке SQL  
2. Манипулирование данными с помощью API DataFrame/Dataset  

Ядро Spark SQL реализует оптимизатор запросов **Catalyst**, который автоматически анализирует, оптимизирует и преобразует 
планы выполнения запросов для достижения максимальной производительности. Для хранения и передачи данных используется 
оптимизированный формат **Tungsten**.  

### Поток выполнения запроса
1. Разбор (Parsing): SQL-запрос преобразуется в логический план.  
2. Анализ (Analysis): проверка схемы, синтаксиса и разрешение имён столбцов.  
3. Оптимизация (Optimization): применение правил **Catalyst** для преобразования логического плана в более эффективный.  
4. Планирование (Planning): генерация физического плана выполнения.  
5. Выполнение (Execution): выполнение физического плана на кластере Spark.  

## 7.9.3 Работа с DataFrame
DataFrame — основной объект Spark SQL для представления структурированных данных. Его можно создать из различных 
источников данных: файлов (JSON, Parquet, ORC, CSV), баз данных, RDD и других.  

#### Пример создания SparkSession

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("SparkSQLExample") \
    .getOrCreate()
```
                  
#### Загрузка данных в DataFrame

```python
# Загрузка данных из CSV-файла
df = spark.read.csv("path/to/data.csv", header=True, inferSchema=True)

# Загрузка данных из Parquet-файла
df_parquet = spark.read.parquet("path/to/data.parquet")
```
                  
### Основные операции
- **Выборка столбцов:**

    ```python
    df.select("name", "age")
    ```
                  
- **Фильтрация строк:**

    ```python
    df.filter(df.age > 21)
    ```
                  
- **Группировка и агрегирование:**

    ```python
    df.groupBy("city").agg({"salary": "avg"})
    ```
                  
- **Соединения (joins):**

    ```python
    df1.join(df2, df1.id == df2.id, "inner")
    ```
                  
- **Сортировка:**

    ```python
    df.orderBy(df.age.desc())
    ```

## 7.9.4 Работа с SQL-запросами
Spark SQL позволяет выполнять SQL-запросы к DataFrame, регистрируя их как временные (**temporary**) 
или постоянные (**global temporary**) представления (**views**).  

```python
# Регистрация DataFrame как временного представления
df.createOrReplaceTempView("people")

# Выполнение SQL-запроса
result = spark.sql("SELECT name, COUNT(*) FROM people GROUP BY name")
```
                  
В глобальном контексте временные представления доступны только в рамках одной сессии, глобальные временные — 
во всех сессиях Spark.  

## 7.9.5 Dataset API
Dataset — типизированная абстракция для работы с данными (доступна на Scala и Java), сочетающая преимущества RDD 
(безопасность типов, высокоуровневая трансформация) и DataFrame (оптимизация, ленивое выполнение).  

```scala
// Пример на Scala
case class Person(name: String, age: Int)
val ds = spark.read.json("people.json").as[Person]
ds.filter(_.age > 18).show()
```
                  
## 7.9.6 Оптимизация запросов: Catalyst и Tungsten
- **Оптимизатор Catalyst** — фреймворк для анализа и преобразования запросов, реализующий правила переписывания 
выражений, оптимизации логических и физических планов.  

- **Tungsten** — компонент для эффективного управления памятью и выполнения вычислений на низком уровне, что позволяет 
повысить производительность Spark SQL.  

## 7.9.7 Поддерживаемые форматы и источники данных
- CSV, JSON, Parquet, ORC, Avro
- JDBC (реляционные базы данных)
- Hive (через HiveContext / SparkSession с Hive Support)
- Delta Lake и другие форматы данных для управления транзакциями

#### Примеры загрузки различных форматов

```python
# Загрузка данных из JSON
df_json = spark.read.json("data.json")

# Загрузка из реляционной БД через JDBC
df_jdbc = spark.read \
    .format("jdbc") \
    .option("url", "jdbc:postgresql://host:port/db") \
    .option("dbtable", "schema.tablename") \
    .option("user", "username") \
    .option("password", "password") \
    .load()
```
                  
## 7.9.8 Интеграция с Hive
Spark SQL поддерживает интеграцию с Apache Hive, включая чтение и запись данных из таблиц Hive, использование 
Hive UDF и выполнение запросов HiveQL.  

```python
# Включение поддержки Hive
spark = SparkSession.builder \
    .appName("HiveExample") \
    .enableHiveSupport() \
    .getOrCreate()

df_hive = spark.sql("SELECT * FROM hive_table")
```
                  
## 7.9.9 Управление схемой и эволюция схемы
- **Инференция схемы** — автоматическое определение структуры данных при загрузке (например, `inferSchema=True`).  
- **Явное задание схемы** — определение схемы с помощью `StructType` и `StructField`.  

```python
from pyspark.sql.types import StructType, StructField, StringType, IntegerType

schema = StructType([
    StructField("name", StringType(), True),
    StructField("age", IntegerType(), True)
])

df = spark.read.csv("data.csv", schema=schema, header=True)
```

Spark SQL поддерживает эволюцию схемы (schema evolution) при работе с некоторыми форматами данных, например, Parquet.  

## 7.9.10 Преобразования и действия (Transformations & Actions)
- **Преобразования (Transformations):** операции, возвращающие новый DataFrame (например, `select`, `filter`, 
`groupBy`, `join`).  
- **Действия (Actions):** операции, инициирующие вычисление (например, `show`, `collect`, `write`).  

#### Пример преобразований и действий

```python
df_filtered = df.filter(df["age"] > 18)
df_grouped = df_filtered.groupBy("city").count()
df_grouped.show()
```

#### Пользовательские функции (UDF)
Spark SQL поддерживает определение и использование пользовательских функций (User Defined Functions, UDF) для обработки данных, выходящих за рамки стандартных функций.

```python
from pyspark.sql.functions import udf
from pyspark.sql.types import IntegerType

def add_ten(x):
    return x + 10

add_ten_udf = udf(add_ten, IntegerType())
df.withColumn("age_plus_10", add_ten_udf(df.age)).show()
```
  
## 7.9.11 Запись данных
- **Поддержка различных форматов:** CSV, JSON, Parquet, ORC, Avro, JDBC и др.  
- **Управление режимами записи:** `overwrite`, `append`, `ignore`, `errorIfExists`.  

```python
# Запись в Parquet
df.write.mode("overwrite").parquet("output/path")

# Запись в таблицу Hive
df.write.saveAsTable("hive_table")
```
                  
## 7.9.12 Практические применения Spark SQL
- Интерактивная аналитика больших объёмов данных  
- ETL-процессы (извлечение, преобразование, загрузка данных)  
- Интеграция с BI-инструментами через JDBC/ODBC  
- Агрегация и анализ логов, событий, потоковых данных  
- Обработка данных в формате Data Lake  
- Сложный анализ данных с использованием SQL и машинного обучения (через Spark MLlib)  

## 7.9.13 Преимущества Spark SQL
- Масштабируемость и высокая производительность  
- Оптимизация запросов с помощью Catalyst и Tungsten  
- Интеграция с разнообразными источниками и форматами данных  
- Единая точка входа для работы с данными на разных языках программирования  
- Простота перехода между SQL и API DataFrame/Dataset  
- Поддержка пользовательских функций и расширяемость  
