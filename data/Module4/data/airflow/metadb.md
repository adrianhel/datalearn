## 4.5.3 База метаданных (Metadata Database)

### [Назад к Airflow ⤶](/data/Module4/data/airflow.md)

База метаданных содержит всю информацию о том, сколько _DAG_'ов есть в нашей системе и какие подключения настроены. БД 
логирует абсолютно все исполнения задач и их статусы. В зависимости от того, какой у нас _[Executor](executors.md)_,
могут использоваться разные БД.

### Основные таблицы
Таблицы _DAG_, где хранится вся информация о них: активных, неактивных, их расположении и вся метаинформация, 
которая доступна. **Airflow** хранит даже "удаленные" _DAG's_.  
Вторая по важности таблица — это таблица _Connection_, в которой хранятся объекты подключения.  
Также важными таблицами являются таблицы _task_instance_, в которых хранится информация обо всех запусках наших задач.  

### Connections и Variables
- **Connection** отвечает за хранение информации о подключениях к внешним системам.
- **Variables** — это глобальные переменные.

### Connections
Представляют собой конфигурационные параметры, которые позволяют установить соединение с внешними ресурсами, 
такими как базы данных, облачные сервисы, _API_ и другие системы. _Connections_ содержат информацию, необходимую 
для установки и аутентификации соединений с этими ресурсами.  

Каждое соединение в **Airflow** имеет уникальное имя и определённый тип, который соответствует типу ресурса, с которым 
устанавливается соединение. Использование _Connections_ позволяет избежать хранения конфиденциальной информации, 
такой как пароли, прямо в коде задач или _DAG_. Можно определить соединение в **Airflow** и ссылаться на него в задачах 
или _DAG_, чтобы получать доступ к необходимым ресурсам.

#### Пример создания соединения
Использовать _Connection_ можно несколькими способами. Можно создать соединение через веб-интерфейс **Airflow**, 
перейдя в раздел `Admin -> Connections`, или программно, используя _Connection API_.

_Connection_ может хранить все необходимые ключи для того, чтобы подключаться к базе данных или какому-либо веб-ресурсу. 
В этом случае мы передаем в параметры оператора идентификатор подключения. Идентификатором подключения является 
уникальное имя, которое мы задаем при создании этого объекта в интерфейсе.  

```python
create_view = ClickHouseOperator(
    task_id=...,
    sql=... ,                                 # SQL запрос, который нужно выполнить
    clickhouse_conn_id='clickhouse_default',  # ID подключения, настроенное в Airflow
    dag=dag,
)
```

_Connection_ — это просто структура данных - словарь _(dict)_. Мы можем создать подключение и указать в его полях любые 
необходимые нам данные, а затем обращаться к ним с помощью кода.  

#### Пример доступа к переменной connection

```python
from airflow.hooks.base_hook import BaseHook                  # Подгружаем модуль, чтобы хукать

host = BaseHook.get_connection("postgres_default").host       # Обращаемся к postgres_default и получаем поле host
pass = BaseHook.get_connection("postgres_default").password   # Обращаемся к postgres_default и получаем password
```

### Variables
_Variables_ представляют собой глобальные переменные, которые хранят информацию которая редко меняется, к примеру 
ключи _API_, пути к конфигурационным файлам и др.  

_Variables_ могут содержать любые данные, такие как строки, числа, списки и даже сложные объекты в формате _JSON_. 
Их можно создать, изменить и удалить через веб-интерфейс **Airflow** или с использованием _API_.  

_Variables_ могут быть использованы в шаблонах _Jinja_, что позволяет вставлять их значения в код задач или _DAG_. 
Можно использовать переменные в качестве параметров для операторов или в условных выражениях для принятия решений 
во время выполнения.  

#### Пример доступа Variables в Airflow
- Хранение конфигурационных параметров, таких как пути к файлам или настройки подключения к базе данных.  
- Хранение состояния процесса выполнения пайплайна, например, флагов выполнения или результатов предыдущих задач.  
- Передача данных между задачами или _DAG_, когда другие механизмы, не являются подходящими.  

#### Пример доступа к глобальной переменной

```python
from airflow.models import Variable

foo = Variable.get("key", deserialize_json=True)    # Преобразует переменную из json в python dict
```

### Пример DAG c Connections и Variables

```python
from airflow import DAG
from datetime import timedelta
from airflow.utils.dates import days_ago
from airflow.operators.python import PythonOperator

from airflow.hooks.base_hook import BaseHook
from airflow.models import Variable

# Результат будет перехвачен Airflow и попадет в лог файл задачи
def connection():
    host = BaseHook.get_connection("postgres_default").host
    password = BaseHook.get_connection("postgres_default").password

    print("Result:", host, password)

def variables():  
    foo = Variable.get("key", deserialize_json=True)
    print(foo)


with DAG('example_dag', schedule_interval=None, start_date=days_ago(1),tags=['examples']) as dag:
    # Создадим оператор для исполнения python функции
    t1 = PythonOperator(task_id='connection',
                      python_callable=connection)
    t2 = PythonOperator(task_id='variables',
                      python_callable=variables)
```