# Practice 7: NAIVE BAYES
#### 1. Importing libraries.
```scala
import org.apache.spark.ml.classification.NaiveBayes
import org.apache.spark.ml.evaluation.MulticlassClassificationEvaluator
```
#### 2. Loading data.
```scala
val data = spark.read.format("libsvm").load("C:/Users/Sebas/Desktop/sample_libsvm_data.txt")
data.show(2)
```
#### 3. Split data for training
```scala
val Array(trainingData, testData) = data.randomSplit(Array(0.7, 0.3), seed = 1234L)
```
#### 4. Training model NB.
```scala
val model = new NaiveBayes().fit(trainingData)
```
#### 5. Print columns.
```scala
val predictions = model.transform(testData)
predictions.show()
```
#### 6. Select columns  (prediccion, etiqueta de cierto) y calcular errores de prueba
```scala
val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("accuracy")

val accuracy = evaluator.evaluate(predictions)

println(s"Test set accuracy = $accuracy")
```
