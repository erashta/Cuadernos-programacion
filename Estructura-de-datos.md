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

 
Una implementación típica de lista enlazada tendría un código que define un nodo y se parece a esto:  

```
class ListNode { 
  String data; 
  ListNode nextNode; 
} 
ListNode firstNode; 
```

Luego podrías escribir un método para agregar nuevos nodos insertándolos al principio de la lista: 

```
ListNode newNode = new ListNode(); 
NewNode.nextNode = firstNode; 
firstNode = newNode; 
```
 
Se puede recorrer todos los elementos de la lista es una tarea sencilla: 

```
ListNode curNode = firstNode;
while (curNode != null) {
  ProcessData(curNode);
  curNode = curNode.nextNode;
}
```

Una estructura de datos relacionada, la lista doblemente enlazada, ayuda un poco a este problema. La diferencia con una lista enlazada típica es que la estructura de datos raíz 
almacena un puntero tanto al primer nodo como al último. 

Cada nodo individual tiene un enlace al nodo anterior y al siguiente en la lista. Esto crea una estructura más flexible que permite viajar en ambas direcciones. 
Aún así, sin embargo, esto es bastante limitado. 

### Colas  


Una cola es una estructura de datos que se describe mejor como  `primero en entrar, primero en salir`. 
 
Quizás el uso más común de una cola dentro de un problema es implementar un Breadth First Search  (BFS). BFS significa explorar primero todos los estados que se pueden alcanzar en un paso, luego todos los estados que se pueden alcanzar en dos pasos, etc. 

Una cola ayuda a implementar esta solución porque almacena una lista de todos los espacios de estado que se han visitado.  
