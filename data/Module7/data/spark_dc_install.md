## 7.2 Установка PySpark c помощью Docker

[![Docker](https://img.shields.io/badge/docker_desktop-4.43.2-blue?logo=docker)](https://www.docker.com/)
[![Spark](https://img.shields.io/badge/apache_spark-3.3.0-blue?logo=apache)](https://spark.apache.org/downloads.html)

### [Назад в Модуль 7 ⤶](/data/Module7/readme.md)

**Docker Desktop** должен быть установлен.

1. Перейдите в папку проекта и запустите файл **Dockerfile**:

```bash
cd C:\Users\andy\*\datalearn\data\Module7\data

docker build -t pyspark .
```

2. Дождитесь окончания установки и перейдите в **Docker Desktop** в раздел **Images**.  
Нажмите на кнопку **Run**, появится окошко с настройками.  

Разверните **Optional settings** и пропишите порты:  
- 4040 для :4040/tcp;  
- 8888 для :8888/tcp;  
  
Это разовая настройка (делается при первом старте).  
Теперь нажмите **Run**.  
Вас перекинет в  раздел **Containers** и откроется лог контейнера.  

3. Контейнер запустился, а внутри него запустился **Jupyter Notebook** + **Pyspark**. 
Перейдите по адресу http://localhost:8888/ .

Если у вас открылся **Jupyter Notebook** — отлично, почти все готово. 

4. Проверим, что **PySpark** тоже запускается. Создайте ноутбук и выполните следующий код:

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("MyApp") \
    .getOrCreate()

print("Spark version:", spark.version)
```

На экране могут появиться предупреждения, а затем вы должны увидеть версию **Pyspark**:

<img src="/data/Module7/img/spark_version_2.png" width="80%">  

Если всё так — установка прошла успешно! 

5. После этого проверьте, что **Spark UI** тоже открывается http://localhost:4040/ :

<img src="/data/Module7/img/spark_ui_2.png" width="40%">

Установка завершена успешно.

6. Повторный запуск:
- Запустите **Docker Desktop**;
- Перейдите на вкладку **Containers** и нажмите кнопку **Start**.