## 4.5.1 Знакомство с Apache Airflow

[![Airflow](https://img.shields.io/badge/apache-airflow-green?logo=airbnb)](https://airflow.apache.org/docs/)

### [Назад в Модуль 4 ⤶](/data/Module4/readme.md)

> ***Apache Airflow*** — это _open-source_ оркестратор для управления процессами загрузки и обработки данных.

### Преимущества Airflow
- **Масштабируемость**:

_Поддерживает распределённое выполнение (Celery/Kubernetes), интеграцию с облаками (AWS, GCP), базами данных 
и инструментами ML._

- **Отказоустойчивость**:

_Состояние задач хранится в БД, что позволяет восстанавливать прогресс._

- **Гибкость**: 

_Пользователи определяют DAG на Python, добавляя сложную логику._

> ***DAG (Directed Acyclic Graph)*** – направленный ациклический граф.

<img src="/data/Module4/img/DAG.png" width="50%">

## 4.5.2 Установка Airflow

[Руководство по установке и настройке Airflow](airflow/airflow_install.md)

## 4.5.3 Архитектура Airflow
- **Scheduler (Планировщик)**:

_Отвечает за парсинг DAG-файлов, планирование задач и передачу их в очередь. Работает как демон-процесс._

- **Executor (Исполнитель)**:

    - _LocalExecutor (для одной машины),_

    - _CeleryExecutor (распределённые задачи через Celery),_

    - _KubernetesExecutor (запуск в Kubernetes Pods)._

_Определяет как выполняются задачи._

- **Web Server (Веб-сервер)**:

_Предоставляет UI для мониторинга DAG, просмотра логов, ручного запуска задач и управления._

- **Metadata Database (База метаданных)**:

_Хранит состояние DAG, задач, конфигурации и историю выполнения (PostgreSQL, MySQL, SQLite)._

- **Worker (Воркер)**:

_В распределённых режимах (например, с Celery) обрабатывает задачи из очереди сообщений (Redis/RabbitMQ)._

- **DAG Directory**:

_Папка, где хранятся Python-скрипты, определяющие DAG._

## 4.5.4 Принцип работы Airflow
- **DAG**-файлы загружаются из каталога в метабазу.

- **Scheduler** сканирует DAG, планирует задачи и ставит их в очередь.

- **Executor** забирает задачи из очереди и запускает их (напрямую или через **Worker**).

- **Web Server** отображает статус в реальном времени и логи.

<img src="/data/Module4/img/airflow_architecture.png" width="80%">

`DAG → Scheduler → Queue → Executor/Worker → Результат в БД → Отображение в UI`

## 4.5.5 Первый DAG

Пишем [Первый DAG](airflow/first_dag.md).

### Состовляющие DAG
**1. Импорт библиотек:**  
- `from airflow import DAG`: _Импорт класса DAG из Airflow._  
- `from airflow.operators.dummy_operator import DummyOperator`: _Импорт оператора, который не выполняет никаких действий 
(используется для обозначения начала и конца)._  
- `from airflow.operators.python_operator import PythonOperator`: _Импорт оператора для выполнения Python-функций._  
- `from datetime import datetime`: _Импорт класса `datetime` для работы с датами._  

**2. Определение функции:**  
- `def my_task()`:
_Определение функции, которую мы будем выполнять в одной из задач. В данном случае она просто выводит сообщение._  

**3. Настройки по умолчанию:**
- `default_args`:
_Словарь, содержащий параметры по умолчанию для задач в DAG. 
Здесь мы указываем владельца, дату начала и количество попыток при ошибках._  

**4. Создание DAG:**  
- `dag = DAG(...)`: _Создание экземпляра класса DAG с указанием имени, параметров по умолчанию, описания и расписания 
выполнения._  

**5. Определение задач:**  
- `start = DummyOperator(...)`: _Создание задачи, которая просто обозначает начало процесса._
- `run_my_task = PythonOperator(...)`: _Создание задачи, которая будет выполнять нашу функцию my_task._
- `end = DummyOperator(...)`: _Создание задачи, которая обозначает конец процесса._  

**6. Определение порядка выполнения задач:**  
- `start >> run_my_task >> end`: _Указание порядка выполнения задач. Здесь мы указываем, что сначала выполняется задача 
`start`, затем `run_my_task` и в конце `end`._  

## 4.5.6 Сенсоры в Apache Airflow
Сенсоры — это специальные операторы, которые ожидают выполнения определенного условия перед тем, как продолжить 
выполнение следующих задач в рабочем процессе (DAG). Они полезны для синхронизации задач и контроля за состоянием внешних систем.

### Основные виды сенсоров
- **FileSensor**: Ожидает появления файла в указанной директории.

```python
from airflow.sensors.filesystem import FileSensor

file_sensor = FileSensor(
     task_id='check_file',
     filepath='/path/to/file.txt',
     fs_conn_id='fs_default',
     poke_interval=10,
     timeout=600
)
```

- **ExternalTaskSensor**: Ожидает завершения задачи в другом DAG.

```python
from airflow.sensors.external_task import ExternalTaskSensor

external_task_sensor = ExternalTaskSensor(
     task_id='wait_for_other_dag',
     external_dag_id='other_dag',
     external_task_id='task_in_other_dag',
     mode='poke',
     timeout=600
)
```

- **TimeDeltaSensor**: Ожидает определенного времени перед выполнением задачи.

```python
from airflow.sensors.time_delta import TimeDeltaSensor
from datetime import timedelta

time_delta_sensor = TimeDeltaSensor(
    task_id='wait_for_time_delta',
    delta=timedelta(minutes=5)
)
```

- **SqlSensor**: Ожидает выполнения SQL-запроса, который возвращает результат.

```python
from airflow.sensors.sql import SqlSensor

sql_sensor = SqlSensor(
    task_id='check_sql_condition',
    sql='SELECT COUNT(*) FROM my_table WHERE condition = true',
    conn_id='my_database'
)
```

### Настройка сенсоров
- **poke_interval**: Интервал между попытками проверки условия.  
- **timeout**: Максимальное время ожидания, после которого сенсор завершится с ошибкой.  
- **mode**: Режим работы сенсора, может быть `poke` (проверка с интервалами) или `reschedule` (ожидание события).  
