## 6.2.2 Установка ClickHouse

[![ClickHouse](https://img.shields.io/badge/clickhouse-23.4-blue?logo=clickhouse)](https://github.com/ClickHouse/examples/tree/main/docker-compose-recipes)

### [Назад к ClickHouse ⤶](/data/Module6/data/clickhouse.md)

Экземпляр **ClickHouse** с одним узлом и одним _Keeper-сервером ClickHouse_.
По умолчанию используется последняя версия **ClickHouse**, а **ClickHouse Keeper** — последняя версия на `alpine`. 
Можно указать конкретные версии, задав переменные среды перед запуском `docker compose -up`.

```bash
export CHVER=23.4
export CHKVER=23.4-alpine
docker compose up
```