Following the pseudocode of the following link:

https://es.wikipedia.org/wiki/Sucesión_de_Fibonacci

We will apply those codes to Scala:

**Fibonacci 1
**
```scala
def Fibonacci1(n:Int): Int =  {
		if(n < 2)
		{
			n
		}
		else
		{
			Fibonacci1(n-1)+Fibonacci1(n-2)
			
		}
}
```



**Fibonacci 2
**
```scala
def Fibonacci2(n:Double): Double = 
	var p: Double = ((1 + scala.math.sqrt(5)) / 2)
	var j: Double = ((scala.math.pow(p, n) - scala.math.pow((1 - p), n)) / scala.math.sqrt(5))
	if(n < 2)
	{
		n
	}
	else
	{
		j
	}
}
```


**Fibonacci 3
**
```scala
def Fibonacci3(n:Int) : Int = {
	var a = 0 
	var b = 1
	var c = b + a
	var k = 0

	while(k < n)
	{
		c = b + a
		a = b
		b = c

		k = k + 1
	}
	return a	
}
```



**Fibonacci 4
**
```scala
def Fibonacci4(n:Int) : Int = {
	var a = 0
	var b = 1
	var k = 0

	while(k < n)
	{
		b = b + a
		a = b - a

		k = k + 1 
	}
	return a
}
```



**Fibonacci 5 
**
```scala
def Fibonacci5(n:Int) : Int = {
	if(n < 2)
	{
		n
	}
	else
	{
		var z = new Array[Int](n+1)
		z(0) = 0
		z(1) = 1
		for(x <- 2 to (n))
		{
			z(x) = (z(x-1) + z(x-2))
		}
		z(n)
	}
}
```
