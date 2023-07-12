## Estructuras de datos

### Estructuras de datos simples 

Las estructuras de datos más simples son las variables primitivas. Tienen un valor único  y  más allá de eso, tienen un uso limitado. 
Cuando es necesario almacenar muchos valores relacionados, se utiliza un arreglo o una  matriz. 

Un concepto algo más difícil, aunque igualmente primitivo, son los **punteros**. 

Los punteros, en lugar de contener un valor real, simplemente contienen una dirección de memoria que, en teoría, contiene  algún dato útil. Debería ser suficiente saber que los punteros `apuntan` a algún lugar de la memoria y, en realidad, no almacenan datos por sí mismos.  


### Arreglos 

Los arreglos son una estructura de datos muy simple y pueden considerarse como una lista de una longitud fija. 
Los arreglos son adecuados para situaciones en las que se conoce la cantidad de elementos de datos (o se puede determinar mediante programación). 
Una gran fortaleza de los arreglos es que se puede acceder a ellos aleatoriamente, por índice. 

Por ejemplo, si tienes un arreglo que contiene una lista de nombres de estudiantes sentados en un salón de clases, donde cada asiento está numerado del `1` al `n`, entonces 
`NombreEstudiante[i]` es una forma trivial de leer o almacenar el nombre del estudiante en el asiento `i`.  

### Listas enlazadas 

Una lista enlazada es una estructura de datos que puede contener una cantidad arbitraria de elementos de datos y puede cambiar fácilmente el tamaño para agregar o eliminar elementos. 

Una lista enlazada, en su forma más simple, es un puntero a un nodo de datos. Luego, cada nodo de datos se compone de datos (posiblemente un registro con varios valores de datos) y un puntero 
al siguiente nodo. Al final de la lista, el puntero se establece en nulo.  

Por la naturaleza de su diseño, una lista enlazada es ideal para almacenar datos cuando la cantidad de elementos es desconocida o está sujeta a cambios. 

Sin embargo, no proporciona ninguna forma de acceder a un elemento arbitrario de la lista, a menos que comience desde el principio y recorra todos los nodos hasta llegar al que desea. 

Lo mismo es cierto si desea insertar un nuevo nodo en una ubicación específica. No es difícil ver el problema de la ineficiencia.  

Una estructura de datos relacionada, la **lista doblemente enlazada**, ayuda un poco a este problema. La diferencia con una lista enlazada típica es que la estructura de datos raíz 
almacena un puntero tanto al primer nodo como al último. 

Cada nodo individual tiene un enlace al nodo anterior y al siguiente en la lista. Esto crea una estructura más flexible que permite viajar en ambas direcciones. 
Aún así, sin embargo, esto es bastante limitado. 

### Colas  

Una cola es una estructura de datos que se describe mejor como  `primero en entrar, primero en salir`. 
 
Quizás el uso más común de una cola dentro de un problema es implementar un Breadth First Search  (BFS). BFS significa explorar primero todos los estados que se pueden alcanzar en un paso, luego todos los estados que se pueden alcanzar en dos pasos, etc. 

Una cola ayuda a implementar esta solución porque almacena una lista de todos los espacios de estado que se han visitado.  

### Pilas

Las pilas son, en cierto sentido, lo opuesto a las colas, ya que se describen como  `último en entrar, primero en salir`. 

Si bien puede parecer que las pilas rara vez se implementan explícitamente, una sólida comprensión de cómo funcionan y cómo se usan implícitamente es una educación que vale la pena. 

Aquellos que han estado programando por un tiempo están íntimamente familiarizados con la forma en que se usa la pila cada vez que se llama a una subrutina desde dentro de un programa. 

Todos los parámetros y por lo general, las variables locales se asignan sin espacio en la pila. Luego, una vez finalizada la subrutina, las variables locales se eliminan y la dirección de retorno se `extrae` de la pila, de modo que la ejecución del programa puede continuar donde se quedó antes de llamar a la subrutina. 

La comprensión de lo que esto implica se vuelve más importante a medida que las funciones llaman a otras funciones, que a su vez llaman a otras funciones. Cada llamada de función aumenta el `nivel de anidamiento` (la profundidad de las llamadas de función, por así decirlo) de la ejecución y utiliza cada vez más espacio en la pila. 

De suma importancia es el caso de una función recursiva. Cuando una función recursiva se llama continuamente a sí misma, el espacio de la pila se usa rápidamente a medida que aumenta la profundidad de la recursividad. 




