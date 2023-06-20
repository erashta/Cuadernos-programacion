Archivos
~~~~~~~~

En C++, manejamos y organizamos archivos, carpetas y directorios.
Podemos escribir archivos y mostrar los elementos en los archivos
creados. ¿Qué es exactamente un archivo en C++?.

Los archivos creados en C++ ayudan a almacenar datos en el disco duro y
permanecen en el sistema para su uso posterior. Esto es diferente de los
programas tradicionales, donde los datos u objetos que contienen datos
se eliminan después de la finalización del programa para recuperar el
espacio de memoria temporal asignado. Muchas veces, necesitamos qu los
datos se almacenen de forma segura en el sistema, por lo que obtener
elementos de un archivo requiere menos tiempo.

Flujos
^^^^^^

Para que el usuario ingrese y proporcione detalles o valores, usamos
flujos de entrada (input streams). Un flujo (stream) se refiere a un
grupo de bytes a los que se puede acceder secuencialmente. Hay dos tipos
diferentes de stream.

Los **flujos de entrada** se utilizan para contener la entrada de un
usuario dada desde un teclado. Por ejemplo, el usuario puede presionar
cualquier tecla del teclado, incluso cuando no se le solicite. En una
situación como esta, todas estas claves se guardan en el flujo de
entrada y se usan más tarde cuando el propio programa lo requiere.

El segundo tipo es un **flujo de salida** (output stream), que se
utiliza para la salida a un monitor, archivo o impresora. Por ejemplo,
supongamos que necesitas una impresión, pero la impresora está
trabajando actualmente en otro documento, por lo que tus datos/archivo
esperan su turno para ser entregados como salida. Para poder usar todas
estas funcionalidades en tu código C++, debes incluir el archivo de
encabezado ``iostream`` que proporciona al programa una jerarquía
completa de clases (herencia múltiple) para hacer uso de todas las
clases de E/S.

C++ proporciona las siguientes clases para realizar la salida y entrada
de caracteres a/desde archivos:

-  La clase ``iostream`` puede manejar flujos de entrada y salida. Es la
   clase base para todas las demás clases de flujo de E/S que usaremos.

-  La clase ``istream`` es para los flujos de entrada y esta clase se
   deriva de la clase ``iostream``. El operador de extracción ``(>>)``
   elimina valores del flujo creado cuando el usuario presiona las
   teclas (ya sea que se le solicite o no). También tiene funciones para
   operaciones de entrada como ``get()`` y ``getline()``.

-  La clase ``ostream`` es para flujos de salida y esta clase se deriva
   de la clase ``iostream``. El operador de inserción ``(<<)`` se usa
   para poner valores en el flujo para que se muestren en dispositivos
   de salida como monitores. También tiene funciones para operaciones de
   entrada como ``put()`` y ``write()``.

-  La clase ``fstream`` puede manejar operaciones de entrada y salida
   relacionadas con la apertura y el cierre de archivos, como leer
   elementos particulares de un archivo o escribir en un archivo. Tiene
   varias funciones con una operatividad especial.

-  La clase ``ifstream`` maneja todas las operaciones relacionadas con
   la entrada de archivos y tiene varias funciones con operabilidad
   especial, como ``get()``, ``getline()``, ``read()``, ``seekg()`` y
   ``tellg()``.

-  La clase ``ofstream`` puede manejar operaciones de entrada
   relacionadas con la escritura en un archivo. Tiene varias funciones
   con operabilidad especial como ``put()``, ``write()``, ``seekp()`` y
   ``tellp()``.

-  La clase ``streambuf`` se utiliza para administrar los flujos de
   entrada y salida a través de un puntero, que apunta al búfer.

Ya hemos usado objetos cuyos tipos eran estas clases: ``cin`` es un
objeto de clase ``istream`` y cout es un objeto de clase ``ostream``.
Por lo tanto, ya hemos estado usando clases relacionadas con los flujos
de archivos. Y de hecho, podemos usar los flujos de archivos de la misma
manera que ya estamos acostumbrados a usar ``cin`` y ``cout``, con la
única diferencia de que tenemos que asociar estos flujos con archivos
físicos.

Ejemplo
^^^^^^^

.. code:: c++

    #include <iostream>
    #include <fstream>
    using namespace std;
    int main()
    {
        ofstream ofObj;
        string linea1;
        ofObj.open("archivo1.txt");
        
        while (ofObj)
        {
            getline(cin, linea1);
            if (linea1 == "-1")
                break;
            cout<< linea << endl;
        }
        ofObj.close();
        ifstream ifObj;
        ifObj.open("archivo1.txt");
        while (ifObj)
        {
            getline(ifObj, linea1);
            cout << linea1 << endl;
        }
        ifObj.close();
            return 0;
    }

En este código, usamos la clase ``ofstream`` y creamos un objeto y un
valor de cadena. Luego usamos el método ``open`` para acceder al archivo
en modo abierto. El texto ingresado se escribe en el archivo usando el
objeto de clase ``ifstream``. Luego, el programa imprime el contenido
del archivo recién escrito. Se recurre al método ``close`` para cerrar
el archivo que habíamos abierto anteriormente.

Operaciones de archivos
~~~~~~~~~~~~~~~~~~~~~~~

Los principales pasos necesarios en C++ para el manejo de archivos son
los siguientes:

Creación de un nuevo archivo
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: c++

    #include <iostream>
    #include <fstream>
    
    using namespace std;
    
    int main()
    {
        // completa 
    
        if(fs.is_open()==0)
        {
            cout<<"No se puede abrir el archivo";
        }
    
        fs.close();
        return 0;
    }

En el programa anterior, creamos el objeto ``fstream fs`` y usamos el
método ``open()`` para abrir un archivo usando el modo ``ios::out``.
Después de eso, usamos el método ``is_open()`` para verificar si el
archivo se abrió con éxito o no. Este método devuelve 1 si se abre el
archivo, de lo contrario, devuelve 0 cuando no puede abrir el archivo.

El método ``close()`` se usa para cerrar el archivo que está abierto en
la memoria.

.. code:: c++

    // Otra forma
    
    #include <iostream>
    #include <fstream> // 
    using namespace std;
    
    int main()
    {
        fstream archivo1;
        archivo.open("archivo1",ios::out);
        if(!archivo1)
        {
            cout<<"El archivo no fue creado";
        }
        else
        {
            cout<<"El archivo ha sido creado";
            archivo1.close();
        }
        return 0;
    }

En este código, usamos la clase ``fstream`` y creamos un objeto de esa
clase. Usamos el método ``open`` para acceder al archivo en modo
abierto. Luego, el programa imprime el mensaje
``El archivo ha sido creado``.

Se recurre al método ``close`` para cerrar el archivo que abrimos
anteriormente.

Abrir un archivo
^^^^^^^^^^^^^^^^

Este es el primer paso que se da hacia la administración de archivos en
C++ y se puede hacer pasando el nombre de archivo en el constructor
cuando se crea un objeto o usando el método ``open()``. La sintaxis en
general para abrir un archivo es la siguiente:

.. code:: c++

    void open(const char* ourFileName, ios::openMode mode);  // funcion open()

Ahora, se pueden activar varios
`modos <https://gist.github.com/kapumota/404263d6d6453960c54420d0bced86cd>`__
en C++.

También podemos combinar diferentes modos de apertura separando cada uno
de ellos mediante el símbolo \|, llamado símbolo lógico ``or``.

Por ejemplo, si queremos abrir el archivo ``ejemplo.bin`` en modo
binario para agregar datos, podemos hacerlo mediante la siguiente
llamada a la función miembro ``open``:

.. code:: c++

    ofstream archivo1;
    archivo1.open ("ejemplo.bin", ios::out | ios::app | ios::binary);

Cada una de las funciones miembro ``open`` de las clases ``ofstream``,
``ifstream`` y ``fstream`` tiene un modo predeterminado que se usa si el
archivo se abre sin un segundo argumento:

-  ``ofstream ios::out``
-  ``ifstream ios::in``
-  ``fstream ios::in | ios::out``

Para las clases ``ifstream`` y ``ofstream``, ``ios::in`` y ``ios::out``
se asumen automática y respectivamente, incluso si se pasa un modo que
no los incluye como segundo argumento a la función miembro ``open``.

Para ``fstream``, el valor predeterminado solo se aplica si se llama a
la función sin especificar ningún valor para el parámetro de modo. Si se
llama a la función con cualquier valor en ese parámetro, el modo
predeterminado se anula, no se combina.

Los flujos de archivos abiertos en modo binario realizan operaciones de
entrada y salida independientemente de cualquier consideración de
formato. Los archivos no binarios se conocen como archivos de texto y
pueden producirse algunas traducciones debido al formato de algunos
caracteres especiales (como los caracteres de nueva línea y de retorno
de carro).

Dado que la primera tarea que se realiza en un flujo de archivos
generalmente es abrir un archivo, estas tres clases incluyen un
constructor que llama automáticamente a la función de miembro ``open`` y
tiene exactamente los mismos parámetros que este miembro. Por lo tanto,
también podríamos haber declarado el objeto ``archivo1`` anterior y
realizar la misma operación de apertura el el ejemplo anterior
escribiendo:

.. code:: c++

    ofstream archivo1("ejemplo.bin", ios::out | ios::app | ios::binary);


Para verificar si se abrió correctamente un archivo, puedes utilizar el
miembro ``is_open``. Esta función miembro devuelve un valor ``bool`` de
``true`` en el caso de que el objeto de flujo esté asociado con un
archivo abierto, o ``false`` en caso contrario:

.. code:: c++

    if (archivo1.is_open()) // ok, proceda con la salida 

Ejemplo: abrir un archivo
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: c++

    #include <iostream>
    #include <fstream>
    
    using namespace std;
    
    int main()
    {
        // completa
    
        if(fs.is_open()==0)
        {
            cout<<"No se puede abrir el archivo";
        }
    
        fs.close();
        return 0;
    }

Ejemplo
^^^^^^^

.. code:: c++

    #include <iostream>
    #include <fstream> // trabajando con archivos de texto
    using namespace std;
    
    int main()
    
    {
    
        ofstream ofObj;
        string linea1;
        ofObj.open("archivos1.txt");
        while (ofObj)
        {
            getline(cin, linea1);
            if (linea1 == "-1")
                break;
            cout<< linea1 << endl;
        }
        ofObj.close();
        ifstream ifObj;
        ifObj.open("archivos1.txt");
        while (ifObj)
        {
            getline(ifObj, linea1);
                cout << linea1 << endl;
        }
        ifObj.close();
            return 0;
    }

En este código, usamos la clase ``ofstream`` y creamos un objeto de esa
clase y una cadena. Luego usamos el método ``open`` para acceder
alarchivo en modo abierto. El texto ingresado se escribe en el archivo
usando el objeto de clase ``ifstream``. Luego, el programa imprime el
contenido del archivo recién escrito.

Se recurre al método ``close`` para cerrar el archivo que habíamos
abierto anteriormente.

Escribir en un archivo
^^^^^^^^^^^^^^^^^^^^^^

Esto se hace usando las clases ``ofstream`` o ``fstream`` para ingresar
datos en los archivos creados o abiertos.

.. code:: c++

    #include<iostream>                         
    #include<fstream>                            
    using namespace std;
    
    int main() {
        fstream archivo1;                       
        // completa ...
        return 0;
    }

Aquí tenemos creamos un objeto de la clase ``fstream`` llamado
``archivo1``. En el objeto creado anteriormente, tenemos que aplicar la
función ``open()`` para crear un nuevo archivo, y el modo se establece
en ``out``, lo que nos permitirá escribir en el archivo.

Usamos la declaración ``if`` para verificar la creación del archivo.
Escribimos los datos en el archivo creado. Usamos la función ``close()``
en el objeto para cerrar el archivo.

Ejercicio
^^^^^^^^^

Escribe el programa a partir de las siguiente indicaciones.

1.  Incluye el archivo de encabezado ``iostream`` en el programa para
    usar sus funciones.
2.  Incluye el archivo de encabezado ``fstream`` en el programa para
    usar sus clases.
3.  Incluye el espacio de nombres estándar en el programa para usar sus
    clases sin llamarlo.
4.  Llama a la función ``main()``. La lógica del programa debe agregarse
    dentro del cuerpo de esta función.
5.  Crea una instancia de la clase ``fstream`` y asígnala el nombre
    ``archivo1``.
6.  Usa la función ``open()`` para crear un nuevo archivo llamado
    ``archivo1.txt``. El archivo se abrirá en el modo ``out`` para
    escribir en él.
7.  Usa una declaración ``if`` para verificar si el archivo no se ha
    abierto.
8.  Escribe el texto para imprimir en la consola si el archivo no está
    abierto.
9.  Termina del cuerpo de la instrucción ``if``.
10. Usa una declaración ``else`` para indicar qué hacer si se creó el
    archivo.
11. Escribe texto para imprimir en la consola si se creó el archivo.
12. Escribe un texto en el archivo creado.
13. Utiliza la función ``close()`` para cerrar el archivo.
14. Finaliza el cuerpo de la sentencia ``else``.
15. El programa debe retornar el valor al completarse con éxito.
16. Finaliza del cuerpo de la función ``main()``.

.. code:: c++

    // Completa

Lectura de un archivo
^^^^^^^^^^^^^^^^^^^^^

Se usan las clases ``ifstream`` o ``fstream`` para obtener datos de los
archivos creados o abiertos.

.. code:: c++

    #include <iostream>
    #include <fstream>
    
    using namespace std;
    
    int main()
    {
        fstream fs;
        char c;
    
        fs.open("data.txt", ios::in);
    
        if(fs.is_open()==0)
        {
            cout<<"No se puede abrir el archivo";
        }
        else
        {
            while(!fs.eof())
            {
                fs.get(c);
                cout<<c;
            }
            fs.close();
        }
        return 0;
    }

En el programa anterior, hemos ejecutado un ciclo ``while`` infinito
para leer todos los caracteres del archivo hasta que obtengamos el final
del carácter del archivo usando el método ``eof()``. Leemos caracteres
individuales del archivo en una variable de carácter ``c`` usando el
método ``get()`` y lo imprimimos en la pantalla. El ciclo ``while``
termina cuando obtenemos el carácter ``eof`` (CTRL + D) del archivo.

Ejercicio
^^^^^^^^^

Utiliza un ciclo ``while`` que usa la función ``getline()`` para leer
una línea del archivo en una variable de cadena. Imprimimos la línea
junto con el carácter de nueva línea usando la instrucción ``endl`` en
la pantalla.

El ciclo ``while`` termina cuando la función ``getline()`` llega al
final del archivo.

.. code:: c++

    // Completa

Ejercicio
^^^^^^^^^

Escribe el programa a partir de las siguiente indicaciones.

1.  Incluye el archivo de encabezado ``iostream`` en el programa para
    usar sus funciones.
2.  Incluye el archivo de encabezado ``fstream`` en el programa para
    usar sus clases.
3.  Incluye el espacio de nombres estándar en el programa para usar sus
    clases sin llamarlo.
4.  Llama a la función ``main()``. La lógica del programa debe agregarse
    dentro del cuerpo de esta función.
5.  Crea una instancia de la clase ``fstream`` y asígnala el nombre
    ``archivo1``.
6.  Usa la función ``open()`` para crear un nuevo archivo llamado
    ``archivo1.txt``. El archivo se abrirá en el modo ``in`` para
    escribir en él.
7.  Usa una declaración ``if`` para verificar si el archivo no se ha
    abierto.
8.  Escribe el texto para imprimir en la consola si el archivo no está
    abierto.
9.  Termina del cuerpo de la instrucción ``if``.
10. Usa una declaración ``else`` para indicar qué hacer si se creó el
    archivo.
11. Crea una variable char llamada ``ch``.
12. Crea un bucle ``while`` para iterar sobre el contenido del archivo.
13. Escribe/almacena el contenido del archivo en la variable ``ch``.
14. Usa una condición ``if`` y la función ``eof()``, es decir, el final
    del archivo, para asegurarse de que el compilador siga leyendo el
    archivo si no se llega al final.
15. Usa una declaración ``break`` para dejar de leer el archivo una vez
    que se llega al final.
16. Imprime el contenido de la variable ``ch`` en la consola.
17. Finaliza el cuerpo ``while``.
18. Finaliza el cuerpo de la sentencia ``else``.
19. Llama a la función ``close()`` para cerrar el archivo.
20. El programa debe retornar valor al completarse con éxito.
21. Finaliza el cuerpo de la función ``main()``.

.. code:: c++

    // Completa

Cambiar el nombre de un archivo
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: c++

    #include <iostream>
    #include <stdio.h>
    
    using namespace std;
    
    int main()
    {
        rename("data1.txt", "data2.txt");
        printf("Archivo renombrado!");
        return 0;
    }

Nota: Antes de cambiar el nombre del archivo, asegúrate de que el
archivo esté cerrado,de lo contrario, no se podrá cambiar el nombre.

Eliminar un archivo
^^^^^^^^^^^^^^^^^^^

.. code:: c++

    #include <iostream>
    #include <stdio.h>
    
    using namespace std;
    
    int main()
    {
        remove("data2.txt");
        printf("Archivo eliminado!");
        return 0;
    }

Nota: Antes de eliminar el archivo, asegúrate de que el archivo esté
cerrado, de lo contrario, no se podrá eliminar.

Cerrar un archivo
^^^^^^^^^^^^^^^^^

Cuando hayamos terminado con las operaciones de entrada y salida en un
archivo, lo cerraremos para que el sistema operativo sea notificado y
sus recursos vuelvan a estar disponibles. Para eso, llamamos a la
función miembro ``close``. Esta función miembro vacía los búferes
asociados y cierra el archivo.

Este es el último paso importante en el manejo de archivos.

Comprobación de los indicadores de estados
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Las siguientes funciones miembro existen para verificar estados
específicos de un stream (todas ellas devuelven un valor booleano):

``bad()``: devuelve ``true`` si falla una operación de lectura o
escritura. Por ejemplo, en el caso de que intentemos escribir en un
archivo que no está abierto para escribir o si el dispositivo donde
intentamos escribir no tiene espacio libre.

``fail()``: devuelve ``true`` en los mismos casos que ``bad()``, pero
también en el caso de que ocurra un error de formato, como cuando se
extrae un carácter alfabético, cuando intentamos leer un número entero.

``eof()``: devuelve ``true`` si un archivo abierto para lectura ha
llegado al final.

``good()``: es el indicador de estado más genérica: devuelve ``false``
en los mismos casos en que llamar a cualquiera de las funciones
anteriores devolvería ``true``.

Ejercicios
~~~~~~~~~~

1. Escribe un programa en C++ para contar dígitos, letras y espacios
   usando el manejo de archivos.

2. Escribe un programa en C++ para contar palabras, líneas y el tamaño
   total usando el manejo de archivos.

.. code:: c++

    // Respuestas

Acceso aleatorio a un archivo
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Todos los objetos de flujos de E/S mantienen internamente al menos una
posición interna:

``ifstream``, como ``istream``, mantiene una posición ``get`` interna
con la ubicación del elemento que se leerá en la siguiente operación de
entrada.

``ofstream``, como ``ostream`` mantiene una posición ``put`` interna con
la ubicación donde debe escribirse el siguiente elemento.

Finalmente, ``fstream`` mantiene tanto la posición ``get`` como ``put``
como ``iostream``.

Estas posiciones de flujo interno apuntan a las ubicaciones dentro del
flujo donde se realiza la siguiente operación de lectura o escritura.
Estas posiciones se pueden observar y modificar utilizando las
siguientes funciones miembro:

**tellg() y tellp()**

Estas dos funciones sin parámetros devuelven un valor del tipo
``streampos``, que es un tipo que representa la posición ``get`` (en el
caso de ``tellg``) o la posición ``put`` (en el caso de ``tellp``).

**seekg() y seekp()**

Estas funciones permiten cambiar la ubicación de las posiciones ``get``
y ``put``. Ambas funciones están sobrecargadas con dos prototipos
diferentes. La primera forma es:

.. code:: c++

    seekg ( position );
    seekp ( position );

Usando este prototipo, el puntero de flujo se cambia a la posición
absoluta ``position`` (contando desde el comienzo del archivo). El tipo
de este parámetro es ``streampos``, que es el mismo tipo que devuelven
las funciones ``tellg`` y ``tellp``.

La otra forma para estas funciones es:

.. code:: c++

    seekg ( offset, direction );
    seekp ( offset, direction );

Con este prototipo, la posición ``get`` o ``put`` se establece en un
valor ``offset`` relativo a algún punto específico determinado por el
parámetro ``direction``. ``offset`` es de tipo ``streamoff``. Y
``direction`` es de tipo ``seekdir``, que es un tipo enumerado que
determina el punto desde donde se cuenta el offser y que puede tomar
cualquiera de los siguientes valores:

-  ``ios::beg`` offset contado desde el comienzo del flujo
-  ``ios::cur`` offset contado desde la posición actual
-  ``ios::end`` offset contado desde el final del flujo.

Ejemplo
^^^^^^^

El siguiente ejemplo utiliza las funciones miembro que acabamos de ver
para obtener el tamaño de un archivo:

.. code:: c++

    // Completa

Observa el tipo que hemos usado para las variables ``begin`` y ``end``:

::

   streampos tam;

``streampos`` es un tipo específico utilizado para el posicionamiento de
archivos y búfer y es el tipo devuelto por ``archivo.tellg()``. Los
valores de este tipo se pueden restar de forma segura de otros valores
del mismo tipo y también se pueden convertir a un tipo entero lo
suficientemente grande como para contener el tamaño del archivo.

Estas funciones de posicionamiento de flujo utilizan dos tipos
particulares: ``streampos`` y ``streamoff``. Estos tipos también se
definen como tipos de miembros de la clase de flujo:

-  ``streampos ios::pos_type`` Definido como fpos. Se puede convertir
   a/desde ``streamoff`` y se pueden sumar o restar valores de estos
   tipos

-  ``streamoff ios::off_type`` Es un alias de uno de los tipos
   integrales fundamentales (como int o long long).

Ejemplo general
^^^^^^^^^^^^^^^

.. code:: c++

    #include <iostream>
    #include <fstream>
    
    using namespace std;
    
    int main()
    {
        fstream fs;
        char s[]="C++ y Python";
        char c;
        int i;
    
        fs.open("data.txt", ios::in | ios::out);
    
    
        if(fs.is_open()==0)
        {
            cout<<"No se puede abrir el archivo";
        }
        else
        {
            // Completa
        }
    
        fs.close();
        return 0;
    }

¿Qué es un archivo binario?
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Comprendamos cómo se almacenan los datos en un archivo binario y cómo se
procesan en las computadoras.

Los archivos binarios almacenan datos en forma de una secuencia de
bytes. Estas secuencias son un flujo de grupos de ocho o dieciséis bits.
Estos archivos se utilizan principalmente para almacenar datos
personalizados para aplicaciones y, a veces, archivos que guardan varios
tipos de datos, como imágenes, audio, texto, etc.

Los desarrolladores que codifican estos formatos de archivo
personalizados diseñan las aplicaciones de soporte para convertir la
información binaria en alguna forma significativa. Por ejemplo, un
archivo binario tiene los datos de 5 audios en formato de texto. Si
abres el archivo en un editor de texto, verás secuencias de datos
binarios, lo cual no es comprensible. Sin embargo, si el desarrollador
diseña una aplicación de reproducción de audio que comprende y convierte
estos datos binarios en audio y los reproduce, puede escucharlos.

Los archivos binarios suelen contener encabezados como ``.jpg`` y
``.png`` para indicar el tipo de información que han almacenado. Los
datos del archivo binario se cifran con 1 y 0, lo que lo hace más seguro
ya que la información no es legible. Ocupan mucho menos espacio a medida
que se almacenan en la memoria según su tamaño de bits (igual que el
almacenamiento de la memoria).

Las desventajas de los archivos binarios son que un simple error en los
datos corrompe todo el archivo y es difícil corregir tales errores. Sin
embargo hay formas de prevenir la corrupción de datos en el futuro. El
archivo debe sufrir muchas variaciones internas y formas de
representación para transferir archivos binarios de una computadora a
otra. Los usuarios habituales siempre deben tener un sistema de soporte
convertible para ver los datos en archivos binarios.

Ejemplo: salida de un archivo de texto
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: c++

    #include <fstream>
    #include <iostream>
    #include <cstring>
    using namespace std;
    
    struct Persona
    {
       char nombre[20];
       int edad;
    };
    
    int main()
    {
       Persona checha;
       strncpy(checha.nombre, "Checha", 6);
       checha.edad=52;
    
       //abre un archivo texto.out para el salida del texto
       ofstream outFile("texto.out");
    
       if (! outFile)
       {
          cerr << "no puedes abrir \"texto.out\" para la salida\n";
          return -1;
       }
    
       outFile << checha.nombre;
       outFile << checha.edad;
    
       outFile.close();
    
       return 0;
    }

Ejercicio: escribe la salida de un archivo binario
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: c++

    // Completa

Información:

-  El dump hexadecimal se produce con el siguiente comando:
   ``hexdump -C nombre de archivo``

-  El dump octal se produce con el siguiente comando:
   ``od -c nombre de archivo``

**Preguntas:**

-  ¿Qué cosas notas que son diferentes en el código binario?

-  ¿Por qué crees que el archivo binario tiene más caracteres (y
   extraños) en comparación con el archivo de texto?

-  ¿Cuál es el valor binario de 72? ¿Qué es eso en hexadecimal?.

.. code:: c++

    // Tus respuestas

Antes de leer y escribir binario
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Antes de realizar una lectura o escritura, debes especificar que tu
archivo es binario. Puedes hacerlo a través de la llamada al constructor
o a través de la función ``open``.

-  Lectura

::

   ifstream is("indata.dat", ios::binary); 
   ifstream is;
   is.open("indata.dat", ios::binary);

-  Escritura

::

   ofstream os("outdata.dat", ios::trunc|ios::binary);
   ofstream os;
   os.open("outdata.dat", ios::trunc|ios::binary);

Modos de apertura de archivos binarios
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A continuación, anotamos los diferentes tipos de modos de apertura de
archivos que se usan para abrir un archivo binario usando el objeto de
clase ``fstream``.

-  ``ios::binary | ios::in``: este modo se utiliza para abrir un archivo
   en modo binario para su lectura. El puntero de archivo se coloca al
   principio del archivo. Es el modo predeterminado cuando se abre un
   archivo sin ningún modo especificado.

-  ``ios::binary | ios::out``: este modo se usa para abrir un archivo en
   modo binario para escritura. Si el archivo no existe, se creará. Si
   el archivo ya existe, se sobrescribirá. El puntero de archivo se
   coloca al principio del archivo.

-  ``ios::binary | ios::app``: este modo se usa para abrir un archivo en
   modo binario para agregarlo. Si el archivo no existe, se creará. Si
   el archivo ya existe, todos los datos nuevos se agregarán al final
   del archivo.

-  ``ios::binary | ios::in | ios::out``: este modo se utiliza para abrir
   un archivo en modo binario para lectura y escritura. El puntero de
   archivo se coloca al principio del archivo.

-  ``ios::binary| ios::in | ios::out | ios::trunc``: este modo se usa
   para abrir un archivo en modo binario para lectura y escritura y
   trunca el archivo a cero si ya existe. El puntero de archivo se
   coloca al principio del archivo.

-  ``ios::binary | ios::in | ios::out | ios::app``: este modo se usa
   para abrir un archivo en modo binario para lectura y escritura, y
   todos los datos nuevos se agregarán al final del archivo.

-  ``ios::binary | ios::in | ios::out | ios::ate``: este modo se usa
   para abrir un archivo en modo binario para lectura y escritura, y el
   puntero del archivo se coloca al final del archivo.

El formato general de una lectura y escritura
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

   write(start_address, size);
   read(start_address, size);

Debido a que estás trabajando con bytes de lectura y escritura,
``start_address`` es un puntero a los caracteres (los caracteres solo
tienen un byte de longitud). Si tu dirección no es un puntero a un
carácter, entonces necesitas usar un cast ``(char *)`` para
“falsificarlo”. A partir de esa dirección, las cosas aparecerán como una
secuencia de bytes.

Por ejemplo:

::

   outFile.write(checha.nombre,sizeof(checha.nombre));
   outFile.write((char*)&(checha.edad),sizeof(checha.edad));

**Notas:**

-  ``checha.nombre`` es en realidad una dirección al comienzo de un
   arreglo de caracteres, por lo que no es necesario convertir.

-  ``henry.edad`` es un número entero, lo convertimos en una dirección
   usando ``&`` y lo convertimos en una dirección de caracteres

-  ``sizeof`` es un operador que devuelve el tamaño en bytes. Tomará una
   variable o un especificador de tipo como argumento.

-  No olvides cerrar tus archivos.

