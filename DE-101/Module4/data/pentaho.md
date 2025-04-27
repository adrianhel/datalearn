### 4.3.1 Начало работы с Pentaho DI

#### [Назад в Модуль 4 ⤶](/DE-101/Module4/readme.md)

В качестве ETL-инструмента будем использовать _open-source_ решение –
**[Pentaho Data Integration](https://community.pentaho.com/home)**. 

После скачивания и установки Pentaho, устанавливаем переменные среды (если есть необходимость) и можно приступать 
к работе.

В моем случае установлено было так:

<img src="/DE-101/Module4/img/environment.png" width="40%">

### 4.3.2 Основные элементы и типы проектов PDI
### Steps и Hops
В Pentaho Data Integration (PDI) ***Steps*** и ***Hops*** являются основными элементами, которые помогают в создании 
и выполнении ETL-процессов.

> ***Steps*** — это отдельные операции или трансформации, которые выполняются в процессе обработки данных.

Каждое действие, которое вы хотите выполнить с данными, представлено шагом.

> ***Hops*** — это соединения между шагами, которые определяют порядок выполнения. Они показывают, как данные 
> перемещаются от одного шага к другому.

Могут использоваться для передачи метаданных и других параметров между шагами.

### Jobs и Transformations
В PDI есть два типа проектов — ***Jobs*** и ***Transformations***.

> ***Transformations*** представляют собой набор шагов, которые обрабатывают данные.

Каждая трансформация отвечает за извлечение данных, их преобразование и загрузку в конечный источник.

> ***Jobs*** — это проекты, которые управляют выполнением одной или нескольких трансформаций и других задач. 

Они помогают организовать выполнение ETL-процессов, обеспечивая управление зависимостями и обработку ошибок.

### 4.3.3 План действий
Работать будем с исходными данными нашего «Superstore» из 
[Модуля 1](https://github.com/adrianhel/datalearn/blob/main/DE-101/Module1/readme.md).
1. Сделаем job, где скачаем файл **superstore.xls** через _http_ протокол.
2. Объединим данные из разных листов «Superstore» в одну таблицу **superstore_general.csv** (формат **CSV**).
3. Разобьем данные на разные форматы:
    - Информацию о продуктах сохраним в **JSON**
    - Информацию о возвратах сохраним в **XML**
    - Информацию о заказах разобьем по регионам:
        - CENTRAL - одним файлом в формате **XLS**
        - WEST - несколько файлов, разбитых по штатам в формате **CSV**
        - SOUTH - одним файлом в **ZIP** архиве в формате **CSV**
        - EAST - одним текстовым файлом с расширением **.dat**
4. Добавим ошибки, эмулируя реальные проблемы:
    - WEST - разные названия страны (US, USA, United States), лишние символы в поле `City`
    - EAST - опечатки в названиях городов (сложнопрогнозируемые для ручного исправления)
    - SOUTH - дубли заказов
### 4.3.4 Выполнение плана
После запуска PDI, для удобства в работе, нажимаем комбинацию клавиш **«CTRL-ALT-J»** и настраиваем окружение — 
выбираем папку для хранения jobs и transformations (в моем случае _work_), и папку для хранения собранных данных 
(в моем случае _data_).

<img src="/DE-101/Module4/img/envi_var.png" width="40%">

Приступаем к выполнению.
1. [Job для скачивания исходного файла Superstore](/DE-101/Module4/data/pentaho/job_download_superstore.kjb)

<img src="/DE-101/Module4/img/job_download.png" width="50%">

2. [Transformation для создания Superstore general](/DE-101/Module4/data/pentaho/transformation_general.ktr)

<img src="/DE-101/Module4/img/transform_general.png" width="80%">

Метрики

<img src="/DE-101/Module4/img/general_metrics.png" width="90%">

3. [Transformation для разбиения Superstore general](/DE-101/Module4/data/pentaho/transformation_general_split.ktr)

<img src="/DE-101/Module4/img/transform_general_split.png" width="100%">

Метрики

<img src="/DE-101/Module4/img/split_metrics.png" width="90%">

4. [Transformation для добавления ошибок](/DE-101/Module4/data/pentaho/transformation_add_errors.ktr)

<img src="/DE-101/Module4/img/transform_add_errors.png" width="90%">

Метрики

<img src="/DE-101/Module4/img/errors_metrics.png" width="90%">

