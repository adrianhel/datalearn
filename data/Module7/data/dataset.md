## 7.6.1 Структура данных Dataset

### [Назад в Модуль 7 ⤶](/data/Module7/readme.md)

>**DataSet** – это расширение API DataFrame, обеспечивающее функциональность объектно-ориентированного RDD-API 
>(строгая типизация, лямбда-функции), производительность оптимизатора запросов Catalyst и механизм хранения вне кучи.  

DataSet эффективно обрабатывает структурированные и неструктурированные данные, представляя их в виде строки 
JVM-объектов или коллекции. Для представления табличных форм используется кодировщик (encoder).

### Пример схемы данных
В Dataset схема данных явная и определяется структурой типизированных объектов (классов), хранящихся в Dataset.
Dataset сочетает в себе преимущества RDD (типизированный API) и DataFrame (явная схема и оптимизация).  

```scala
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.types._

val spark = SparkSession.builder.appName("Dataset Example").getOrCreate()
import spark.implicits._

case class Person(name: String, age: Int)
val data = Seq(Person("Alice", 1), Person("Bob", 2), Person("Cathy", 3))
val ds = data.toDS()

// Схема данных определяется структурой класса Person
ds.printSchema()
```