# Practice 1: LINEAR REGRESSION EXERCISE
### Complete the commented tasks

##### 1. Import LinearRegression
```scala
import org.apache.spark.ml.regression.LinearRegression
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
val spark = SparkSession.builder().appName("LinearRegressionAssigment").getOrCreate()
```

##### 3. Load the Data Frame 
```scala
// Utilice Spark para el archivo csv Clean-Ecommerce .
val cleanecommerce = spark.read.option("header", "true").option("inferSchema","true")csv("C:/Users/Carlos Bojorquez/Desktop/Noveno semestre/Datos Masivones/BigData/Spark_Regression/Clean-Ecommerce.csv")
```
To print the schema we run the following code:
```scala
// Imprima el schema en el DataFrame.
cleanecommerce.printSchema()

// Imprima un renglon de ejemplo del DataFrane.
cleanecommerce.show(1)
```



#### Configure the dataframe to machine learning

##### 4. Transform the dataframe to take the (labels, features) form
Import the following libraries
```scala
// Importe VectorAssembler y Vectors
import org.apache.spark.ml.feature.VectorAssembler
import org.apache.spark.ml.linalg.Vectors
```
Rename the columns Yearly Amount Spent as "label" aswell as taking only the numeric column. All of this will be saved in a new data frame named df
```scala
// Renombre la columna Yearly Amount Spent como "label"
// Tambien de los datos tome solo la columa numerica 
// Deje todo esto como un nuevo DataFrame que se llame df
val df = cleanecommerce.select($"Avg Session Length", $"Time on App", $"Time on Website", $"Length of Membership", $"Yearly Amount Spent".as("label"))
```
Create a new assembler, this to transform all the columns in the df to one single column.
```scala
// Que el objeto assembler convierta los valores de entrada a un vector
// Utilice el objeto VectorAssembler para convertir la columnas de entradas del df
// a una sola columna de salida de un arreglo llamado  "features"
// Configure las columnas de entrada de donde se supone que leemos los valores.
// Llamar a esto nuevo assambler.
val assembler = new VectorAssembler().setInputCols(Array("Avg Session Length", "Time on App", "Time on Website", "Length of Membership")).setOutputCol("features")
```
Utilize the assembler to transform the two columns in the dataframe. The column will be named as label and features
```scala
// Utilice el assembler para transform nuestro DataFrame a dos columnas: label and features
//val df = 
val dataassem = assembler.transform(df).select($"label", $"features")
```

####Linear Regression

##### 5. Apply the Linear Regression Model to the dataframe

Create a new LinearRegression object
```scala
// Crear un objeto para modelo de regresion linea.
val lr = new LinearRegression()
```
Adjust the model to apply to the data in our dataframe and name this operation as lrModelo
```scala
// Ajuste el modelo para los datos y llame a este modelo lrModelo
val lrModelo = lr.fit(dataassem)
```
Make a summary of the training and print the result
```scala
// Resuma el modelo sobre el conjunto de entrenamiento imprima la salida de algunas metricas!
// Utilize metodo .summary de nuestro  modelo para crear un objeto
// llamado trainingSummary
val trainingSummary = lrModelo.summary
```
Print the coefficients and the intercept
```scala
// Imprima the  coefficients y intercept para la regresion lineal
println(s"Coefficients: ${lrModelo.coefficients} Intercept: ${lrModelo.intercept}")

Coefficients: [25.734271084670716,38.709153810828816,0.43673883558514964,61.57732375487594] Intercept: -1051.5942552990748]
```
Show the residuals
```scala
// Muestre los valores de residuals, el RMSE, el MSE, y tambien el R^2 .
trainingSummary.residuals.show()
```

|          residuals|
|-------------------|
| -6.788234090018818|
| 11.841128565326073|
| -17.65262700858966|
| 11.454889631178617|
| 7.7833824373080915|
|-1.8347332184773677|
|  4.620232401352382|
| -8.526545950978175|
| 11.012210896516763|
|-13.828032682158891|
| -16.04456458615175|
|  8.786634365463442|
| 10.425717191807507|
| 12.161293785003522|
|  9.989313714461446|
| 10.626662732649379|
|  20.15641408428496|
|-3.7708446586326545|
| -4.129505481591934|
|  9.206694655890487|