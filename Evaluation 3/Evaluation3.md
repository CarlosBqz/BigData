<div align="center">

**Instituto Tecnológico de Tijuana**

Departamento de Ciencias y Computación

Ingeniería en Sistemas Computacionales
 
 [![](https://upload.wikimedia.org/wikipedia/commons/2/2e/ITT.jpg)](https://upload.wikimedia.org/wikipedia/commons/2/2e/ITT.jpg)

**Title:**

Evaluation Unit 3

**Subject:**

BDD-1704 SC9A Datos Masivos

**Unit:**

III

**Professor:**

JOSE CHRISTIAN ROMERO HERNANDEZ

**Students:**

Victor Jair Aulis Sanchez 
17212836

Bojórquez Vargas Carlos Francisco
16211977

**Group:**

SC9A

**Date:**

Tijuana, Baja California, December 18, 2020. 
</div>


## Instructions
Instrucciones
Desarrolle las siguientes instrucciones en Spark con el lenguaje de programación Scala.

Objetivo:
El objetivo de este examen practico es tratar de agrupar los clientes de regiones específicas de un distribuidor al mayoreo. Esto en base a las ventas de algunas categorías de productos.

Las fuente de datos se encuentra en el repositorio:
https://github.com/jcromerohdz/BigData/blob/master/Spark_clustering/Wholesale%20customers%20data.csv

1. Importar una simple sesión Spark.
2. Utilice las lineas de código para minimizar errores
3. Cree una instancia de la sesión Spark
4. Importar la librería de Kmeans para el algoritmo de agrupamiento.
5. Carga el dataset de Wholesale Customers Data
6. Seleccione las siguientes columnas: Fresh, Milk, Grocery, Frozen, Detergents_Paper, Delicassen y llamar a este conjunto feature_data
7. Importar Vector Assembler y Vector
8. Crea un nuevo objeto Vector Assembler para las columnas de caracteristicas como un conjunto de entrada, recordando que no hay etiquetas
9. Utilice el objeto assembler para transformar feature_data
10. Crear un modelo Kmeans con K=3
11. Evalúe  los grupos utilizando Within Set Sum of Squared Errors WSSSE e imprima los centroides.

## Development

1. Importar una simple sesión Spark.

```scala
import org.apache.spark.sql.SparkSession
```

2. Utilice las lineas de código para minimizar errores

```scala
import org.apache.log4j._
Logger.getLogger("org").setLevel(Level.ERROR)
```

3. Cree una instancia de la sesión Spark

```scala
val spark = SparkSession.builder().appName("LinearRegressionAssigment").getOrCreate()
```

4. Importar la librería de Kmeans para el algoritmo de agrupamiento.

```scala
import org.apache.spark.ml.clustering.KMeans
```

5. Carga el dataset de Wholesale Customers Data

```scala
val data = spark.read.option("header", "true").option("inferSchema","true")csv("C:\\Users\\Carlos Bojorquez\\Desktop\\Noveno semestre\\Datos Masivones\\BigData\\Spark_clustering\\Wholesale_customers_data.csv")

```

6. Seleccione las siguientes columnas: Fresh, Milk, Grocery, Frozen, Detergents_Paper, Delicassen y llamar a este conjunto feature_data

```scala
val  fd  = data.select("Fresh","Milk","Grocery","Frozen","Detergents_Paper","Delicassen")
fd.show(5)
```
[![](https://lh3.googleusercontent.com/pw/ACtC-3dWRGCMqma-wmjDiL9SuBaBL0t9_U5dffqKpQlZHCZZARj-SgqGxi_s7vCNa4sZQP88JZEgWZ8TBTocbs82kxNNu6JsFF3dU9wS14K_0an0DN_s_tYYn_25KBHyCIDhjQEXyL39f9XCyCWa1fThATeR=w508-h214-no?authuser=1)](FD)

7. Importar Vector Assembler y Vector

```scala
import org.apache.spark.ml.feature.VectorAssembler
import org.apache.spark.ml.linalg.Vector
```

8. Crea un nuevo objeto Vector Assembler para las columnas de caracteristicas como un conjunto de entrada, recordando que no hay etiquetas

```scala
val assembler = new VectorAssembler() .setInputCols(Array("Fresh","Milk","Grocery","Frozen","Detergents_Paper","Delicassen")).setOutputCol("features")
```

9. Utilice el objeto assembler para transformar feature_data

```scala
val features = assembler.transform(fd)
features.show(5)
```
[![](https://lh3.googleusercontent.com/pw/ACtC-3eIuK0K19X_KkQo5adC1hprzXwbmHAgQtPX78dV4mEgf_M484llbhyrt22viOVp6e8W08AEIH1ioO8hZKEYiaBaw9MuLce8IxonhI8W4amBKiodreeQcSuCrY9SXJFCjDfX3FTsgxNyN-PkCHwHik3u=w691-h209-no?authuser=1)](Features)

10. Crear un modelo Kmeans con K=3

```scala
val kmeans = new KMeans().setK(3).setMaxIterations(5)
val model = kmeans.fit(features)
val model: KMeansModel = kmeans.fit(features)
```

11. Evalúe  los grupos utilizando Within Set Sum of Squared Errors WSSSE e imprima los centroides.

```scala
val WSSSE = model.computeCost(features)
println(s"Within set sum of Squared Errors = $WSSSE")
```
[![](https://lh3.googleusercontent.com/pw/ACtC-3dXdCGgDLWNwFJN3EEBersHdGrs2hNwwcpR4BCojJCXYO5ad-32BNG8hIjSZx5lfqzMl68srGqgpxp9CAM7wGkT3czG-OjxGO1i1nweKWlNRshA33WxBbL_fUPUQfX192GN7dPh-twPPFtd58sU7jHh=w749-h65-no?authuser=1)](WSSSE)
```scala
println("Cluster Centers: ")
model.clusterCenters.foreach(println)
```
[![](https://lh3.googleusercontent.com/pw/ACtC-3fpF3FioYg5iGRHJdwOcXvJNe2LAyWvx9D0cyBITMoomsZxvDGX0QHHQtLYy6-I6q1-aaVt3Hm1k2MI7wvzNOX6oHrWtrk3aZHVeK1t7o70M6ZW-9fSiM2Weip4dWbmDRfH6Eixy48TG-WKi_M8XnCT=w1032-h102-no?authuser=1)](FE)