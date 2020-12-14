<div align="center">

**Instituto Tecnológico de Tijuana**

Departamento de Ciencias y Computación

Ingeniería en Sistemas Computacionales
 
 [![](https://upload.wikimedia.org/wikipedia/commons/2/2e/ITT.jpg)](https://upload.wikimedia.org/wikipedia/commons/2/2e/ITT.jpg)

**Title:**

Evaluation Unit 2

**Subject:**

BDD-1704 SC9A Datos Masivos

**Unit:**

II

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

Tijuana, Baja California, December 15, 2020. 
</div>

## Instructions
**Develop the following instructions in Spark with the Scala programming language, using only Spark Machine Learning Mllib library documentation and Google.**

1. Load into a dataframe Iris.csv found in https://github.com/jcromerohdz/iris, prepare the necessary data to be processed by the following algorithm (Important, this cleaning must be done by using a Scala script in Spark). to. Use the Spark Mllib library the corresponding Machine Learning algorithm a multilayer perceptron
2. What are the names of the columns?
3. What is the scheme like?
4. Print the first 5 columns.
5. Use the describe () method to learn more about the data in the DataFrame.
6. Make the relevant transformation for the categorical data which will be
7. our labels to be classified.
8. Build the classification model and explain its architecture.
9. Print the model results

## Development

### 1. Loading the Dataframe
##### First we import the following libraries
```scala
import org.apache.spark.ml.classification.MultilayerPerceptronClassifier
import org.apache.spark.ml.evaluation.MulticlassClassificationEvaluator
import org.apache.spark.ml.Pipeline
import org.apache.spark.ml.classification.DecisionTreeClassifier
import org.apache.spark.ml.evaluation.MulticlassClassificationEvaluator
import org.apache.spark.ml.feature.{IndexToString, StringIndexer, VectorIndexer}
import org.apache.spark.ml.feature.VectorAssembler
import org.apache.spark.ml.feature.StringIndexer
```

##### And the Error logger (optional)
```scala
import org.apache.log4j._
Logger.getLogger("org").setLevel(Level.ERROR)
```

##### Initiate a Simple Session in Spark
```scala
import org.apache.spark.sql.SparkSession
val spark = SparkSession.builder().appName("LinearRegressionAssigment").getOrCreate()

```
##### Load the data stored in LIBSVM format as a DataFrame.
```scala
val data = spark.read.option("header", "true").option("inferSchema","true")csv("iris.csv")

```
### 2. Column Names
##### To get the name of the columns we run the following code:
```scala
data.columns.seq
```

### 3. Scheme of the Dataframe
```scala
data.printSchema()
```

### 4. First 5 rows of the Dataframe
```scala
data.show(5)
```

### 5. Describe function
```scala
data.describe()
```
### 6. Transform the data

```scala
val assembler = new VectorAssembler() .setInputCols(Array("sepal_length", "sepal_width", "petal_length", "petal_width")).setOutputCol("features")
val features = assembler.transform(data)
features.show(5)
```
##### Index labels, adding metadata to the label column. Fit on whole dataset to include all labels in index.
```scala
val labelIndexer = new StringIndexer().setInputCol("species").setOutputCol("indexedLabel").fit(features)
println(s"Found labels: ${labelIndexer.labels.mkString("[", ", ", "]")}")
```
##### Automatically identify categorical features, and index them.
##### Set maxCategories so features with > 4 distinct values are treated as continuous.
```scala
val featureIndexer = new VectorIndexer().setInputCol("features").setOutputCol("indexedFeatures").setMaxCategories(4).fit(features)
```

### 7. Building the model

##### Split the data into train and test sets
```scala
val splits = features.randomSplit(Array(0.6, 0.4))
val trainingData = splits(0)
val testData = splits(1)
```

##### Specify layers for the neural network: input layer of size 4 (features), two intermediate of size 5 and 4 and output of size 3 (classes)
```scala
val layers = Array[Int](4, 5, 4, 3)
```

##### Create the trainer and set its parameters
```scala
val trainer = new MultilayerPerceptronClassifier().setLayers(layers).setLabelCol("indexedLabel").setFeaturesCol("indexedFeatures").setBlockSize(128).setSeed(System.currentTimeMillis).setMaxIter(200)
```

##### Convert indexed labels back to original labels.
```scala
val labelConverter = new IndexToString().setInputCol("prediction").setOutputCol("predictedLabel").setLabels(labelIndexer.labels)
```

##### Chain indexers and MultilayerPerceptronClassifier in a Pipeline.
```scala
val pipeline = new Pipeline().setStages(Array(labelIndexer, featureIndexer, trainer, labelConverter))
```

##### Train model. This also runs the indexers.
```scala
val model = pipeline.fit(trainingData)
```

##### Make predictions.
```scala
val predictions = model.transform(testData)

predictions.show(5)
```

##### Select (prediction, true label) and compute test error.
```scala
val evaluator = new MulticlassClassificationEvaluator().setLabelCol("indexedLabel").setPredictionCol("prediction").setMetricName("accuracy")
val accuracy = evaluator.evaluate(predictions)
```



### 8. Printing the result
```scala
println("Test Error = " + (1.0 - accuracy))
```
