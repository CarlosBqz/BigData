**Instituto Tecnológico de Tijuana**
Departamento de Ciencias y Computación
Ingeniería en Sistemas Computacionales
 
 [![](https://upload.wikimedia.org/wikipedia/commons/2/2e/ITT.jpg)](https://upload.wikimedia.org/wikipedia/commons/2/2e/ITT.jpg)

**Proyecto / Tarea / Práctica:**
Coeficiente de Correlación de Pearson

**Materia:**
BDD-1704 SC9A Datos Masivos

**Unidad:**
Unidad I

**Facilitador:**
JOSE CHRISTIAN ROMERO HERNANDEZ

**Alumno:**
Bojórquez Vargas Carlos Francisco
16211977

**Grupo:**
SC9A

**Fecha:**
Tijuana, Baja California a 19 de Octubre de 2020. 


###¿Qué es el coeficiente de correlación de Pearson?

El **coeficiente de correlación de Pearson** es la covarianza estandarizada, y su ecuación difiere dependiendo de si se aplica a una muestra, Coeficiente de Pearson muestral (r), o si se aplica la población Coeficiente de Pearson poblacional (ρ).
 
 [![](https://www.webyempresas.com/wp-content/uploads/2018/05/formula.jpg)](https://www.webyempresas.com/wp-content/uploads/2018/05/formula.jpg)

**Condiciones**

●	La relación que se quiere estudiar entre ambas variables es lineal (de lo contrario, el coeficiente de Pearson no la puede detectar).
●	Las dos variables deben de ser cuantitativas.
●	Normalidad: ambas variables se tienen que distribuir de forma normal. Varios textos defienden su robustez cuando las variables se alejan moderadamente de la normal.
●	Homocedasticidad: La varianza de Y debe ser constante a lo largo de la variable X. Esto se puede identificar si en el scatterplot los puntos mantienen la misma dispersión en las distintas zonas de la variable X. Esta condición no la he encontrado mencionada en todos los libros.

**Características**

●	Toma valores entre [-1, +1], siendo +1 una correlación lineal positiva perfecta y -1 una correlación lineal negativa perfecta.
●	Es una medida independiente de las escalas en las que se midan las variables.
●	No varía si se aplican transformaciones a las variables.
●	No tiene en consideración que las variables sean dependientes o independientes.
●	El coeficiente de correlación de Pearson no equivale a la pendiente de la recta de regresión.
●	Es sensible a outliers, por lo que se recomienda en caso de poder justificarlos, excluirlos del análisis.

**Interpretación**

Además del valor obtenido para el coeficiente, es necesario calcular su significancia. Solo si el p-value es significativo se puede aceptar que existe correlación y esta será de la magnitud que indique el coeficiente. Por muy cercano que sea el valor del coeficiente de correlación a +1 o -1, si no es significativo, se ha de interpretar que la correlación de ambas variables es 0 ya que el valor observado se puede deber al azar. (Ver más adelante cómo calcular la significancia).

**Conclusión**

Lo que entendí sobre el coeficiente de correlación de Pearson es que este nos ayuda a identificar la relación entre dos variables cuantitativas, es decir, dos variables que están relacionadas linealmente. Un ejemplo del uso de este coeficiente es cuando se busca identificar la relación entre peso y altura o, en un caso conocido, la relación entre número de tiros y porcentaje de encesto de un jugador de basketball.

**Referencias**

[1] https://psicologiaymente.com/miscelanea/coeficiente-correlacion-pearson#:~:text=El%20coeficiente%20de%20correlación%20de%20Pearson%20se%20utiliza%20para%20estudiar,la%20dirección%20de%20la%20relación.. (2020). Coeficiente de correlación de Pearson: qué es y cómo se usa. 19 de octubre de 2020, de Psicología y Mente Sitio web: https://psicologiaymente.com/miscelanea/coeficiente-correlacion-pearson#:~:text=El%20coeficiente%20de%20correlación%20de%20Pearson%20se%20utiliza%20para%20estudiar,la%20dirección%20de%20la%20relación.

[2] Joaquín Amat Rodrigo. (2016). Correlación lineal y Regresión lineal simple. 19 de octubre de 2020, de Ciencia de Datos Sitio web: https://www.cienciadedatos.net/documentos/24_correlacion_y_regresion_lineal



