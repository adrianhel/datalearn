## 7.11.1 Spark MLlib: машинное обучение

### [Назад в Модуль 7 ⤶](/data/Module7/readme.md)

**Машинное обучение (Machine Learning, ML)** — область искусственного интеллекта, занимающаяся разработкой алгоритмов 
и моделей, способных выявлять закономерности в данных и делать прогнозы или принимать решения без явного 
программирования. Ключевыми этапами процесса машинного обучения являются сбор и подготовка данных, выбор модели, 
обучение (training), валидация, тестирование и внедрение модели.  

**DataFrame API:** основной API для работы с MLlib, основанный на концепции таблиц с именованными столбцами.  

**ML Pipeline:** механизм построения последовательностей этапов обработки данных и обучения моделей, состоящий из 
трансформеров (transformers) и оценщиков (estimators).  

**Трансформеры (Transformers):** объекты, преобразующие DataFrame (например, StandardScaler, PCA, модель классификации).  

**Оценщики (Estimators):** объекты, которые могут быть обучены на данных и превращаются в трансформеры (например, 
LogisticRegression, DecisionTreeClassifier).  

**Параметры (Params):** механизм настройки поведения моделей и трансформеров через ключ-значение.  

**Модели (Models):** обученные экземпляры оценщиков, используемые для предсказаний.  

## 7.11.2 Типы задач машинного обучения
1. **Обучение с учителем (Supervised Learning):** используется, когда имеются размеченные данные (каждому объекту 
сопоставлена метка или значение целевой переменной).  
2. **Обучение без учителя (Unsupervised Learning):** задачи группировки данных или выявления структуры в неразмеченных 
данных.  
3. **Обучение с подкреплением (Reinforcement Learning):** в MLlib не реализовано.  

## 7.11.3 Основные алгоритмы и методы MLlib
### Классификация
**Классификация** — задача отнесения объектов к одному из заранее известных классов.  
  
- **Логистическая регрессия (Logistic Regression):** применяется для бинарной и многоклассовой классификации.  
- **Деревья решений (Decision Trees):** алгоритм построения дерева, где узлы — проверки признаков, листья — классы.  
- **Случайный лес (Random Forest):** ансамбль деревьев решений для повышения точности и устойчивости.  
- **Градиентный бустинг над деревьями (GBTClassifier):** ансамбль деревьев с последовательным обучением.  
- **Линейные методы (Linear SVM, Naive Bayes):** линейные классификаторы для различных задач.  

#### Пример кода: Логистическая регрессия

```python
from pyspark.ml.classification import LogisticRegression

lr = LogisticRegression(featuresCol="features", labelCol="label")
model = lr.fit(training_data)
predictions = model.transform(test_data)
```

                  
### Регрессия
**Регрессия** — предсказание числового значения целевой переменной.  
  
- **Линейная регрессия (Linear Regression):** базовый метод для регрессионных задач.  
- **Дерево решений для регрессии (DecisionTreeRegressor):** построение дерева для предсказания числовых значений.  
- **Случайный лес для регрессии (RandomForestRegressor):** ансамбль деревьев для регрессии.  
- **Градиентный бустинг для регрессии (GBTRegressor):** ансамбль деревьев с бустингом.  

#### Пример кода: Линейная регрессия

```python
from pyspark.ml.regression import LinearRegression

lr = LinearRegression(featuresCol="features", labelCol="label")
model = lr.fit(training_data)
predictions = model.transform(test_data)
```

                  
### Кластеризация
**Кластеризация** — поиск групп (кластеров) похожих объектов в неразмеченных данных.  
  
- **K-средних (KMeans):** алгоритм разделения данных на K кластеров по расстоянию до центроидов.  
- **Gaussian Mixture Model (GMM):** вероятностный подход к кластеризации на основе смеси гауссовых распределений.  
- **Bisecting K-means:** иерархический вариант алгоритма K-средних.  

#### Пример кода: KMeans

```python
from pyspark.ml.clustering import KMeans

kmeans = KMeans(featuresCol="features", k=3)
model = kmeans.fit(dataset)
predictions = model.transform(dataset)
```
                  
### Снижение размерности
- **Principal Component Analysis (PCA):** линейное преобразование для проекции данных в пространство меньшей размерности.  
- **Feature Selection:** методы отбора наиболее информативных признаков (ChiSqSelector, VectorSlicer).  

#### Пример кода: PCA

```python
from pyspark.ml.feature import PCA

pca = PCA(k=2, inputCol="features", outputCol="pcaFeatures")
model = pca.fit(dataset)
result = model.transform(dataset)
```

## 7.11.4 Предобработка и преобразование признаков
- **VectorAssembler:** сборка нескольких столбцов признаков в вектор признаков.  
- **StandardScaler:** стандартизация признаков (нулевое среднее, единичное стандартное отклонение).  
- **StringIndexer:** преобразование строковых категориальных признаков в числовые индексы.  
- **OneHotEncoder:** one-hot-кодирование категориальных признаков.  
- **Imputer:** заполнение пропущенных значений.  
- **MinMaxScaler, MaxAbsScaler:** масштабирование признаков в заданные диапазоны.  

#### Пример кода: VectorAssembler и StandardScaler

```python
from pyspark.ml.feature import VectorAssembler, StandardScaler

assembler = VectorAssembler(inputCols=["col1", "col2", "col3"], outputCol="features")
assembled = assembler.transform(df)

scaler = StandardScaler(inputCol="features", outputCol="scaledFeatures")
scaled_model = scaler.fit(assembled)
scaled_data = scaled_model.transform(assembled)
```

## 7.11.5 Конвейеры машинного обучения (ML Pipelines)
MLlib поддерживает построение конвейеров (pipelines), которые объединяют этапы предобработки, отбора признаков, 
обучения и применения модели. Это обеспечивает воспроизводимость, масштабируемость и автоматизацию процессов машинного 
обучения.  

#### Структура конвейера
1. Этапы предобработки данных
2. Преобразование признаков
3. Обучение модели
4. Применение модели для предсказаний

#### Пример кода: Конвейер

```python
from pyspark.ml import Pipeline
from pyspark.ml.feature import StringIndexer, VectorAssembler
from pyspark.ml.classification import RandomForestClassifier

indexer = StringIndexer(inputCol="category", outputCol="categoryIndex")
assembler = VectorAssembler(inputCols=["feature1", "feature2", "categoryIndex"], outputCol="features")
rf = RandomForestClassifier(featuresCol="features", labelCol="label")

pipeline = Pipeline(stages=[indexer, assembler, rf])
model = pipeline.fit(training_data)
predictions = model.transform(test_data)
```

## 7.11.6 Оценка качества моделей
**Метрические функции:** для классификации — accuracy, precision, recall, f1-score; для регрессии — RMSE, MAE, R2.  
**CrossValidator:** перекрестная проверка для устойчивой оценки качества моделей.  
**TrainValidationSplit:** разбиение на обучающую и валидационную выборки для подбора гиперпараметров.  

#### Пример кода: Кросс-валидация

```python
from pyspark.ml.classification import LogisticRegression
from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
from pyspark.ml.evaluation import BinaryClassificationEvaluator

lr = LogisticRegression()
paramGrid = ParamGridBuilder() \
    .addGrid(lr.regParam, [0.1, 0.01]) \
    .addGrid(lr.elasticNetParam, [0.0, 0.5, 1.0]) \
    .build()

evaluator = BinaryClassificationEvaluator()
cv = CrossValidator(estimator=lr, estimatorParamMaps=paramGrid, evaluator=evaluator, numFolds=3)

cvModel = cv.fit(training_data)
```