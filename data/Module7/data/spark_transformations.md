## 7.7.1 Преобразования (Transformations) в Apache Spark

### [Назад в Модуль 7 ⤶](/data/Module7/readme.md)

- Преобразования создают новый RDD (или DataFrame/Dataset) на основе существующего, но не выполняют вычислений немедленно. 
Они возвращают новый объект (RDD или DataFrame), который описывает результат трансформации.  
- Преобразования являются ленивыми и только определяют, как должны быть преобразованы данные, без фактического 
выполнения операций.  

## 7.7.2 Классификация трансформаций
- **Нефильтрующие (map-подобные)** — преобразуют каждое отдельное значение или группу значений без 
изменения структуры разбиения:  
    - **map**    
    - **flatMap**    
    - **mapPartitions**   
    - **mapPartitionsWithIndex**  

- **Фильтрующие** — отбирают часть данных на основе заданного условия:  
    - **filter**  
    - **distinct**  
    - **sample**  

- **Агрегирующие и группирующие** — агрегируют, объединяют или группируют данные по ключу:  
    - **groupByKey**   
    - **reduceByKey**  
    - **aggregateByKey**  
    - **combineByKey**  
    - **groupBy**  

- **Сортирующие** — изменяют порядок элементов:  
    - **sortBy**  
    - **sortByKey**  

- **Объединяющие и соединяющие** — работают с несколькими RDD:  
    - **union**  
    - **intersection**  
    - **subtract**  
    - **cartesian**  
    - **join**  
    - **leftOuterJoin**  
    - **rightOuterJoin**  
    - **fullOuterJoin**  
    - **cogroup**  

## 7.7.3 Что происходит при вызове трансформации?
Ничего, кроме обновления внутреннего DAG.

```python
lines_rdd = sc.textFile("data.txt")  # Еще ничего не прочитано
words_rdd = lines_rdd.flatMap(lambda line: line.split(" ")) # Ничего не выполнено
filtered_rdd = words_rdd.filter(lambda word: word.startswith("a")) # Все еще ничего
```

На этом этапе у Spark есть план: `Прочитать файл -> разбить на слова -> отфильтровать слова на 'a'`.  

## 7.7.4 Основные трансформации и примеры
### map
Применяет заданную функцию к каждому элементу RDD, возвращая новый RDD с результатами преобразования.  

```scala
val rdd = sc.parallelize(Seq(1, 2, 3, 4))
val squared = rdd.map(x => x * x) // (1, 4, 9, 16)
```
                  
### flatMap
Применяет функцию к каждому элементу и "разворачивает" полученные коллекции в один плоский список.  

```scala
val rdd = sc.parallelize(Seq("hello world", "spark rdd"))
val words = rdd.flatMap(line => line.split(" ")) // ("hello", "world", "spark", "rdd")
```
                  
### filter
Отбирает только те элементы, для которых функция возвращает true.  

```scala
val rdd = sc.parallelize(Seq(1, 2, 3, 4, 5))
val even = rdd.filter(x => x % 2 == 0) // (2, 4)
```
                  
### distinct
Возвращает новый RDD, содержащий только уникальные элементы исходного RDD.  

```scala
val rdd = sc.parallelize(Seq(1, 2, 2, 3, 3, 3))
val distinctRdd = rdd.distinct() // (1, 2, 3)
```
                  
### sample
Генерирует случайную выборку элементов с или без возвращения.  

```scala
val rdd = sc.parallelize(1 to 100)
val sampleRdd = rdd.sample(false, 0.1, 42) // ~10 случайных чисел
```
                  
### union
Объединяет элементы двух RDD, создавая новый RDD, содержащий все элементы обоих входных RDD.  

```scala
val rdd1 = sc.parallelize(Seq(1, 2))
val rdd2 = sc.parallelize(Seq(3, 4))
val unionRdd = rdd1.union(rdd2) // (1, 2, 3, 4)
```
                  
### intersection
Возвращает RDD, содержащий элементы, присутствующие в обоих RDD.  

```scala
val rdd1 = sc.parallelize(Seq(1, 2, 3))
val rdd2 = sc.parallelize(Seq(2, 3, 4))
val interRdd = rdd1.intersection(rdd2) // (2, 3)
```
                  
### subtract
Возвращает элементы из первого RDD, которых нет во втором.  

```scala
val rdd1 = sc.parallelize(Seq(1, 2, 3))
val rdd2 = sc.parallelize(Seq(2, 3, 4))
val subRdd = rdd1.subtract(rdd2) // (1)
```
                  
### cartesian
Возвращает декартово произведение двух RDD.  

```scala
val rdd1 = sc.parallelize(Seq(1, 2))
val rdd2 = sc.parallelize(Seq("a", "b"))
val cartRdd = rdd1.cartesian(rdd2) // ((1,"a"), (1,"b"), (2,"a"), (2,"b"))
```
                  
### groupBy
Группирует элементы по значению, возвращаемому функцией.  

```scala
val rdd = sc.parallelize(Seq("apple", "banana", "carrot", "avocado"))
val grouped = rdd.groupBy(_.charAt(0))
// ('a', [apple, avocado]), ('b', [banana]), ('c', [carrot])
```
                  
### groupByKey
Для RDD пар (ключ, значение) группирует значения по ключу.  

```scala
val rdd = sc.parallelize(Seq(("a", 1), ("b", 2), ("a", 3)))
val grouped = rdd.groupByKey()
// ("a", [1, 3]), ("b", [2])
```
                  
### reduceByKey
Для RDD пар (ключ, значение) агрегирует значения с одинаковым ключом с помощью заданной функции.  

```scala
val rdd = sc.parallelize(Seq(("a", 1), ("b", 2), ("a", 3)))
val reduced = rdd.reduceByKey(_ + _)
// ("a", 4), ("b", 2)
```
                  
### aggregateByKey
Позволяет задать начальное значение и две отдельные функции: одну для агрегации внутри партиции, другую — между партициями.  

```scala
val rdd = sc.parallelize(Seq(("a", 1), ("b", 2), ("a", 3)))
val aggregated = rdd.aggregateByKey(0)(_ + _, _ + _)
// ("a", 4), ("b", 2)
```
                  
### combineByKey
Универсальная трансформация для агрегации значений по ключу с использованием трёх функций:  
- **createCombiner** — преобразует значение в первый элемент комбайнера  
- **mergeValue** — добавляет новое значение в существующий комбайнер  
- **mergeCombiners** — объединяет два комбайнера  

```scala
val rdd = sc.parallelize(Seq(("a", 1), ("b", 2), ("a", 3)))
val combined = rdd.combineByKey(
  (v: Int) => (v, 1),
  (acc: (Int, Int), v: Int) => (acc._1 + v, acc._2 + 1),
  (acc1: (Int, Int), acc2: (Int, Int)) => (acc1._1 + acc2._1, acc1._2 + acc2._2)
)
// ("a", (4,2)), ("b", (2,1))
```
                  
### sortBy
Сортирует элементы по заданной функции.  

```scala
val rdd = sc.parallelize(Seq(5, 2, 8, 1))
val sorted = rdd.sortBy(x => x) // (1, 2, 5, 8)
```
                  
### sortByKey
Для RDD пар (ключ, значение) сортирует по ключу.  

```scala
val rdd = sc.parallelize(Seq((3, "c"), (1, "a"), (2, "b")))
val sorted = rdd.sortByKey() // (1, "a"), (2, "b"), (3, "c")
```
                  
### join, leftOuterJoin, rightOuterJoin, fullOuterJoin
Операции соединения позволяют объединять элементы двух RDD по ключу.  

```scala
val rdd1 = sc.parallelize(Seq((1, "a"), (2, "b")))
val rdd2 = sc.parallelize(Seq((1, "x"), (3, "y")))
val joinRdd = rdd1.join(rdd2) // (1, ("a", "x"))
val leftRdd = rdd1.leftOuterJoin(rdd2) // (1, ("a", Some("x"))), (2, ("b", None))
val rightRdd = rdd1.rightOuterJoin(rdd2) // (1, (Some("a"), "x")), (3, (None, "y"))
val fullRdd = rdd1.fullOuterJoin(rdd2) // (1, (Some("a"), Some("x"))), (2, (Some("b"), None)), (3, (None, Some("y")))
```
                  
### mapPartitions
Позволяет применять функцию к каждой партиции (разделу) RDD, что может быть эффективнее, если функция работает с 
несколькими элементами сразу.  

```scala
val rdd = sc.parallelize(1 to 10, 2)
val mapped = rdd.mapPartitions(iter => Iterator(iter.sum))
// на выходе: сумма по каждой партиции
```
                  
### mapPartitionsWithIndex
Позволяет применять функцию к каждой партиции с номером партиции.  

```scala
val rdd = sc.parallelize(1 to 5, 2)
val mapped = rdd.mapPartitionsWithIndex{
  case (index, iter) => iter.map(x => (index, x))
}
// элементы будут помечены номером партиции
```
                  
### coalesce, repartition
Изменяют число партиций RDD. **coalesce** уменьшает число партиций (с возможностью перераспределения), 
**repartition** — увеличивает или уменьшает число партиций с полной перераспределением данных.  

```scala
val rdd = sc.parallelize(1 to 100, 10)
val coalesced = rdd.coalesce(2)
val repartitioned = rdd.repartition(20)
```

## 7.7.5 Особенности и принципы трансформаций
- **Ленивость вычислений (Lazy evaluation)**: трансформации не выполняются сразу, а формируют план вычислений (DAG), 
который исполняется при вызове действия.  
- **Непрерывность**: результат трансформации — новый RDD, который можно подвергать дальнейшим операциям.  
- **Стойкость к сбоям**: благодаря **lineage** (цепочке преобразований), Spark может восстановить данные после сбоя.  
- **Детерминированность**: применение одной и той же трансформации к одинаковому исходному RDD всегда даёт одинаковый 
результат.  