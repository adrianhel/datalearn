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

<p align="center">
<img src="/data/Module4/img/dag.png" width="50%">
</p>

## 4.5.2 Установка Airflow

[Руководство по установке и настройке Airflow](airflow/airflow_install.md)

## 4.5.3 Архитектура Airflow
- **Scheduler (Планировщик)**:
_Отвечает за парсинг DAG-файлов, планирование задач и передачу их в очередь. Работает как демон-процесс._  

- **[Executor (Исполнитель)](airflow/executors.md)**:  
    - _Sequential Executor (однопоточный режим),_  
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
- **Scheduler** сканирует **DAG**, планирует задачи и ставит их в очередь.  
- **Executor** забирает задачи из очереди и запускает их (напрямую или через **Worker**).
- **Web Server** отображает статус в реальном времени и логи.  

<img src="/data/Module4/img/airflow_architecture.png" width="80%">

`DAG → Scheduler → Queue → Executor/Worker → Результат в БД → Отображение в UI`

## 4.5.5 Планирование (Scheduling)
Планирование в Airflow относится к автоматическому запуску рабочих процессов на предварительно заданных интервалах. 
Это осуществляется с помощью cron-подобных выражений, которые позволяют очень гибко настраивать частоту запуска.  

Возможности планирования включают:
- **Start Date**: 
_Дата и время, с которого начнется выполнение первого запуска DAG._  
- **End Date**: 
_Опциональная дата и время, после которых DAG больше не будет запускаться._  
- **Interval**: 
_Расписание, по которому DAG должен выполняться (например, ежедневно, каждые пять минут и т.д.)._  

## 4.5.6 Первый DAG  
Пишем [Первый DAG](airflow/first_dag.md).

### Состовляющие DAG
**1. Импорт библиотек:**  
- `from airflow import DAG`: 
_Импорт класса **DAG** из Airflow._  
- `from airflow.operators.dummy_operator import DummyOperator`: 
_Импорт оператора, который не выполняет никаких действий 
(используется для обозначения начала и конца)._  
- `from airflow.operators.python_operator import PythonOperator`: 
_Импорт оператора для выполнения Python-функций._  
- `from datetime import datetime`: 
_Импорт класса **datetime** для работы с датами._  

**2. Определение функции:**  
- `def my_task()`:
_Определение функции **my_task()**, которую мы будем выполнять в одной из задач. 
В данном случае она просто выводит сообщение **Hello, Airflow!**._  

**3. Определение аргументов по умолчанию:**
- `default_args`:
_Словарь, содержащий [параметры по умолчанию для задач в DAG](airflow/default_args.md). 
Здесь мы указываем владельца, дату начала и количество попыток при ошибках._  
- `owner`:
_Устанавливает владельца DAG._  
- `start_date`: 
_Устанавливает [дату начала DAG](airflow/start_date.md) на 1 января 2025 года._  

**4. Создание DAG:**  
- `dag = DAG(...)`:
- _Создает объект **DAG** с именем **dag** с помощью следующих параметров:_  
  - `simple_dag`: 
  _Задает имя DAG как **simple_dag**._  
  - `default_args=default_args`: 
  _Указывает словарь аргументов по умолчанию, определенный ранее._  
  - `description='My first DAG'`: 
  _Предоставляет описание DAG._  
  - `schedule_interval='@daily'`: 
  _Устанавливает интервал выполнения DAG для ежедневного запуска._  

**5. Определение задач:**  
- `start = DummyOperator(...)`: 
_Создание задачи, которая обозначает начало процесса с помощью следующих параметров:_
  - `task_id='start'`: 
  _Устанавливает ID задачи на **start**._  
  - `dag=dag`: 
  _Назначает задачу объекту dag._  
- `run_my_task = PythonOperator(...)`: 
_Здесь мы создаем экземпляр класса **PythonOperator** и присваиваем его переменной **run_my_task**:_  
  - `task_id='run_my_task'`: 
  _Устанавливает ID задачи на **run_my_task**._    
  - `python_callable=my_task`: 
  _Параметр python_callable указывает на функцию, которая будет выполнена, когда задача будет запущена._   
  - `dag=dag`: 
  _Назначает задачу объекту dag._
- `end = DummyOperator(...)`: 
_Создание задачи, которая обозначает конец процесса с помощью следующих параметров:_
  - `task_id='start'`: 
  _Устанавливает ID задачи на **end**._  
  - `dag=dag`: 
  _Назначает задачу объекту dag._  

**6. Определение порядка выполнения задач:**  
- `start >> run_my_task >> end`: 
_Указание порядка выполнения задач. Здесь мы указываем, что сначала выполняется задача 
**start**, затем **run_my_task** и в конце **end**._  

## 4.5.7 Операторы Apache Airflow (Operators)
> ***Операторы*** — это базовые строительные блоки, определяющие, что должно быть выполнено в рабочем процессе.

Операторы можно разделить на несколько основных категорий в зависимости от их функциональности:
1. **Action Operators**: Выполняют определенные действия или задачи.  
Например:  
   - **BashOperator** — выполняет _bash_-команду.  
   - **PythonOperator** — выполняет функцию _Python_.  
   - **EmailOperator** — отправляет _email_.  
2. **Transfer Operators**: Перемещают данные между различными источниками и приемниками.  
Например:
   - **S3ToRedshiftOperator** — переносит данные из _Amazon S3_ в _Redshift_.  
   - **GoogleCloudStorageToBigQueryOperator** — загружает данные из _Google Cloud Storage_ в _BigQuery_.  
3. **Sensor Operators**: Ожидают наступления определенного события или условия.

### Наиболее часто используемые операторы в Airflow
- **BashOperator**: 
_Запускает команды в операционной системе, используя интерпретатор командной строки (например, Bash)._

```python
from airflow.operators.bash import BashOperator

run_bash_command = BashOperator(
    task_id='run_bash_command',
    bash_command='echo "Hello, World!"',
    dag=dag,
)
```
Этот _DAG_ с одной задачей, которая выводит строку _Hello, World!_.


- **EmailOperator**: 
_Отправляет электронные письма._  

```python
from airflow import DAG
from airflow.operators.email import EmailOperator
from datetime import datetime

dag = DAG('email_example', start_date=datetime(2025, 1, 1))

task = EmailOperator(
    task_id='send_email',
    to='adrianhel@mail.ru',
    subject='Airflow Email',
    html_content='<p>This is an Airflow email.</p>',
    dag=dag
)
```
Этот _DAG_ с одной задачей, которая отправляет электронное письмо с помощью **EmailOperator**.


- **PythonOperator**:
_Выполняет произвольный код Python в рамках оператора._  

```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from datetime import datetime

dag = DAG('python_example', start_date=datetime(2025, 1, 1))

def print_hello():
    print("Hello Airflow")

task = PythonOperator(
    task_id='print_hello',
    python_callable=print_hello,
    dag=dag
)
```
Этот _DAG_ с одной задачей, которая выполняет функцию _print_hello_, выводящую строку _Hello Airflow_.


- **SQLExecuteQueryOperator**: 
_Выполняет SQL-запросы на заданном подключении к базе данных._

```python
from airflow import DAG
from airflow.providers.common.sql.operators.sql import SQLExecuteQueryOperator
from datetime import datetime

dag = DAG('sql_example', start_date=datetime(2025, 1, 1))

task = SQLExecuteQueryOperator(
    task_id='run_sql',
    sql='SELECT * FROM my_table',
    database='my_database',
    dag=dag
)
```
Этот _DAG_ с одной задачей, которая выполняет запрос с помощью _SQLExecuteQueryOperator._


- **DockerOperator**: 
_Запускает контейнеры Docker._

```python
from airflow import DAG
from airflow.providers.docker.operators.docker import DockerOperator
from datetime import datetime

dag = DAG('docker_example', start_date=datetime(2025, 1, 1))

task = DockerOperator(
    task_id='run_container',
    image='my_image:latest',
    command='python script.py',
    dag=dag
)
```
Этот _DAG_ с одной задачей, которая запускает _Docker_-контейнер из указанного образа с помощью оператора DockerOperator, 
плюс *python script.py* выполняется внутри контейнера.


- **DummyOperator**:
_Это оператор, который ничего не делает._

```python
from airflow.operators.dummy_operator import DummyOperator

start_task = DummyOperator(
    task_id='start_task',
    dag=dag,
)
```
Обычно используется для создания _пустых_ задач, которые могут служить в качестве маркеров или для управления зависимостями.  


- **BranchPythonOperator**: 
_Позволяет создать ветвление в DAG._

```python
from airflow.operators.branch_operator import BranchPythonOperator

branching = BranchPythonOperator(
    task_id='branching',
    python_callable=my_branching_function,
    dag=dag,
)
```
На основе результата выполнения функции можно выбрать, какая задача будет выполнена дальше.


- **HiveOperator**: 
_Выполняет операции Hive._  
- **S3FileTransferOperator**: 
_Копирует файлы между локальной файловой системой и Amazon S3._  
- **SlackAPIOperator**: 
_Отправляет сообщения в Slack._  
- **SparkSubmitOperator**: 
_Отправляет задачи Spark для выполнения на кластере Apache Spark._  
- **HttpOperator**: 
_Выполняет HTTP-запросы к удаленным серверам._  

Можно создавать собственные пользовательские операторы, наследуемые от базового класса **BaseOperator**, чтобы выполнить 
специфические для вашего рабочего процесса задачи.  

### При использовании операторов в Airflow рекомендуется
- Использовать параметры для управления поведением операторов, чтобы сделать выполнение задач более гибким и настраиваемым.  
- Определять зависимости между задачами ясно и логически, используя _set_upstream()_ или _set_downstream()_, 
либо операторы `>>` и `<<`.  
- Избегать запуска тяжелых процессов непосредственно в операторах, таких как **PythonOperator**, и вместо этого вызывать 
внешние скрипты или сервисы.  

## 4.5.8 Сенсоры в Apache Airflow (Sensor Operators)
> ***Сенсоры*** — это операторы, ожидающие выполнения определенного условия, для продолжения выполнения следующих задач 
> в рабочем процессе.  

Полезны для синхронизации задач и контроля за состоянием внешних систем.

### Основные виды сенсоров
- **FileSensor**: 
_Ожидает появления файла в указанной директории._  

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

- **ExternalTaskSensor**: 
_Ожидает завершения задачи в другом DAG._  

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

- **TimeDeltaSensor**: 
_Ожидает определенного времени перед выполнением задачи._  

```python
from airflow.sensors.time_delta import TimeDeltaSensor
from datetime import timedelta

time_delta_sensor = TimeDeltaSensor(
    task_id='wait_for_time_delta',
    delta=timedelta(minutes=5)
)
```

- **SqlSensor**: 
_Ожидает выполнения SQL-запроса, который возвращает результат._  

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

## 4.5.9 Пользовательский интерфейс (Airflow UI)
Airflow предоставляет веб-интерфейс, который позволяет пользователям визуально управлять выполнением и мониторингом 
рабочих процессов.  

Веб-интерфейс предоставляет следующие возможности:  
- **DAG Overview**: 
_Просмотр всех DAGs, доступных в системе, с информацией о том, когда каждый DAG был запущен, его статус и интерактивное 
представление структуры DAG._  
- **Task Instance Details**: 
_Подробная информация о каждой задаче в рамках DAG, включая логи выполнения, время начала и окончания, а также текущий 
статус._  
- **Code Viewing**: 
_Возможность просмотра кода, который определяет каждый DAG, прямо из интерфейса._  
- **Triggering & Clearing**: 
_Возможность ручного запуска или очистки задач для тестирования или оперативного управления._  
- **Gantt Chart & Graph View**: 
_Визуализация выполнения DAG в виде диаграммы Ганта или графического представления, что помогает анализировать 
продолжительность и зависимости задач._  