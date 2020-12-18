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
Instructions
Develop the following instructions in Spark with the Scala programming language.

Objective:
The goal of this hands-on test is to try to group customers from specific regions of a wholesaler. This based on the sales of some product categories.

The data sources are in the repository:
https://github.com/jcromerohdz/BigData/blob/master/Spark_clustering/Wholesale%20customers%20data.csv

- Import a simple Spark session.
- Use lines of code to minimize errors
- Create an instance of the Spark session
- Import the K library means for the clustering algorithm.
- Load the wholesale customer data dataset
- Select the following columns: Fresh, Milk, Grocery, Frozen, Detergents_Paper, Delicassen and call this set feature_data
- Import Vector Assembler and Vector
- Create a new Vector Assembler object for the feature columns as an input set, remembering that there are no labels
- Use the assembler object to transform feature_data
- Create a Kmeans model with K = 3
- Evaluate the groups using within the WSSSE Sum of Squared Errors set and print the centroids.

## Development

1. Import a simple Spark session.

```scala
import org.apache.spark.sql.SparkSession
```

2. Use lines of code to minimize errors

```scala
import org.apache.log4j._
Logger.getLogger("org").setLevel(Level.ERROR)
```

3. Create an instance of the Spark session

```scala
val spark = SparkSession.builder().appName("LinearRegressionAssigment").getOrCreate()
```

4. Import the K library means for the clustering algorithm.

```scala
import org.apache.spark.ml.clustering.KMeans
```

5. Load the wholesale customer data dataset

```scala
val data = spark.read.option("header", "true").option("inferSchema","true")csv("C:\\Users\\Carlos Bojorquez\\Desktop\\Noveno semestre\\Datos Masivones\\BigData\\Spark_clustering\\Wholesale_customers_data.csv")

```

6. Select the following columns: Fresh, Milk, Grocery, Frozen, Detergents_Paper, Delicassen and call this set feature_data

```scala
val  fd  = data.select("Fresh","Milk","Grocery","Frozen","Detergents_Paper","Delicassen")
fd.show(5)
```
[![](https://lh3.googleusercontent.com/pw/ACtC-3dWRGCMqma-wmjDiL9SuBaBL0t9_U5dffqKpQlZHCZZARj-SgqGxi_s7vCNa4sZQP88JZEgWZ8TBTocbs82kxNNu6JsFF3dU9wS14K_0an0DN_s_tYYn_25KBHyCIDhjQEXyL39f9XCyCWa1fThATeR=w508-h214-no?authuser=1)](FD)

7. Import Vector Assembler and Vector

```scala
import org.apache.spark.ml.feature.VectorAssembler
import org.apache.spark.ml.linalg.Vector
```

8. Create a new Vector Assembler object for the feature columns as an input set, remembering that there are no labels

```scala
val assembler = new VectorAssembler() .setInputCols(Array("Fresh","Milk","Grocery","Frozen","Detergents_Paper","Delicassen")).setOutputCol("features")
```

9. Use the assembler object to transform feature_data

```scala
val features = assembler.transform(fd)
features.show(5)
```
[![](https://lh3.googleusercontent.com/pw/ACtC-3eIuK0K19X_KkQo5adC1hprzXwbmHAgQtPX78dV4mEgf_M484llbhyrt22viOVp6e8W08AEIH1ioO8hZKEYiaBaw9MuLce8IxonhI8W4amBKiodreeQcSuCrY9SXJFCjDfX3FTsgxNyN-PkCHwHik3u=w691-h209-no?authuser=1)](Features)

10. Create a Kmeans model with K = 3

```scala
val kmeans = new KMeans().setK(3).setMaxIterations(5)
val model = kmeans.fit(features)
```

11. Evaluate the groups using within the WSSSE Sum of Squared Errors set and print the centroids.

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
