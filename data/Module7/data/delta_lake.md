## 7.8.1 Форматы с поддержкой транзакций (Табличные форматы)

### [Назад в Модуль 7 ⤶](/data/Module7/readme.md)

Эти форматы построены поверх **Parquet/ORC** и добавляют слой управления данными, поддерживая ACID-транзакции, 
upserts и удаления в больших данных.  

### Delta Lake
**Delta Lake** – открытый формат, разработанный **Databricks**, который превращает ваши данные в Parquet в надежное 
"озеро данных" (**Data Lakehouse**). Сильно рекомендуется для современных пайплайнов.  

#### Преимущества
- **ACID-транзакции**: Гарантирует согласованность данных при параллельных чтениях/записях.  
- **Upserts и удаления**: Позволяет обновлять и удалять записи с помощью MERGE и DELETE.  
- **Управление версиами (Time Travel)**: Позволяет query предыдущие версии данных.  
- **Управление схемой**: Обеспечивает эволюцию и проверку схемы.  
- Полностью совместим с API Spark.  

```python
# Необходимо установить delta-spark пакет
# Запись
df.write.format("delta").save("/path/to/delta_table")

# Чтение
df = spark.read.format("delta").load("/path/to/delta_table")

# Time Travel к версии 1
# df_v1 = spark.read.format("delta").option("versionAsOf", 1).load("/path/to/delta_table")
```

### Apache Hudi
**Apache Hudi** – конкурирующий формат, похожий на **Delta Lake**, с акцентом на инкрементальные обработки и стриминг.  

#### Преимущества
- Поддержка upserts, удалений и инкрементальных запросов.  
- Различные типы таблиц (Copy-on-Write, Merge-on-Read) для баланса между производительностью записи и чтения.  

```python
# Использование Hudi
hudi_options = {
  'hoodie.table.name': 'my_table',
  'hoodie.datasource.write.recordkey.field': 'id',
  'hoodie.datasource.write.precombine.field': 'timestamp'
}
df.write.format("hudi").options(**hudi_options).mode("overwrite").save("/path/to/hudi_table")
```

### Apache Iceberg
**Apache Iceberg** – открытый табличный формат, набирающий популярность. Сфокусирован на скрытии сложности 
распределенных данных от пользователя.  

#### Преимущества
- Отличная изоляция: запись не блокирует чтение.
- Скрытое партиционирование (Partition Evolution): можно менять схему партиционирования без перезаписи данных.
- Очень гибкая эволюция схемы.  

```python
df.write.format("iceberg").save("catalog.db.table")
df = spark.read.format("iceberg").load("catalog.db.table")
```
