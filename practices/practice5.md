## Practice 5
 
 
**1.- Para mostrar el Data Frame usamos esta función:**
```scala
df.show()
```
**2.- Podemos mostrar únicamente los nombres de las columnas del DataFrame:**
```scala
df.columns
```
**3.- También podemos mostrar sólo las columnas existentes e información sobre las mismas:**
```scala
df.printSchema()
```
**4.- Para mostrar un nuevo DataFrame con la selección de las variables nombre y edad:**
```scala
df.select("nombre","edad").show()
```
**5.-También os comentamos antes que se poseen las mismas ventajas que los RDD, como por ejemplo, podemos aplicar un filtro sobre este DataFrame. En este caso para filtrar los niños (children) que tengan menos de 13 años y que nos muestre esos elementos:**
```scala
val children = df.filter($."edad" < 13)
children.show
```
**6.-Se pueden hacer muchas más operaciones, como por ejemplo mostrar el número de elementos que contiene el DataFrame:**
```scala
df.count()
```
**7.- También mostrar los tres primeros elementos del DataFrame:**
```scala
df.head(3)
```
**8.- Eliminar datos nulos del data frame:**
```scala
df.na.drop()
```
**9.- Ordena la columna de salario y calcula la suma acumulada:**
```scala
orderBy()
```
**10.- Orden descendiente**
```scala
desc ("salario")
```
**11.- Muestra un ejemplo que calcula la covarianza sobre un conjunto de datos**
```scala
df.stat.cov("edad", "salario")
```
**12.- Muestra un ejemplo de identificar el valor mínimo sobre un conjunto de datos**
```scala
df.select(min("edad")).show()
```
**13.- Muestra un ejemplo de identificar el valor máximo sobre un conjunto de datos**
```scala
df.select(max("edad")).show()
```
**14.- Muestra un ejemplo que calcula la correlación sobre un conjunto de datos**
```scala
df.stat.corr("edad", "salario")
```
**15.- Muestra un ejemplo de cómo realizar la suma sobre un conjunto de datos**
```scala
df.select(sum("edad")).show()
```
**16.- Muestra un ejemplo de cómo realizar la media en un conjunto de datos**
```scala
df.select(avg("edad")).show()
```
