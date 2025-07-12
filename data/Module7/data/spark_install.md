## 7.2 Установка Apache Spark

[![Scala](https://img.shields.io/badge/scala-2.13.11-red)](https://scala-lang.org/download/2.13.11.html)
[![Java](https://img.shields.io/badge/java_JDK-21.0.7-red)](https://www.oracle.com/java/technologies/downloads/)
[![Spark](https://img.shields.io/badge/apache_spark-3.5.6-red)](https://www.apache.org/dyn/closer.lua/spark/spark-3.5.6/spark-3.5.6-bin-hadoop3-scala2.13.tgz)

### [Назад в Модуль 7 ⤶](/data/Module7/readme.md)

### 7.2.1 Установка Java
- Скачать _[Java JDK-21](https://download.oracle.com/java/21/latest/jdk-21_windows-x64_bin.exe)_   
- Установить _Java JDK_  
- Добавьте `JAVA_HOME` в `PATH` со значением `C:\Program Files\Java\jdk-21`  
- Откройте командную строку и введите:

```bash
java --version
```

Ниже отобразится информация об установке _Java_:

<img src="/data/Module7/img/java_version.png" width="60%">


### 7.2.2 Установка Scala
- Скачать _[Scala 2.13.11](https://github.com/scala/scala/releases/download/v2.13.11/scala-2.13.11.msi)_.  
- Установить _Scala_  
- В командной строке введите следующую команду:  

```bash
scala
```

Ниже отобразится информация об установке _Scala_:  

<img src="/data/Module7/img/scala_version.png" width="60%">


### 7.2.3 Установка Spark
- Скачать _[Spark](https://dlcdn.apache.org/spark/spark-3.5.6/spark-3.5.6-bin-hadoop3-scala2.13.tgz)_
- Распаковать скачанный архив на диск, например в папку `C:\Spark`
- В пользовательской переменной добавьте `SPARK_HOME` в `PATH` со значением `C:\Spark\spark-3.5.6-bin-hadoop3-scala2.13`
- В системной переменной добавьте `%SPARK_HOME%\bin` в переменную `PATH`