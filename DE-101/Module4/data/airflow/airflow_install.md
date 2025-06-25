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

### 2. Создаем файл окружения
Создаем в корне проекта `.env` файл со следующим содержимым:

```
AIRFLOW_UID=50000
```

### 3. Запускаем docker-compose
Запускаем docker-compose файл и инициализируем БД следующей командой:

```bash
docker compose up airflow-init
```
Будут созданы необходимые директории для создания дагов и плагинов, а так-же директория с логами.

### 4. Запускаем все службы
Будут подняты все необходимые сервисы (включая бд Postgres, redis, celery воркеры и тд).

```bash
docker-compose up
```

### 5. Авторизация