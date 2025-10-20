## 7.8.1 Apache ORC (Optimized Row Columnar)

### [Назад в Модуль 7 ⤶](/data/Module7/readme.md)

**Apache ORC** (Optimized Row Columnar) – популярный колоночный формат, конкурирующий с **Parquet**. Часто ассоциируется 
с экосистемой **Hive**.  

#### Преимущества
- Очень похож на **Parquet** по производительности и возможностям.
- В некоторых бенчмарках может показывать лучшую производительность для определенных типов запросов в **Hive**.  

#### Недостатки
- Менее популярен в Spark-экосистеме по сравнению с **Parquet**.  

```python
df.write.orc("/path/to/data.orc")
df = spark.read.orc("/path/to/data.orc")
```
