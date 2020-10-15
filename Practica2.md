## Practice 2

**1.- Crea una lista llamada "lista" con los elementos "rojo", "blanco", "negro"**
Creating a list
```scala
var a = List("rojo", "blanco", "negro")
```

**2.- Añadir 5 elementos mas a "lista" "verde" ,"amarillo", "azul", "naranja", "perla"**
Adding elements to list

```scala
a :+="verde"
a :+="amarillo"
a :+="azul"
a :+="naranja"
a :+="perla" 
```
**3.- Traer los elementos de "lista" "verde", "amarillo", "azul"**
Getting elements from list

```scala
a(3)
a(4)
a(5)
```

**4.- Crea un arreglo de numero en rango del 1-1000 en pasos de 5 en 5**
Creating array and adding the number sequence and changing first number to number 1

```scala
val x = Array.range(0, 1005, 5)
x(0) = 1
x ```

**5.- Cuales son los elementos unicos de la lista Lista(1,3,3,4,6,7,3,7) utilice conversion a conjuntos**

First we declare the list with the elements provided by the exercise:

```scala
scala> val Lista = List(1,3,3,4,6,7,3,7)
```

Then we apply the "distinct" method to the list, this will throw all the uniques values in order:

```scala
scala> Lista.distinct
```

The method will print like the code below:

```scala
res0: List[Int] = List(1, 3, 4, 6, 7)
```


**6.- Crea una mapa mutable llamado nombres que contenga los siguiente "Jose", 20, "Luis", 24, "Ana", 23, "Susana", "27"**

First of all, we have to import the library that allows us to create a mutable map:

```scala
scala> import scala.collection.mutable.Map
import scala.collection.mutable.Map
```

Secondly, we create the map using the following syntax:

```scala
scala> val nombres: Map[String, Int] = Map(("Jose", 20),("Luis", 24),("Ana", 23),("Susana", 27))
```


**6 a .- Imprime todas la llaves del mapa**

To print the map, we are using the println method:

```scala
println(nombres)
Map(Susana -> 27, Ana -> 23, Luis -> 24, Jose -> 20)
```


**7 a .- Agrega el siguiente valor al mapa("Miguel", 23)**

To add an element to the map we have to follow the syntax that we show below:

```scala
nombres += ("Miguel" -> 23)
res6: nombres.type = Map(Susana -> 27, Ana -> 23, Miguel -> 23, Luis -> 24, Jose -> 20)
```

To confirm that the element has been added to the map, we use the println method to print all the keys:

```scala
println(nombres)
Map(Susana -> 27, Ana -> 23, Miguel -> 23, Luis -> 24, Jose -> 20)
```
