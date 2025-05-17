### 3.4.3 Установка и настройка Superset

#### [Назад к Superset ⤶](/DE-101/Module3/data/superset.md)

[Краткое руководство](https://superset.apache.org/docs/quickstart/) по установке Apache Superset 
(**Docker** и **Git** должны быть установлены).

#### 1. Проверяем git и docker

```commandline
git --version
```

```commandline
docker --version
```

#### 2. Клонируем репозиторий Superset
Создаем sset директорию

```commandline
git clone https://github.com/apache/superset sset
```

#### 3. Переходим в нашу директорию

```commandline
cd sset
```

#### 4. Переходим на рабочую ветку

```commandline
git checkout tags/4.1.2
```

#### 5. Запускаем Superset используя Docker Compose

```commandline
docker compose -f docker-compose-image-tag.yml up
```

#### 6. Вход в Superset
Перейдем по адресу http://localhost:8088 введем логин и пароль:

```commandline
username: admin
password: admin
```

#### 7. После работы с Superset
Контейнер можно остановить и удалить Superset при необходимости

```commandline
docker compose down
```