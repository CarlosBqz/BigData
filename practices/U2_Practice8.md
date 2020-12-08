# Practice 8: One vs Rest

#### 1. Importing one vs rest libraries
```scala
import org.apache.spark.ml.classification.{LogisticRegression, OneVsRest}
import org.apache.spark.ml.evaluation.MulticlassClassificationEvaluator
```

#### 2. loading data.
```scala
var inputData = spark.read.format("libsvm").load("/opt/spark/data/mllib/sample_multiclass_classification_data.txt")
```
#### 3. split data for trainning.
```scala
val Array(train, test) = inputData.randomSplit(Array(0.8, 0.2))
```

#### 4. Creating a instance from the Logistinc Regression classifier
```scala
val classifier = new LogisticRegression().setMaxIter(10).setTol(1E-6).setFitIntercept(true)
```

#### 5. Creating a instance One Vs Rest.
```scala
val ovr = new OneVsRest().setClassifier(classifier)
```

#### 6. Trainning multi class model.
```scala
val ovrModel = ovr.fit(train)
```

#### 7. Score the model on the test data.
```scala
val predictions = ovrModel.transform(test)
```

#### 8. Get Evaluation.
```scala
val evaluator = new MulticlassClassificationEvaluator().setMetricName("accuracy")
```

#### 9.Calculates the classification error in the test data.
```scala
val accuracy = evaluator.evaluate(predictions)
println(s"Test Error = ${1 - accuracy}")
```
