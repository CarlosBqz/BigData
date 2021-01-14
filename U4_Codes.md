## SVM

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

// Error logger 
import org.apache.log4j._
Logger.getLogger("org").setLevel(Level.ERROR)

// Creating a spark session named SVM
val spark = SparkSession.builder.appName("SVM").getOrCreate()

// Importing the dataframe bank
val df  = spark.read.option("header","true").option("inferSchema", "true").option("delimiter",";").format("csv").load("C:/Users/Carlos Bojorquez/Desktop/Noveno semestre/bank.csv")

// Applying index 
val labelIndexer = new StringIndexer().setInputCol("y").setOutputCol("indexedLabel").fit(df)
val indexed = labelIndexer.transform(df).drop("y").withColumnRenamed("indexedLabel", "label")

//Vector of the numeric category columns.
val vectorFeatures = (new VectorAssembler().setInputCols(Array("balance","day","duration","pdays","previous")).setOutputCol("features"))

//Transforming the indexed value.
val features = vectorFeatures.transform(indexed)

//Renaming the column y as label.
val featuresLabel = features.withColumnRenamed("y", "label")

//Union of label and features as dataIndexed.
val dataIndexed = featuresLabel.select("label","features")

//Creation of labelIndexer and featureIndexer for the pipeline, Where features with distinct values > 4, are treated as continuous.
val labelIndexer = new StringIndexer().setInputCol("label").setOutputCol("indexedLabel").fit(dataIndexed)
val featureIndexer = new VectorIndexer().setInputCol("features").setOutputCol("indexedFeatures").setMaxCategories(4).fit(dataIndexed)

//Training data as 70% and test data as 30%.
val Array(training, test) = dataIndexed.randomSplit(Array(0.7, 0.3))

//Linear Support Vector Machine object.
val supportVM = new LinearSVC().setMaxIter(10).setRegParam(0.1)
    
//Fitting the trainingData into the model.
val model = supportVM.fit(training)

//Transforming testData for the predictions.
val predictions = model.transform(test)

//Obtaining the metrics.
val predictionAndLabels = predictions.select($"prediction",$"label").as[(Double, Double)].rdd
val metrics = new MulticlassMetrics(predictionAndLabels)

//Confusion matrix.
println("Confusion matrix:")
println(metrics.confusionMatrix)

//Accuracy and Test Error.
println("Accuracy: " + metrics.accuracy) 
println(s"Test Error = ${(1.0 - metrics.accuracy)}")

val endTimeMillis = System.currentTimeMillis()
val durationSeconds = (endTimeMillis - startTimeMillis) / 1000

//Print the time in seconds that took the whole algorithm to compile
println(durationSeconds)


## Decision Tree 

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

//Error level code.
import org.apache.log4j._
Logger.getLogger("org").setLevel(Level.ERROR)

//Spark session.
val spark = SparkSession.builder.appName("DecisionTree").getOrCreate()

//Reading the csv file.
val df  = spark.read.option("header","true").option("inferSchema", "true").option("delimiter",";").format("csv").load("C:/Users/Carlos Bojorquez/Desktop/Noveno semestre/bank.csv")

//Indexing.
val labelIndexer = new StringIndexer().setInputCol("y").setOutputCol("indexedLabel").fit(df)
val indexed = labelIndexer.transform(df).drop("y").withColumnRenamed("indexedLabel", "label")

//Vector of the numeric category columns.
val vectorFeatures = (new VectorAssembler().setInputCols(Array("balance","day","duration","pdays","previous")).setOutputCol("features"))

//Transforming the indexed value.
val features = vectorFeatures.transform(indexed)

//Renaming the column y as label.
val featuresLabel = features.withColumnRenamed("y", "label")

//Union of label and features as dataIndexed.
val dataIndexed = featuresLabel.select("label","features")

//Creation of labelIndexer and featureIndexer for the pipeline, Where features with distinct values > 4, are treated as continuous.
val labelIndexer = new StringIndexer().setInputCol("label").setOutputCol("indexedLabel").fit(dataIndexed)
val featureIndexer = new VectorIndexer().setInputCol("features").setOutputCol("indexedFeatures").setMaxCategories(4).fit(dataIndexed)

//Training data as 70% and test data as 30%.
val Array(training, test) = dataIndexed.randomSplit(Array(0.7, 0.3))

//Creating the Decision Tree object.
val decisionTree = new DecisionTreeClassifier().setLabelCol("indexedLabel").setFeaturesCol("indexedFeatures")

//Creating the Index to String object.
val labelConverter = new IndexToString().setInputCol("prediction").setOutputCol("predictedLabel").setLabels(labelIndexer.labels)

//Creating the pipeline with the objects created before.
val pipeline = new Pipeline().setStages(Array(labelIndexer, featureIndexer, decisionTree, labelConverter))

//Fitting the model with training data.
val model = pipeline.fit(training)

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


## Logistic Regression

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

//Error level code.
import org.apache.log4j._
Logger.getLogger("org").setLevel(Level.ERROR)

//Spark session.
val spark = SparkSession.builder.appName("LogisticRegression").getOrCreate()

//Reading the csv file.
val df  = spark.read.option("header","true").option("inferSchema", "true").option("delimiter",";").format("csv").load("C:/Users/Carlos Bojorquez/Desktop/Noveno semestre/bank.csv")

//Indexing.
val labelIndexer = new StringIndexer().setInputCol("y").setOutputCol("indexedLabel").fit(df)
val indexed = labelIndexer.transform(df).drop("y").withColumnRenamed("indexedLabel", "label")

//Vector of the numeric category columns.
val vectorFeatures = (new VectorAssembler().setInputCols(Array("balance","day","duration","pdays","previous")).setOutputCol("features"))

//Transforming the indexed value.
val features = vectorFeatures.transform(indexed)

//Renaming the column y as label.
val featuresLabel = features.withColumnRenamed("y", "label")

//Union of label and features as dataIndexed.
val dataIndexed = featuresLabel.select("label","features")

//Training data as 70% and test data as 30%.
val Array(trainingData, testData) = dataIndexed.randomSplit(Array(0.7, 0.3))

//Logistic regression object.
val logisticReg = new LogisticRegression().setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8).setFamily("multinomial")

//Fitting the model with the training data.
val model = logisticReg.fit(trainingData)

//Making the predictions transforming the testData.
val predictions = model.transform(testData)

//Obtaining the metrics.
val predictionAndLabels = predictions.select($"prediction",$"label").as[(Double, Double)].rdd
val metrics = new MulticlassMetrics(predictionAndLabels)

//Confusion matrix.
println("Confusion matrix:")
println(metrics.confusionMatrix)

//Accuracy and Test Error.
println("Accuracy: " + metrics.accuracy) 
println(s"Test Error: ${(1.0 - metrics.accuracy)}")

val endTimeMillis = System.currentTimeMillis()
val durationSeconds = (endTimeMillis - startTimeMillis) / 1000

//Print the time in seconds that took the whole algorithm to compile
println(durationSeconds)


## Multilayer Perceptron

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

//Splitting the data in 70% and 30%.
val splits = features.randomSplit(Array(0.7, 0.3))
val trainingData = splits(0)
val testData = splits(1)

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

//Accuracy and Test Error.
println(s"Accuracy: ${evaluator.evaluate(predictionAndLabels)}")
println(s"Test Error: ${1.0 - evaluator.evaluate(predictionAndLabels)}")

val endTimeMillis = System.currentTimeMillis()
val durationSeconds = (endTimeMillis - startTimeMillis) / 1000

//Print the time in seconds that took the whole algorithm to compile
println(durationSeconds)
