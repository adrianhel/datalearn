### 3.4.2 Установка и настройка Superset

#### [Назад к Metabase ⤶](/DE-101/Module3/data/metabase.md)

[Краткое руководство](https://www.metabase.com/docs/latest/installation-and-operation/running-metabase-on-docker) 
по установке Metabase (**Docker** должен быть установлен).

#### 1. Качаем последнюю версию Metabase

```commandline
docker pull metabase/metabase:latest
```

#### 2. Запускаем контейнер Metabase
По умолчанию Metabase запустится на порту 3000 по адресу http://localhost:3000

```commandline
docker run -d -p 3000:3000 --name metabase metabase/metabase
```

#### 3. Запуск Metabase на другом порту
Чтобы запустить Metabase с открытым исходным кодом на другом порту, например, на порту 12345

```commandline
docker run -d -p 12345:3000 --name metabase metabase/metabase
```

#### 4. Дополнительно
Чтобы просмотреть журналы при инициализации
Контейнер можно остановить и удалить Superset при необходимости

```commandline
docker logs -f metabase
```