### 4.5.2 Установка и настройка Airflow

#### [Назад к Airflow ⤶](/DE-101/Module4/data/airflow.md)

[Краткое руководство](https://airflow.apache.org/docs/apache-airflow/2.11.0/howto/docker-compose/index.html)

**Docker** и *Python* должны быть установлены.

### 1. Качаем в локальную папку docker-compose файл

```bash
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.11.0/docker-compose.yaml'
```

Если выдает ошибку, попробуйте удалить псевдоним _curl_ следующей командой:

```bash
Remove-item alias:curl
```

