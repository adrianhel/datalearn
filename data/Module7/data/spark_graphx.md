## 7.12.1 Spark GraphX: обработка графов

### [Назад в Модуль 7 ⤶](/data/Module7/readme.md)

- **Граф** — математическая структура, состоящая из вершин (vertices) и рёбер (edges), которые соединяют пары вершин.  

- **Ориентированный граф (digraph)** — граф, в котором рёбра имеют направление.  

- **Неориентированный граф** — граф, в котором рёбра не имеют направления.  

- **Атрибуты** — дополнительная информация, связанная с вершинами или рёбрами (например, веса, метки и т.д.).  

## 7.12.2 Архитектура GraphX
GraphX реализует граф как комбинацию двух RDD:
- **Vertex RDD** — RDD пар (VertexId, VertexAttr), где VertexId — уникальный идентификатор вершины, VertexAttr — 
атрибут вершины.  
- **Edge RDD** — RDD объектов Edge с полями srcId, dstId и attr, представляющими идентификаторы начальной и конечной 
вершин и атрибут ребра соответственно.  

Граф в GraphX представлен объектом класса Graph[VD, ED], где VD — тип атрибутов вершин, ED — тип атрибутов рёбер.  

## 7.12.3 Создание графа

```python
import org.apache.spark.graphx._
import org.apache.spark.rdd.RDD

// Пример данных
val vertexArray = Array(
  (1L, "Alice"),
  (2L, "Bob"),
  (3L, "Charlie"),
  (4L, "David")
)

val edgeArray = Array(
  Edge(1L, 2L, "friend"),
  Edge(2L, 3L, "follow"),
  Edge(3L, 4L, "friend"),
  Edge(4L, 1L, "follow")
)

// Создание RDD
val vertexRDD: RDD[(Long, String)] = sc.parallelize(vertexArray)
val edgeRDD: RDD[Edge[String]] = sc.parallelize(edgeArray)

// Создание графа
val graph: Graph[String, String] = Graph(vertexRDD, edgeRDD)
```