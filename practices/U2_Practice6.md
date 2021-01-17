# Practice 6: MULTI LAYER PERCEPTRON CLASSIFIER EXERCIST

##### 1. Import MultilayerPerceptron
```scala
import org.apache.spark.ml.classification.MultilayerPerceptronClassifier
import org.apache.spark.ml.evaluation.MulticlassClassificationEvaluator

```
This code below its optional, it helps to configure errors in the code.
```scala
// Opcional: Utilice el siguiente codigo para configurar errores
import org.apache.log4j._
Logger.getLogger("org").setLevel(Level.ERROR)
```

##### 2. Initiate Spark simple sesion
```scala
// Inicie una simple Sesion Spark
import org.apache.spark.sql.SparkSession

val spark = SparkSession
      .builder
      .appName("MultilayerPerceptronClassifierExample")
      .getOrCreate()
```

##### 3. Load the Data Frame 
```scala
// Utilice Spark para el archivo csv Clean-Ecommerce .
 val data = spark.read.format("libsvm")
      .load("sample_multiclass_classification_data.txt")
```
To print the schema we run the following code:
```scala
// Imprima el schema en el DataFrame.
data.printSchema()

// Imprima un renglon de ejemplo del DataFrane.
data.show(1)
```
##### 4. Split trining data and test
```scala
   // Se dividen los datos en entrenamiento y prueba
    val splits = data.randomSplit(Array(0.6, 0.4), seed = 1234L)
    val train = splits(0)
    val test = splits(1)
```
##### 5. Put layer of neural network
```scala
    // Se especifican las capas de la red neuronal:
    // La capa de entrada es de tamaño 4 (caracteristicas), dos capas intermedias
    // una de tamaño 5 y la otra de tamaño 4
    // y 3 de salida (las clases)
    val layers = Array[Int](4, 5, 4, 3)
```
##### 6. Training parameters are set
```scala
   // Se establecen los parametros de entrenamiento
    val trainer = new MultilayerPerceptronClassifier()
      .setLayers(layers)
      .setBlockSize(128)
      .setSeed(1234L)
      .setMaxIter(100)
```
##### 7.Trainning
```scala
   // Se entrena el modelo
    val model = trainer.fit(train)
```
##### 8. The accuracy of the data is calculated
```scala
   // Se calcula la precision de los datos de prueba
    val result = model.transform(test)
    val predictionAndLabels = result.select("prediction", "label")
    val evaluator = new MulticlassClassificationEvaluator()
      .setMetricName("accuracy")
```
##### 9. Pirnting model accuracy
```scala
   // Se imprime la exactidud del modelo
    println(s"Test set accuracy = ${evaluator.evaluate(predictionAndLabels)}")
    // $example off$
 ```


