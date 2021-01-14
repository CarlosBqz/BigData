<div align="center">

**Instituto Tecnológico de Tijuana**

Departamento de Ciencias y Computación

Ingeniería en Sistemas Computacionales
 
 [![](https://upload.wikimedia.org/wikipedia/commons/2/2e/ITT.jpg)](https://upload.wikimedia.org/wikipedia/commons/2/2e/ITT.jpg)

**Title:**

Unit 4 Project

**Subject:**

BDD-1704 SC9A Datos Masivos

**Unit:**

IV

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

Tijuana, Baja California, January 14, 2021. 
</div>

## Introduction

In this documentation we will talk about four different Machine Learning algorithms used in the field of big data. Among them are decision trees, logistic regression, multilayer perceptron, and SVM.

A brief explanation will be given about how each of them works, as well as a brief comparison of their performance, in which the time it takes for the algorithm to complete, the percentage of accuracy it presents and the percentage of error of the same. At the end of this document brief conclusions will be given on the results obtained.

## Theoretical framework
#### Big data
Massive data, also called big data or Big Data, are large amounts of information that, due to their volume, variety and speed of obtaining, require specialized technologies and methods for their use.
- Like any other data set, they can have biases and errors.
- The set of techniques to process the information contained in large databases is called Big Data Analytics, such as data mining, computer learning (machine learning) and network analysis.
- Its benefits can reach all sectors of society; companies would increase their operating margins by up to 60% and public sector agencies would generate large savings.
- It is an activity with high added value and the technology to implement it is already accessible, but its responsible use represents a legislative challenge that must be addressed.
- In Mexico there are federal laws that establish the legal framework for the use of personal data.
- At the international level, the legal frameworks of the United States of America (USA) and the European Union are in contrast to each other. [1]

#### Machine learning
Machine learning is a branch of artificial intelligence (AI) focused on creating applications that learn from data and improve their accuracy over time without being programmed to do so.

In data science, an algorithm is a sequence of statistical processing steps. In machine learning, algorithms are 'trained' to find patterns and characteristics in massive amounts of data in order to make decisions and predictions based on new data. The better the algorithm, the more accurate the decisions and predictions will be as it processes more data. [2]

#### Logistic regression
Logistic Regression is a multivariate statistical technique that allows us to estimate the relationship between a non-metric dependent variable, particularly dichotomous, and a set of independent metric or non-metric variables.

The Logistic Regression Analysis has the same strategy as the Multiple Linear Regression Analysis, which differs essentially from the Logistic Regression Analysis because the dependent variable is metric; In practice, the use of both techniques are very similar, although their mathematical approaches are different. The dependent variable or response is not continuous, but discrete (generally it takes values ​​1.0). The explanatory variables can be quantitative or qualitative; and the equation of the model is not a starting linear function, but an exponential one; although, by simple logarithmic transformation, it can finally be presented as a linear function. [3]

#### Decision Tree
A decision tree classifies data items by asking a series of questions about the characteristics associated with the items. Each question is contained in a node and each internal node points to a child node for each possible answer to your question. The questions thus form a hierarchy, encoded as a tree. In the simplest form, we ask yes or no questions, and each internal node has a "yes" child and a "no" child. An element is classified into a class by following the path from the top node, the root, to a node without children, a leaf, according to the answers that apply to the element under consideration. An item is assigned to the class that has been associated with the reaching sheet. In some variations, each sheet contains a probability distribution over the classes that estimates the conditional probability that an item that reaches the sheet belongs to a given class. [4]

#### SVM
Support vector machines (SVM) are particular linear classifiers that are based on the principle of margin maximization. They perform structural risk minimization, which improves the complexity of the classifier in order to achieve excellent generalization performance. The SVM performs the classification task by constructing, in a higher dimensional space, the hyperplane that optimally separates the data into two categories.

#### Multilayer Perceptron
The perceptron is very useful for classifying data sets that can be separated linearly. They run into serious limitations with data sets that do not fit this pattern as discovered with the XOR problem. The XOR problem shows that for any four-point classification there exists a set that is not linearly separable.

MultiLayer Perceptron (MLP) breaks this restriction and classifies data sets that are not linearly separable. They do this by using a more robust and complex architecture to learn regression and classification models for difficult data sets.

The Perceptron consists of an input layer and an output layer that are completely connected. MLPs have the same input and output layers, but can have multiple

## Implementation

Para llevar a cabo el desarrollo del presente proyecto utilizamos las herramientas Apache Spark para programar en el lenguaje Scala.

#### ¿Qué es Apache Spark?
Apache Spark es un framework de programación para procesamiento de datos distribuidos diseñado para ser rápido y de propósito general. Como su propio nombre indica, ha sido desarrollada en el marco del proyecto Apache, lo que garantiza su licencia Open Source. [7]

#### ¿Qué es Scala?
Scala es un lenguaje de programación moderno multi-paradigma diseñado para expresar patrones de programación comunes de una forma concisa, elegante, y con tipado seguro. Integra fácilmente características de lenguajes orientados a objetos y funcionales. [8]

##### A continuación presentaremos todas las librerías necesarias para desarrollar los algoritmos SVM, Logistic Regression, Decision Trees y Multilayer Perceptron:
```scala
import org.apache.spark.ml.classification.LinearSVC
import org.apache.spark.ml.Pipeline
import org.apache.spark.ml.classification.DecisionTreeClassifier
import org.apache.spark.ml.classification.DecisionTreeClassificationModel
import org.apache.spark.ml.feature.IndexToString
import org.apache.spark.mllib.evaluation.MulticlassMetrics
import org.apache.spark.ml.linalg.Vectors
import org.apache.spark.ml.classification.LogisticRegression
import org.apache.spark.ml.feature.VectorIndexer
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.types.IntegerType
import org.apache.spark.ml.evaluation.MulticlassClassificationEvaluator
import org.apache.spark.ml.classification.MultilayerPerceptronClassifier
import org.apache.spark.ml.feature.StringIndexer
import org.apache.spark.ml.feature.VectorAssembler
```
#### Código para aplicar SVM al dataset bank.csv:
```scala
//Start timer
val startTimeMillis = System.currentTimeMillis()

// Import all the libraries that we will use
import org.apache.spark.sql.SparkSession
import org.apache.spark.mllib.evaluation.MulticlassMetrics
import org.apache.spark.ml.Pipeline
import org.apache.spark.ml.linalg.Vectors
import org.apache.spark.ml.classification.LinearSVC
import org.apache.spark.ml.classification.LogisticRegression
import org.apache.spark.ml.feature.StringIndexer
import org.apache.spark.ml.feature.VectorIndexer
import org.apache.spark.ml.feature.VectorAssembler
```
```scala
// Error logger 
import org.apache.log4j._
Logger.getLogger("org").setLevel(Level.ERROR)
// Creating a spark session named SVM
val spark = SparkSession.builder.appName("SVM").getOrCreate()
```
```scala
// Importing the dataframe bank
val df  = spark.read.option("header","true").option("inferSchema", "true").option("delimiter",";").format("csv").load("C:/Users/Carlos Bojorquez/Desktop/Noveno semestre/bank.csv")
```
```scala
// Applying index 
val labelIndexer = new StringIndexer().setInputCol("y").setOutputCol("indexedLabel").fit(df)
val indexed = labelIndexer.transform(df).drop("y").withColumnRenamed("indexedLabel", "label")
```
```scala
//Vector of the numeric category columns.
val vectorFeatures = (new VectorAssembler().setInputCols(Array("balance","day","duration","pdays","previous")).setOutputCol("features"))
```
```scala
//Transforming the indexed value.
val features = vectorFeatures.transform(indexed)
```
```scala
//Renaming the column y as label.
val featuresLabel = features.withColumnRenamed("y", "label")
```
```scala
//Union of label and features as dataIndexed.
val dataIndexed = featuresLabel.select("label","features")
```
```scala
//Creation of labelIndexer and featureIndexer for the pipeline, Where features with distinct values > 4, are treated as continuous.
val labelIndexer = new StringIndexer().setInputCol("label").setOutputCol("indexedLabel").fit(dataIndexed)
val featureIndexer = new VectorIndexer().setInputCol("features").setOutputCol("indexedFeatures").setMaxCategories(4).fit(dataIndexed)
```
```scala
//Training data as 70% and test data as 30%.
val Array(training, test) = dataIndexed.randomSplit(Array(0.7, 0.3))
```
```scala
//Linear Support Vector Machine object.
val supportVM = new LinearSVC().setMaxIter(10).setRegParam(0.1)
```
```scala
//Fitting the trainingData into the model.
val model = supportVM.fit(training)
```
```scala
//Transforming testData for the predictions.
val predictions = model.transform(test)
```
```scala
//Obtaining the metrics.
val predictionAndLabels = predictions.select($"prediction",$"label").as[(Double, Double)].rdd
val metrics = new MulticlassMetrics(predictionAndLabels)
```
```scala
//Confusion matrix.
println("Confusion matrix:")
println(metrics.confusionMatrix)
```
```scala
//Accuracy and Test Error.
println("Accuracy: " + metrics.accuracy) 
println(s"Test Error = ${(1.0 - metrics.accuracy)}")
```
```scala
val endTimeMillis = System.currentTimeMillis()
val durationSeconds = (endTimeMillis - startTimeMillis) / 1000
```
```scala
//Print the time in seconds that took the whole algorithm to compile
println(durationSeconds)
```

#### Código para aplicar Logistic Regression al dataset bank.csv:
```scala
//Start timer
val startTimeMillis = System.currentTimeMillis()

// Importing this libraries is required in order to get the example done.
import org.apache.spark.sql.SparkSession
import org.apache.spark.mllib.evaluation.MulticlassMetrics
import org.apache.spark.ml.linalg.Vectors
import org.apache.spark.ml.classification.LogisticRegression
import org.apache.spark.ml.feature.VectorAssembler
import org.apache.spark.ml.feature.StringIndexer
import org.apache.spark.ml.feature.VectorIndexer
```
```scala
//Error level code.
import org.apache.log4j._
Logger.getLogger("org").setLevel(Level.ERROR)

//Spark session.
val spark = SparkSession.builder.appName("LogisticRegression").getOrCreate()
```
```scala
//Reading the csv file.
val df  = spark.read.option("header","true").option("inferSchema", "true").option("delimiter",";").format("csv").load("C:/Users/Carlos Bojorquez/Desktop/Noveno semestre/bank.csv")
```
```scala
//Indexing.
val labelIndexer = new StringIndexer().setInputCol("y").setOutputCol("indexedLabel").fit(df)
val indexed = labelIndexer.transform(df).drop("y").withColumnRenamed("indexedLabel", "label")

//Vector of the numeric category columns.
val vectorFeatures = (new VectorAssembler().setInputCols(Array("balance","day","duration","pdays","previous")).setOutputCol("features"))
```
```scala
//Transforming the indexed value.
val features = vectorFeatures.transform(indexed)
```
```scala
//Renaming the column y as label.
val featuresLabel = features.withColumnRenamed("y", "label")
```
```scala
//Union of label and features as dataIndexed.
val dataIndexed = featuresLabel.select("label","features")
```
```scala
//Training data as 70% and test data as 30%.
val Array(trainingData, testData) = dataIndexed.randomSplit(Array(0.7, 0.3))
```
```scala
//Logistic regression object.
val logisticReg = new LogisticRegression().setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8).setFamily("multinomial")
```
```scala
//Fitting the model with the training data.
val model = logisticReg.fit(trainingData)
```
```scala
//Making the predictions transforming the testData.
val predictions = model.transform(testData)
```
```scala
//Obtaining the metrics.
val predictionAndLabels = predictions.select($"prediction",$"label").as[(Double, Double)].rdd
val metrics = new MulticlassMetrics(predictionAndLabels)
```
```scala
//Confusion matrix.
println("Confusion matrix:")
println(metrics.confusionMatrix)
```
```scala
//Accuracy and Test Error.
println("Accuracy: " + metrics.accuracy) 
println(s"Test Error: ${(1.0 - metrics.accuracy)}")

val endTimeMillis = System.currentTimeMillis()
val durationSeconds = (endTimeMillis - startTimeMillis) / 1000
```
```scala
//Print the time in seconds that took the whole algorithm to compile
println(durationSeconds)
```

#### Código para aplicar Decision Tree al dataset bank.csv:
```scala
//Start timer
val startTimeMillis = System.currentTimeMillis()

// Importing this libraries is required in order to get the example done.
import org.apache.spark.sql.SparkSession
import org.apache.spark.ml.Pipeline
import org.apache.spark.ml.linalg.Vectors
import org.apache.spark.ml.evaluation.MulticlassClassificationEvaluator
import org.apache.spark.ml.classification.DecisionTreeClassifier
import org.apache.spark.ml.classification.DecisionTreeClassificationModel
import org.apache.spark.ml.feature.IndexToString
import org.apache.spark.ml.feature.StringIndexer
import org.apache.spark.ml.feature.VectorIndexer
import org.apache.spark.ml.feature.VectorAssembler
```
```scala
//Error level code.
import org.apache.log4j._
Logger.getLogger("org").setLevel(Level.ERROR)

//Spark session.
val spark = SparkSession.builder.appName("DecisionTree").getOrCreate()
```
```scala
//Reading the csv file.
val df  = spark.read.option("header","true").option("inferSchema", "true").option("delimiter",";").format("csv").load("C:/Users/Carlos Bojorquez/Desktop/Noveno semestre/bank.csv")
```
```scala
//Indexing.
val labelIndexer = new StringIndexer().setInputCol("y").setOutputCol("indexedLabel").fit(df)
val indexed = labelIndexer.transform(df).drop("y").withColumnRenamed("indexedLabel", "label")

//Vector of the numeric category columns.
val vectorFeatures = (new VectorAssembler().setInputCols(Array("balance","day","duration","pdays","previous")).setOutputCol("features"))
```
```scala
//Transforming the indexed value.
val features = vectorFeatures.transform(indexed)
```
```scala
//Renaming the column y as label.
val featuresLabel = features.withColumnRenamed("y", "label")
```
```scala
//Union of label and features as dataIndexed.
val dataIndexed = featuresLabel.select("label","features")
```
```scala
//Creation of labelIndexer and featureIndexer for the pipeline, Where features with distinct values > 4, are treated as continuous.
val labelIndexer = new StringIndexer().setInputCol("label").setOutputCol("indexedLabel").fit(dataIndexed)
val featureIndexer = new VectorIndexer().setInputCol("features").setOutputCol("indexedFeatures").setMaxCategories(4).fit(dataIndexed)
```
```scala
//Training data as 70% and test data as 30%.
val Array(training, test) = dataIndexed.randomSplit(Array(0.7, 0.3))
```
```scala
//Creating the Decision Tree object.
val decisionTree = new DecisionTreeClassifier().setLabelCol("indexedLabel").setFeaturesCol("indexedFeatures")
```
```scala
//Creating the Index to String object.
val labelConverter = new IndexToString().setInputCol("prediction").setOutputCol("predictedLabel").setLabels(labelIndexer.labels)
```
```scala
//Creating the pipeline with the objects created before.
val pipeline = new Pipeline().setStages(Array(labelIndexer, featureIndexer, decisionTree, labelConverter))
```
```scala
//Fitting the model with training data.
val model = pipeline.fit(training)
```
```scala
//Making the predictions transforming the testData.
val predictions = model.transform(test)

//Showing the predictions.
predictions.select("predictedLabel", "label", "features").show(5)

//Creating the evaluator.
val evaluator = new MulticlassClassificationEvaluator().setLabelCol("indexedLabel").setPredictionCol("prediction").setMetricName("accuracy")

//Accuracy.
val accuracy = evaluator.evaluate(predictions)

//Accuracy and Test Error.
println(s"Accuracy: ${(accuracy)}")
println(s"Test Error: ${(1.0 - accuracy)}")

val endTimeMillis = System.currentTimeMillis()
val durationSeconds = (endTimeMillis - startTimeMillis) / 1000

//Print the time in seconds that took the whole algorithm to compile
println(durationSeconds)
```

#### Código para aplicar Multilayer Perceptron al dataset bank.csv:
```scala
//Start timer
val startTimeMillis = System.currentTimeMillis()

//Necessary libraries.
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.types.IntegerType
import org.apache.spark.ml.evaluation.MulticlassClassificationEvaluator
import org.apache.spark.ml.classification.MultilayerPerceptronClassifier
import org.apache.spark.ml.feature.StringIndexer 
import org.apache.spark.ml.feature.VectorAssembler

//Error level code.
import org.apache.log4j._
Logger.getLogger("org").setLevel(Level.ERROR)

//Spark session.
val spark = SparkSession.builder.appName("MultilayerPerceptron").getOrCreate()
```
```scala
//Reading the csv file.
val df  = spark.read.option("header","true").option("inferSchema", "true").option("delimiter",";").format("csv").load("C:/Users/Carlos Bojorquez/Desktop/Noveno semestre/bank.csv")

//Indexing.
val labelIndexer = new StringIndexer().setInputCol("y").setOutputCol("indexedLabel").fit(df)
val indexed = labelIndexer.transform(df).drop("y").withColumnRenamed("indexedLabel", "label")

//Vector of the numeric category columns.
val vectorFeatures = (new VectorAssembler().setInputCols(Array("balance","day","duration","pdays","previous")).setOutputCol("features"))

//Transforming the indexed value.
val features = vectorFeatures.transform(indexed)

//Fitting indexed and finding labels 0 and 1.
val labelIndexer = new StringIndexer().setInputCol("label").setOutputCol("indexedLabel").fit(indexed)
```
```scala
//Splitting the data in 70% and 30%.
val splits = features.randomSplit(Array(0.7, 0.3))
val trainingData = splits(0)
val testData = splits(1)
```
```scala
//Creating the layers array.
val layers = Array[Int](5, 4, 1, 2)

//Creating the Multilayer Perceptron object of the Multilayer Perceptron Classifier.
val multilayerP = new MultilayerPerceptronClassifier().setLayers(layers).setBlockSize(128).setSeed(1234L).setMaxIter(100)  

//Fitting trainingData into the model.
val model = multilayerP.fit(trainingData)

//Transforming the testData for the predictions.
val prediction = model.transform(testData)

//Selecting the prediction and label columns.
val predictionAndLabels = prediction.select("prediction", "label")

//Creating a Multiclass Classification Evaluator object.
val evaluator = new MulticlassClassificationEvaluator().setMetricName("accuracy")
```
```scala
//Accuracy and Test Error.
println(s"Accuracy: ${evaluator.evaluate(predictionAndLabels)}")
println(s"Test Error: ${1.0 - evaluator.evaluate(predictionAndLabels)}")

val endTimeMillis = System.currentTimeMillis()
val durationSeconds = (endTimeMillis - startTimeMillis) / 1000

//Print the time in seconds that took the whole algorithm to compile
println(durationSeconds)
```

## Results

#### SVM
| Attempt | Time (s) | Accuracy (%) | Error (%) | Algorithm |
| ------- | -------- | ------------ | --------- | --------- |
| 1       | 19       | 86.78        | 13.21     | SVM       |
| 2       | 19       | 89.67        | 10.32     | SVM       |
| 3       | 20       | 88.83        | 11.16     | SVM       |
| 4       | 19       | 88.48        | 11.51     | SVM       |
| 5       | 19       | 87.67        | 12.32     | SVM       |
| 6       | 19       | 89.13        | 10.86     | SVM       |
| 7       | 18       | 88.14        | 11.85     | SVM       |
| 8       | 19       | 88.73        | 11.26     | SVM       |
| 9       | 19       | 87.82        | 12.17     | SVM       |
| 10      | 19       | 88.49        | 11.5      | SVM

| Average |
| ------- |
| 19      |
| 88.374  |
| 11.616  |
| SVM     |
*Todos los tests se llevaron a cabo de manera individual.


#### Logistic Regression
| Attempt | Time (s) | Accuracy (%) | Error (%) | Algorithm           |
| ------- | -------- | ------------ | --------- | ------------------- |
| 1       | 26       | 88.55        | 11.44     | Logistic Regression |
| 2       | 28       | 88.64        | 11.35     | Logistic Regression |
| 3       | 29       | 87.88        | 12.11     | Logistic Regression |
| 4       | 28       | 88.85        | 11.14     | Logistic Regression |
| 5       | 27       | 89.19        | 10.8      | Logistic Regression |
| 6       | 27       | 86.86        | 13.13     | Logistic Regression |
| 7       | 27       | 89.13        | 10.86     | Logistic Regression |
| 8       | 25       | 87.69        | 12.3      | Logistic Regression |
| 9       | 25       | 89.05        | 10.94     | Logistic Regression |
| 10      | 28       | 88.03        | 11.96     | Logistic Regression |

| Average             |
| ------------------- |
| 27                  |
| 88.387              |
| 11.603              |
| Logistic Regression |
*Todos los tests se llevaron a cabo de manera individual.

#### Decision Tree

Attempt #
Time (s)
Accuracy (%)
Error (%)
Algorithm
1
18
88.52
11.47
Decision Tree
2
22
89.18
10.81
Decision Tree
3
21
89.69
10.3
Decision Tree
4
22
87.91
12.08
Decision Tree
5
20
88.42
11.57
Decision Tree
6
19
88.09
11.9
Decision Tree
7
18
86.87
13.12
Decision Tree
8
21
89.15
10.84
Decision Tree
9
22
87.88
12.11
Decision Tree
10
29
88.15
11.84
Decision Tree
Average
21.2
88.386
11.604
Decision Tree
*Todos los tests se llevaron a cabo de manera individual

#### Multilayer Perceptron
Attempt #
Time (s)
Accuracy (%)
Error (%)
Algorithm
1
21
88.61
11.38
Multilayer Perceptron
2
19
88.9
11.09
Multilayer Perceptron
3
23
89.7
10.29
Multilayer Perceptron
4
21
88.64
11.35
Multilayer Perceptron
5
21
86.86
13.13
Multilayer Perceptron
6
29
88.32
11.67
Multilayer Perceptron
7
23
88.21
11.78
Multilayer Perceptron
8
27
88.51
11.48
Multilayer Perceptron
9
27
89.48
10.51
Multilayer Perceptron
10
24
87.24
12.75
Multilayer Perceptron
Average
23.5
88.447
11.543
Multilayer Perceptron
*Todos los tests se llevaron a cabo de manera individual

## Conclusions

As shown in the final results of each of the methods, we could observe that all have an approximate precision of 88.3% except for the perceptron algorithm method, which obtained a precision of 88.447%, these being results already averaged, in terms of the algorithm's speed. with the best performance was support vector machine, which has an average time of 19 seconds, which makes it the fastest for this case, against the others which have times of 23 seconds perceptron, 21 seconds decision tree and 27 seconds logistic regression.

We can see that we have 2 possible scenarios, one where what we want is the maximum precision that can be obtained, in which the most prominent algorithm would be the Multilayer Perceptron or have the algorithm with the shortest times, which would be Support Vector Machine

## References

[1] Dr. Alexandro Heiblum Robles. (2018). Los datos masivos (Big Data). 10 de enero de 2021, de INCyTU
[2] IBM Cloud Education. (2020). What is Machine Learning?. 10 de enero de 2021, de IBM
[3] Celia Mercedes Salcedo Poma. (-). Estimación de la ocurrencia de incidencias en declaraciones de importación. 10 de enero de 2021, de Universidad Nacional de San Marcos
[4] Carl Kingsford and Steven L Salzberg. (2008). What are decision trees?. 10 de enero de 2021, de National Center for Biotechnology Information
[5] Mathias M. AdankonMohamed Cheriet. (2009). Support Vector Machine. 10 de enero de 2021, de Springer
[6] DeepAI. (2021). Multilayer Perceptron. 10 de enero de 2021, de DeepAI
[7] ESIC. (2018). Apache Spark: Introducción, qué es y cómo funciona. 10 de enero de 2021, de ESIC
[8] Scala. (2021). Introducción | Scala Documentation. 10 de enero de 2021, de The Scala Programming Language
