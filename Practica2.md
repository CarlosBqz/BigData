## Practice 2

**1.- Crea una lista llamada "lista" con los elementos "rojo", "blanco", "negro"**


**2.- Añadir 5 elementos mas a "lista" "verde" ,"amarillo", "azul", "naranja", "perla"**


**3.- Traer los elementos de "lista" "verde", "amarillo", "azul"**


**4.- Crea un arreglo de numero en rango del 1-1000 en pasos de 5 en 5**


**5.- Cuales son los elementos unicos de la lista Lista(1,3,3,4,6,7,3,7) utilice conversion a conjuntos**


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

To add an element to the map we have to follow the syntax that we show bellow:

```scala
nombres += ("Miguel" -> 23)
res6: nombres.type = Map(Susana -> 27, Ana -> 23, Miguel -> 23, Luis -> 24, Jose -> 20)
```

To confirm that the element has been added to the map, we use the println method to print all the keys:

```scala
println(nombres)
Map(Susana -> 27, Ana -> 23, Miguel -> 23, Luis -> 24, Jose -> 20)
```
