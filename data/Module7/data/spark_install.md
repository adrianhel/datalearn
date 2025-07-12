## 7.2 Установка Apache Spark

[![Scala](https://img.shields.io/badge/scala-2.12.18-red)](https://scala-lang.org/download/)
[![Java](https://img.shields.io/badge/java_JDK-21.0.7-red)](https://www.oracle.com/java/technologies/downloads/)
[![Spark](https://img.shields.io/badge/apache_spark-3.5.6-red)](https://spark.apache.org/downloads.html)

### [Назад в Модуль 7 ⤶](/data/Module7/readme.md)

Для установки нам понадобится скачать **Java JDK**, **Scala*, **Spark** и ещё 1 файлик с **github**.
Прежде, чем начать, проверьте в 
[таблице совместимости](https://community.cloudera.com/t5/Community-Articles/Spark-Scala-Version-Compatibility-Matrix/ta-p/383713)
версии **Java JDK**, **Scala**, **Spark** соответственно.

### 7.2.1 Установка Java
- Скачать _[Java JDK-21](https://download.oracle.com/java/21/latest/jdk-21_windows-x64_bin.exe)_   
- Установить _Java JDK_  
- Добавляем системную переменную: Имя: `JAVA_HOME`, Значение: `C:\Program Files\Java\jdk-21`
- Откройте командную строку и введите:

```bash
java --version
```

Ниже отобразится информация об установке _Java_:

<img src="/data/Module7/img/java_version.png" width="60%">


### 7.2.2 Установка Scala
- Скачать _[Scala 2.12.18](https://github.com/scala/scala/releases/download/v2.12.18/scala-2.12.18.msi)_  
- Установить _Scala_   
- Добавляем системную переменную: Имя: `SCALA_HOME`, Значение: `C:\Program Files (x86)\scala` 
- В командной строке введите следующую команду:  

```bash
scala
```

Ниже отобразится информация об установке _Scala_:  

<img src="/data/Module7/img/scala_version.png" width="60%">


### 7.2.3 Установка Spark
- Скачать _[Spark](https://dlcdn.apache.org/spark/spark-3.5.6/spark-3.5.6-bin-hadoop3.tgz)_
- Распаковать скачанный архив на диск, например в папку `C:\spark`  
- Добавляем системную переменную: Имя: `SPARK_HOME`, Значение: `C:\spark\spark-3.5.6-bin-hadoop3`  
- Выбираем среду `Path` и нажимаем  `Изменить`, далее нажимаем `Создать` и добавляем значение: `%SPARK_HOME%\bin`  

### 7.2.4 Загрузка утилит Windows
Если вы хотите работать с данными _Hadoop_, выполните следующие действия, чтобы загрузить утилиту для _Hadoop_:

- Скачайте файл _[winutils.exe](https://github.com/steveloughran/winutils/tree/master)_  
- Скопируйте файл в папку `C:\hadoop\bin`  
- Добавляем системную переменную: Имя: `HADOOP_HOME`, Значение: `C:\hadoop`  
- Выбираем среду `Path` и нажимаем  `Изменить`, далее нажимаем `Создать` и добавляем значение: `%HADOOP_HOME%\bin`  


- Выполните команду, чтобы проверить установку Spark, как показано ниже:

```bash
spark-shell
```
