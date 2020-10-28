**1-. Start a simple session in spark**

Import the following library to start a spark session
```scala
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.functions._
```
Declare the variable spark to initialize the session, this will allow us to work with data frames using sql syntax.

```scala
val spark = SparkSession.builder().getOrCreate()
```
**2-. Upload Netflix Stock CSV file, make Spark infer data types.**

To load the csv we use the following method, indicating that there is a header and providing the path of the file.
```scala
//Load the csv
val ndf = spark.read.option("header", "true").option("inferSchema","true")csv("C:\\Users\\Carlos Bojorquez\\Desktop\\Evaluacion_Unidad 1_Scala\\Netflix_2011_2016.csv")
```
**3-. What are the column names?**

To get the names of the columns of a csv, we used the columns method, this will throw an array that contains all the headers of the data frame.
```scala
//Name of the columns
ndf.columns
res4: Array[String] = Array(Date, Open, High, Low, Close, Volume, Adj Close)
```

**4-. How is the scheme?**

To print the data frame schema, we used the printSchema() method. This method will throw a list that shows the type of data that contains the csv.
```scala
ndf.printSchema()


root
 |-- Date: string (nullable = true)
 |-- Open: string (nullable = true)
 |-- High: string (nullable = true)
 |-- Low: string (nullable = true)
 |-- Close: string (nullable = true)
 |-- Volume: string (nullable = true)
 |-- Adj Close: string (nullable = true)
```

**5-. Print the first 5 columns.**

To print the first five columns, we used the select method, writing the name of the first five columns and showing just one record of the csv.
```scala
scala> ndf.select("Date", "Open", "High", "Low", "Close").show(1)
```


|               Date|      Open|              High|       Low|     Close|
|-------------------|----------|------------------|----------|----------|
|2011-10-24 00:00:00|119.100002|120.28000300000001|115.100004|118.839996|

only showing top 1 row

**6-. Use describe () to learn about the DataFrame.**
Using the describe function, this function calculate different things:
- Count: this is the total of rows that the csv contains.
- Mean: this show the mean of all the columns.
- Stddev: this show the standard deviation of all the columns.
- Min: shows the minimum value in the column.
- Max: shows the maximum value in the column.

```scala
scala> ndf.describe().show()
```

| summary | Date       | Open               | High               | Low                | Close              | Volume               | Adj Close          |
|---------|------------|--------------------|--------------------|--------------------|--------------------|----------------------|--------------------|
| count   | 1259       | 1259               | 1259               | 1259               | 1259               | 1259                 | 1259               |
| mean    | null       | 230.39351086656092 | 233.97320872915006 | 226.80127876251044 | 230.522453845909   | 2.5634836060365368E7 | 55.610540036536875 |
| stddev  | null       | 164.37456353264244 | 165.9705082667129  | 162.6506358235739  | 164.40918905512854 | 2.306312683388607E7  | 35.186669331525486 |
| min     | 2011-10-24 | 100.009999         | 100.050003         | 100.010002         | 100.040001         | 100060100            | 10.017142999999999 |
| max     | 2016-10-24 | 99.970001          | 99.93              | 99.860001          | 99.889999          | 9991800              | 99.889999          |


**7-. Create a new dataframe with a new column called “HV Ratio” which is the relationship between the price in the “High” column versus the “Volume” column of shares traded for a day.**

To create a new data frame containing an extra column, we have to use the withColumn() method, this method will ask us to write the name of the column and the value that will contain. The exercise says that the name of the column is "HV Ratio" and the values are the columns High divided by the column Volume.
```scala
//(Hint: Es una operación de columnas).
ndf.select($"High"/$"Volume").show()

val df7 = ndf.withColumn("HV Ratio", col("High")/col("Volume"))

//Printing the dataframe df7, we can appreciate the extra column at the end of the table.
scala> df7.show()
```

|      Date|             Open|              High|       Low|            Close|   Volume|         Adj Close|            HV Ratio|
|----------|-----------------|------------------|----------|-----------------|---------|------------------|--------------------|
|2011-10-24|       119.100002|120.28000300000001|115.100004|       118.839996|120460200|         16.977142|9.985040951285156E-7|
|2011-10-25|        74.899999|         79.390001| 74.249997|        77.370002|315541800|11.052857000000001|2.515989989281927E-7|
|2011-10-26|            78.73|         81.420001| 75.399997|        79.400002|148733900|         11.342857|5.474206014903126E-7|
|2011-10-27|        82.179998| 82.71999699999999| 79.249998|80.86000200000001| 71190000|11.551428999999999|1.161960907430818...|
|2011-10-28|        80.280002|         84.660002| 79.599999|84.14000300000001| 57769600|             12.02|1.465476686700271...|
|2011-10-31|83.63999799999999|         84.090002| 81.450002|        82.080003| 39653600|         11.725715|2.120614572195210...|
|2011-11-01|        80.109998|         80.999998|     78.74|        80.089997| 33016200|         11.441428|2.453341026526372E-6|
|2011-11-02|        80.709998|         84.400002| 80.109998|        83.389999| 41384000|         11.912857|2.039435578967717E-6|
|2011-11-03|        84.130003|         92.600003| 81.800003|        92.290003| 94685500|13.184285999999998| 9.77974483949496E-7|
|2011-11-04|91.46999699999999| 92.89000300000001| 87.749999|        90.019998| 84483700|             12.86|1.099502069629999...|
|2011-11-07|             91.0|         93.839998| 89.979997|        90.830003| 47485200|         12.975715|1.976194645910725...|
|2011-11-08|91.22999899999999|         92.600003| 89.650002|        90.470001| 31906000|         12.924286|2.902275528113834...|
|2011-11-09|        89.000001|         90.440001| 87.999998|        88.049999| 28756000|         12.578571|3.145082800111281E-6|
|2011-11-10|        89.290001| 90.29999699999999| 84.839999|85.11999899999999| 39614400|             12.16|2.279474054889131E-6|
|2011-11-11|        85.899997|         87.949997|      83.7|        87.749999| 38140200|         12.535714|2.305965805108520...|
|2011-11-14|        87.989998|              88.1|     85.45|        85.719999| 21811300|         12.245714|4.039190694731629...|
|2011-11-15|            85.15|         87.050003| 84.499998|        86.279999| 21372400|         12.325714|4.073010190713256...|
|2011-11-16|        86.460003|         86.460003| 80.890002|        81.180002| 34560400|11.597142999999999|2.501707242971725E-6|
|2011-11-17|            80.77|         80.999998| 75.789999|        76.460001| 52823400|         10.922857|1.533411291208063...|
|2011-11-18|             76.7|         78.999999| 76.039998|        78.059998| 34729100|         11.151428|2.274749388841058...|

only showing top 20 rows

**8-. ¿Qué día tuvo el pico mas alto en la columna “Close”?**
To only get the day and the close column, we used the select method, we indicated that we only want to show those two columns. After that we sort the data in a descending way to show the maximum value and applying the show method to just show the top of the table.
```scala
scala> ndf.select("Date", "Close").sort(desc("Close")).show(1)
```

| Date              |     Close|
|-------------------|----------|
|2015-07-13 00:00:00|707.610001|

only showing top 1 row

**9-. Write in your own words in a comment of your code. What is the meaning of the Close column "Close"?**
```scala
//Analyzing the data frame column by column, we can understand that open means the value of a netflix action at the start of the day, the column high means the top value that an action got that day and the column low its the lowest value. Knowing that, we can say that the close column means the value of a netflix action at the end of the day.
```

**10 What is the maximum and minimum of the “Volume” column?**

To get the maximum and minimum of a column, we used the "agg" function, this function allow us to use the "min" and "max" method, those methods calculate the minimum and maximum values of the column respectively. To use the method we need to specify the name of the column that we want to calculate his minimum and maximum values.

```scala
scala> ndf.agg(min("Volume"), max("Volume")).show()
```
|min(Volume)|max(Volume)|
------------|-----------|
|    3531300|  315541800|

**11. With Syntax Scala / Spark $ answer the following:**
◦ Hint: Basicamente muy parecido a la session de dates, tendran que crear otro dataframe para contestar algunos de los incisos.

**a. How many days was the “Close” column less than $ 600?**

To get the values of the close column that are below 600, we applied a filter to the csv. To do it, we use the filter method specifying the condition that will follow the data frame.

Before creating a new data frame with the filter, first we sort the result in a descending way, this will allow us to see if the maximum value of the close column its below 600.

```scala
ndf.filter($"Close" < 600).sort(desc("Close")).show()
scala> ndf.filter($"Close" < 600).sort(desc("Close")).show()
```
|               Date|             Open|             High|              Low|            Close|   Volume|        Adj Close|
|-------------------|-----------------|-----------------|-----------------|-----------------|---------|-----------------|
|2015-05-11 00:00:00|576.2700120000001|       593.999977|575.3000030000001|       589.950005| 23882600|        84.278572|
|2015-05-14 00:00:00|       582.999992|       587.470001|       576.310013|       586.850014|  8898400|        83.835716|
|2015-05-12 00:00:00|       586.659996|       586.659996|       580.950012|583.6400070000001| 11907000|        83.377144|
|2015-05-13 00:00:00|       583.830025|       589.380005|       578.840004|580.1099929999999| 11155900|        82.872856|
|2015-05-08 00:00:00|       567.289993|       575.069984|       566.750008|       574.600014| 13828500|        82.085716|
|2015-04-17 00:00:00|       558.450005|       575.000023|       558.000008|       571.550011| 58306500|        81.650002|
|2015-04-20 00:00:00|       572.499992|       576.129982|       562.670021|567.3900219999999| 30766400|        81.055717|
|2015-04-27 00:00:00|        562.04998|       572.499992|561.6100230000001|       566.079979| 15246700|80.86856800000001|
|2015-05-05 00:00:00|568.6699980000001|       577.099991|       565.299988|        565.54998| 27116600|        80.792854|
|2015-05-07 00:00:00|       560.800018|        565.56002|        556.20002|       565.240013| 10220700|        80.748573|
|2015-04-28 00:00:00|564.1299740000001|       568.950005|559.6099780000001|563.0599900000001|  8826300|        80.437141|
|2015-04-29 00:00:00|       560.489998|567.3900219999999|        557.51001|       562.849998|  9534700|        80.407143|
|2015-04-16 00:00:00|            532.0|           568.75|       530.000008|        562.04998|104500900|        80.292854|
|2015-05-06 00:00:00|       567.200005|       568.500008|       556.650017|       560.539986| 11504500|        80.077141|
|2015-04-21 00:00:00|       568.639984|       570.389984|       558.600021|560.4400099999999| 15925700|        80.062859|
|2015-04-23 00:00:00|       557.590012|       562.400002|        552.68998|559.0600049999999| 12687500|79.86571500000001|
|2015-04-24 00:00:00|       561.210014|       565.659996|       556.549988|       558.400017| 11045300|        79.771431|
|2015-04-22 00:00:00|        561.47998|       564.990021|        556.81002|           557.68| 12089700|        79.668571|
|2015-05-01 00:00:00|        558.98999|559.7699809999999|        552.26001|       557.029999|  8906100|        79.575714|
|2015-04-30 00:00:00|       561.660011|       565.850014|553.8699799999999|            556.5| 10565100|             79.5|

only showing top 20 rows

After that, we create a new data frame with the filter.

```Scala
val a11 =  ndf.filter($"Close" < 600)

a11.sort(desc("Close"))

//And we use the count method to show the total of rows where close is below 600.
a11.count
Long = 1218
```
**b. What percentage of the time was the “High” column greater than $ 500?**

To get the percentage of time that the column high was over 500. First, we create a new data frame named b11, where the values in the column high are above 500. Then we create a variable type double named b11percent, we have to strictly specify it as double because we are working with numbers that are in decimal. After that, we give the value of the total rows in the filtred data of b11 divided by the total rows in the csv, then we multiply it by 100. To get the total rows of the data frames we are using the count method and also the toDouble method, in that way the int that provides the count method will be converted to Double.

```scala
val b11 =  ndf.filter($"High" > 500)
b11.sort(asc("High")).show()
var b11percent: Double = (b11.count.toDouble/ndf.count.toDouble)*100

b11percent: Double = 4.924543288324067	
```
**c. What is the Pearson correlation between column "High" and column "Volume"?**

To calculate the Pearson Correlation of the column High over the column Volume, we are using the select method to show only the result and the corr method to calculate the correlation, specifying the columns High and Volume in it.

```scala
scala> ndf.select(corr($"High",$"Volume")).show()
```
|  corr(High, Volume)|
|--------------------|
|-0.20960233287942157|

**d. What is the maximum in the “High” column per year?**

First of all we use a groupBy to group the data in years, then using the "agg" function that allow us to access to the "max" method specifying that we are looking for the maximum value of the High column, after that we sort the data by years just to have an order in the result.

```scala
ndf.groupBy(year($"Date")).agg(max($"High")).sort(year($"Date")).show()
```
|year(Date)|         max(High)|
|--------------------|--------------------|
|      2011|120.28000300000001|
|      2012|        133.429996|
|      2013|        389.159988|
|      2014|        489.290024|
|      2015|        716.159996|
|      2016|129.28999299999998|

**e. What is the “Close” column average for each calendar month?**

Firstly, we used a groupBy to group the data in months, secondly using the "agg" function that allow us to access to the "max" method specifying that we are looking for the maximum value of the Close column, after that we sort the data frame by months just to have an order in the result.

```scala
ndf.groupBy(month($"Date")).agg(mean($"Close")).sort(month($"Date")).show()
```

|month(Date)|        avg(Close)|
|--------------------|--------------------|
|          1|212.22613874257422|
|          2| 254.1954634020619|
|          3| 249.5825228971963|
|          4|246.97514271428562|
|          5|264.37037614150944|
|          6| 295.1597153490566|
|          7|243.64747528037387|
|          8|195.25599892727263|
|          9|206.09598121568627|
|         10|205.93297300900903|
|         11| 194.3172275445545|
|         12| 199.3700942358491|
