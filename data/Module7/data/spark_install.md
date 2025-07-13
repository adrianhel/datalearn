## 7.2 Установка Apache Spark

[![Java](https://img.shields.io/badge/java_JDK-21.0.7-red)](https://www.oracle.com/java/technologies/downloads/)
[![Scala](https://img.shields.io/badge/scala-2.13.16-red)](https://scala-lang.org/download/)
[![Spark](https://img.shields.io/badge/apache_spark-3.5.6-red)](https://spark.apache.org/downloads.html)

### [Назад в Модуль 7 ⤶](/data/Module7/readme.md)

Для установки нам понадобится скачать **Java JDK**, **Scala**, **Spark** и ещё 1 файлик с **github**.
Прежде, чем начать, проверьте в 
[таблице совместимости](https://community.cloudera.com/t5/Community-Articles/Spark-Scala-Version-Compatibility-Matrix/ta-p/383713)
версии *Java JDK*, *Scala* и *Spark* соответственно.

### 7.2.1 Установка Java
- Скачать [Java JDK-21](https://download.oracle.com/java/21/latest/jdk-21_windows-x64_bin.exe)   
- Установить _Java JDK_  
- Добавляем системную переменную: Имя: `JAVA_HOME`, Значение: `C:\Program Files\Java\jdk-21`
- Выбираем среду `Path` и нажимаем  `Изменить`, далее нажимаем `Создать` и добавляем значение: `%JAVA_HOME%\bin` 
- Откройте командную строку и введите:

```bash
java -version
```

Ниже отобразится информация об установке _Java_:

<img src="/data/Module7/img/java_version.png" width="90%">


### 7.2.2 Установка Scala
- Скачать [Scala 2.13.16](https://github.com/scala/scala/releases/download/v2.13.16/scala-2.13.16.msi)  
- Установить _Scala_   
- Добавляем системную переменную: Имя: `SCALA_HOME`, Значение: `C:\Program Files (x86)\scala` 
- Выбираем среду `Path` и нажимаем  `Изменить`, далее нажимаем `Создать` и добавляем значение: `%SCALA_HOME%\bin`  
- В командной строке введите следующую команду:  

```bash
scala
```

Ниже отобразится информация об установке _Scala_:  

<img src="/data/Module7/img/scala_version.png" width="90%">


### 7.2.3 Установка Spark
- Скачать [Spark](https://dlcdn.apache.org/spark/spark-3.5.6/spark-3.5.6-bin-hadoop3.tgz)
- Распаковать скачанный архив на диск, например в папку `C:\spark`  
- Добавляем системную переменную: Имя: `SPARK_HOME`, Значение: `C:\spark\spark-3.5.6-bin-hadoop3`  
- Выбираем среду `Path` и нажимаем  `Изменить`, далее нажимаем `Создать` и добавляем значение: `%SPARK_HOME%\bin`  

### 7.2.4 Загрузка утилит Windows
Если вы хотите работать с данными _Hadoop_, выполните следующие действия, чтобы загрузить утилиту для _Hadoop_:

- Скачайте файл [winutils.exe](https://github.com/steveloughran/winutils/tree/master) для _hadoop3_  
- Скопируйте файл в папку `C:\hadoop\bin`  
- Добавляем системную переменную: Имя: `HADOOP_HOME`, Значение: `C:\hadoop`  
- Выбираем среду `Path` и нажимаем  `Изменить`, далее нажимаем `Создать` и добавляем значение: `%HADOOP_HOME%\bin`  

### 7.2.5 Переменные для pyspark
- Добавляем системную переменную: Имя: `PYSPARK_HOME`, Значение: `C:\Users\andy\AppData\Local\Programs\Python\Python311\python.exe`
- Выбираем среду `Path` и нажимаем  `Изменить`, далее нажимаем `Создать` и добавляем значение: `%PYSPARK%` 

### 7.2.6 Запуск Spark
- Выполните команду, чтобы проверить установку _Spark_, как показано ниже:

```bash
spark-shell
```
Ниже отобразится информация об запуске _Spark_:

<img src="/data/Module7/img/spark_version.png" width="90%">  

Для выхода:

```bash
:q
```

- Выполните команду, чтобы проверить установку _pyspark_, как показано ниже:

```bash
pyspark
```

Для выхода:

```bash
quit
```

<img src="/data/Module7/img/pyspark_version.png" width="90%">

### 7.2.7 Spark UI
Spark UI доступен по адресу http://localhost:4040

<img src="/data/Module7/img/spark_ui.png" width="90%">