### 4.5.5 Пишем первый DAG

### [Назад к Airflow ⤶](/DE-101/Module4/data/airflow.md)

```python
from airflow import DAG
from airflow.operators.dummy_operator import DummyOperator
from airflow.operators.python_operator import PythonOperator
from datetime import datetime

# Функция, которая будет выполняться в задаче
def my_task():
    print("Hello, Airflow!")

# Определение DAG
default_args = {
    'owner': 'airflow',
    'start_date': datetime(2023, 1, 1),
    'retries': 1,
}

dag = DAG(
    'simple_dag',
    default_args=default_args,
    description='A simple DAG example',
    schedule_interval='@daily',
)

# Задача 1: Начало
start = DummyOperator(
    task_id='start',
    dag=dag,
)

# Задача 2: Выполнение Python функции
run_my_task = PythonOperator(
    task_id='run_my_task',
    python_callable=my_task,
    dag=dag,
)

# Задача 3: Завершение
end = DummyOperator(
    task_id='end',
    dag=dag,
)

# Определение порядка выполнения задач
start >> run_my_task >> end
```