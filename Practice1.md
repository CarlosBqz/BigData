// Assessment 1/Practica 1


//1. Desarrollar un algoritmo en scala que calcule el radio de un circulo

//Declaring the variable for the diameter
var diameter = 6
//Making a function to calculate radium
def CalculateRad(diameter:Int) = diameter/2
//Calling the function to calculate
CalculateRad(diameter)
//Returns 3

//2. Desarrollar un algoritmo en scala que me diga si un numero es primo


//3. Dada la variable bird = "tweet", utiliza interpolacion de string para
//   imprimir "Estoy ecribiendo un tweet"

//Declaring the variable tweet
var bird = "tweet"

//Declaring the variable sentence
var sentence = "Estoy escribiendo un"

//Interpolating strings
println(f"$sentence%s $bird")
//Returns Estoy escribiendo un tweet


//4. Dada la variable mensaje = "Hola Luke yo soy tu padre!" utiliza slilce para extraer la
//   secuencia "Luke"


//5. Cual es la diferencia entre value y una variable en scala?

//Value (val) is a constant, it cannot be changed after its declaration
val pi = 3.1416
//Variable (var) can be changed after its declaration
var circle = 60

//Trying to change pi value 
pi = 5
//Throws the error: <console>:25: error: reassignment to val

//Tryng to change circle value
circle = 50
//Returns circle: Int = 50

//6. Dada la tupla (2,4,5,1,2,3,3.1416,23) regresa el numero 3.1416 
