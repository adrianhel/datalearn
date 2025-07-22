## 7.2 Установка Apache Spark (docker)

[![Docker](https://img.shields.io/badge/docker_desktop-4.43.2-blue?logo=docker)](https://www.docker.com/)
[![Spark](https://img.shields.io/badge/apache_spark-3.3.0-blue?logo=apache)](https://spark.apache.org/downloads.html)

### [Назад в Модуль 7 ⤶](/data/Module7/readme.md)

**Docker Desktop** должен быть установлен.

1. Перейдите в папку проекта и запустите файл "Dockerfile":

```bash
cd C:\Users\andy\*\datalearn\data\Module7\data

docker build -t pyspark .
```

2. Дождитесь окончания установки, а затем перейдите **Docker Desktop** в раздел **Images**.  

Нажмите на кнопку **Run**, затем появится окошко с настройками.  

Разверните **Optional settings** и пропишите порты:  
- 4040 для :4040/tcp  
- 8888 для :8888/tcp  

Это разовая настройка, которая делается при первом старте. Теперь нажмите Run.

3. Вас перекинет в  раздел Containers и откроется лог контейнера.

Отлично, наш контейнер запустился, а внутри него запустился наш **Jupyter Notebook** + **Pyspark**. 
Теперь можно попробовать перейти по адресу http://localhost:8888/ 

Если у вас открылся Jupyter Notebook - отлично, почти все готово. 

4. Проверим, что PySpark тоже запускается. Создайте ноутбук и выполните следующий код:

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("MyApp") \
    .getOrCreate()

print("Spark version:", spark.version)
```

На экране могут появиться предупреждения (это нормально), а затем вы должны увидеть версию Pyspark:

<img src="/data/Module7/img/spark_version_2.png" width="80%">  

Если всё так - установка прошла успешно! 

5. После этого проверьте, что Spark UI тоже открывается http://localhost:4040/ 

<img src="/data/Module7/img/spark_ui_2.png" width="40%">

Установка завершена успешно.