## 7.6.1 Структура данных Dataframe

### [Назад в Модуль 7 ⤶](/data/Module7/readme.md)

>**DataFrame** – это распределенная коллекция данных в виде именованных столбцов, аналогично таблице в реляционной 
>базе данных.  

DataFrame работает только со структурированными и полуструктурированными данными, организуя информацию по столбцам, 
как в реляционных таблицах. Это позволяет Spark управлять схемой данных.  

### Пример схемы данных
- В DataFrame схема данных явная и включает имена столбцов и типы данных.  
- Схема может быть автоматически определена при создании DataFrame из источника данных (например, CSV, JSON) или явно 
задана при создании DataFrame.  

```python
from pyspark.sql import SparkSession
from pyspark.sql.types import StructType, StructField, StringType, IntegerType

spark = SparkSession.builder.appName("DataFrame Example").getOrCreate()
data = [("Alice", 1), ("Bob", 2), ("Cathy", 3)]

# Явное определение схемы
schema = StructType([
    StructField("Name", StringType(), True),
    StructField("Value", IntegerType(), True)
])

# Создание DataFrame с явной схемой
df = spark.createDataFrame(data, schema)
df.printSchema()

# Автоматическое определение схемы при чтении данных из CSV
df_auto = spark.read.csv("/path/to/csv/file", header=True, inferSchema=True)
df_auto.printSchema()
```