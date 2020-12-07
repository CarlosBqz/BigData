# Practice 2: Logistic Regression

#### Declaring libraries and importing the data

First of all, import the libraries to apply the logistic regression model and to create a spark session. Then we create the spark session aswell as import the dataframe "advertising". To finalize this section, we print the schema.

Import the libraries
```scala
import org.apache.spark.ml.classification.LogisticRegression
import org.apache.spark.sql.SparkSession
```

Optional to import the error logger
```scala
import org.apache.log4j._
Logger.getLogger("org").setLevel(Level.ERROR)
```

Create a spark session
```scala
val spark = SparkSession.builder().getOrCreate()
```

Import the dataframe
```scala
val data  = spark.read.option("header","true").option("inferSchema", "true").format("csv").load("advertising.csv")
```

Print the Schema
```scala
data.printSchema()
```
#### Deploying the data

The following code allow us to print a example using the first row

```scala
data.head(1)
	
//Printing the first row.
val colnames = data.columns
val firstrow = data.head(1)(0)
println("\n")
println("Example data row")
for(ind <- Range(1, colnames.length)){
	println(colnames(ind))
	println(firstrow(ind))
	println("\n")
}
```

#### Configure the dataframe to Machine Learning

In this section we have to configure our dataframe in a way that the linear regression model or any other machine learning model will be able to work.

Here we create a new dataframe with an extra column named Timestamp
```scala
val timedata = data.withColumn("Hour",hour(data("Timestamp")))
```

Rename the column "Clicked on Ad" to "label"
```scala
val logregdata = timedata.select(data("Clicked on Ad").as("label"), $"Daily Time Spent on Site", $"Age", $"Area Income", $"Daily Internet Usage", $"Hour", $"Male")
```

Import the libraries Vectors and VectorAssembler
```scala
import org.apache.spark.ml.feature.VectorAssembler
import org.apache.spark.ml.linalg.Vectors
```

Create a new assembler and set the columns to features
```scala
val assembler = (new VectorAssembler().setInputCols(Array("Daily Time Spent on Site","Age","Area Income","Daily Internet Usage","Hour","Male")).setOutputCol("features"))
```

Split the data in 70 and 30 using the randomSplit function
```scala
val Array(training, test) = logregdata.randomSplit(Array(0.7, 0.3), seed = 12345)
```

#### Creating a Pipeline

The pipelines are used to create trainings models and print the results of those trainings

Import the pipeline library
```scala
import org.apache.spark.ml.Pipeline

```
Create a logistic regression object
```scala
val lr = new LogisticRegression()

```
Creating a pipeline using the assembler and specifying the model.
```scala
val pipeline = new Pipeline().setStages(Array(assembler, lr))

```
Apply the fit funciton to the pipeline
```scala
val model = pipeline.fit(training)

```
Save the results of the test in a new variable named results
```scala
val results = model.transform(test)

```
#### Evaluating the model

MulticlassMetric is used to see the accuracy and the confusion matrix

Import the multiclassmetrics library
```scala
import org.apache.spark.mllib.evaluation.MulticlassMetrics

```
Converting test results to RDD using .as and .rdd
```scala
val predictionAndLabels = results.select($"prediction",$"label").as[(Double, Double)].rdd

```
Create the metrics object
```scala
val metrics = new MulticlassMetrics(predictionAndLabels)

```
Print the "Confusion Matrix"
```scala
println("Confusion matrix:")
println(metrics.confusionMatrix)
```

Show the accuracy
```scala
metrics.accuracy
```