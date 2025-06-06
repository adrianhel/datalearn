### 4.5.2 Установка и настройка dbt

#### [Назад к dbt ⤶](/DE-101/Module4/data/dbt.md)

[Краткое руководство](https://docs.getdbt.com/docs/core/docker-install)

**Docker** и необходимые **dbt**-адаптеры должеы быть установлены.

### 1. Установите образ Docker dbt из Github Packages
Официальные образы Docker для dbt размещены в виде пакетов вdbt-labsорганизации GitHub. Мы поддерживаем образы и теги для каждой версии каждого адаптера базы данных, а также два тега, которые обновляются по мере выхода новых версий:

- `latest`: Последняя версия dbt-core + этот адаптер
- `<Major>.<Minor>.latest`: Последняя версия dbt-core + этот адаптер для семейства версий `<Major>.<Minor>`. 

Например, `1.1.latest` включает последние обновления для dbt Core v1.1.

Установите образ с помощью команды `docker pull`:

```bash
docker pull ghcr.io/dbt-labs/<db_adapter_name>:<version_tag>
```

### 2. Запуск образа Docker dbt в контейнере
`ENTRYPOINT` для образов Docker с dbt — это команда `dbt`. Вы можете подключить свой проект к `/usr/app` и использовать 
dbt как обычно:

```bash
docker run \
--network=host \
--mount type=bind,source=path/to/project,target=/usr/app \
--mount type=bind,source=path/to/profiles.yml,target=/root/.dbt/profiles.yml \
<dbt_image_name> \
ls
```

Или

```bash
docker run \
--network=host \
--mount type=bind,source=path/to/project,target=/usr/app \
--mount type=bind,source=path/to/profiles.yml.dbt,target=/root/.dbt/ \
<dbt_image_name> \
ls
```

##### Примечания:
Источники привязки должны иметь абсолютный путь
Возможно, вам потребуется внести изменения в настройки сети Docker в зависимости от особенностей вашего Хранилища 
данных или хоста базы данных.