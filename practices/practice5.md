## Practice 5
 
 
**1.- To show the Data Frame we use this function:**
```scala
df.show()
```
**2.- We can show only the names of the columns of the DataFrame:**
```scala
df.columns
```
**3.- We can also show only the existing columns and information about them:**
```scala
df.printSchema()
```
**4.- To display a new DataFrame with the selection of the variables name and age:**
```scala
df.select("nombre","edad").show()
```
**5.- We also mentioned before that they have the same advantages as RDDs, such as, for example, we can apply a filter on this DataFrame. In this case, to filter the children who are under 13 years old and show us those elements:**
```scala
val children = df.filter($."edad" < 13)
children.show
```
**6.- You can do many more operations, such as displaying the number of elements that the DataFrame contains:**
```scala
df.count()
```
**7.- Also show the first three elements of the DataFrame:**
```scala
df.head(3)
```
**8.- Remove null data from data frame:**
```scala
df.na.drop()
```
**9.- Sort the salary column and calculate the accumulated sum:**
```scala
orderBy()
```
**10.- Descending order:**
```scala
desc ("salario")
```
**11.- Show an example that calculates the covariance over a data set:**
```scala
df.stat.cov("edad", "salario")
```
**12.- Show an example of identifying the minimum value over a data set:**
```scala
df.select(min("edad")).show()
```
**13.- Shows an example of identifying the maximum value over a data set:**
```scala
df.select(max("edad")).show()
```
**14.- Shows an example that calculates the correlation on a data set:**
```scala
df.stat.corr("edad", "salario")
```
**15.- Show an example of how to perform addition over a data set:**
```scala
df.select(sum("edad")).show()
```
**16.- Show an example of how to average a data set**
```scala
df.select(avg("edad")).show()
```
