Algunas respuestas de la lista de ejercicios 6
----------------------------------------------

Algunos problemas con la asignación de memoria
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Explica la salida del siguiente código en C++

**Caso: uso de una variable dinámica antes de crearla.**

.. code:: c++

    #include<iostream>
    using namespace std;
    int main()
    {
       char* p = nullptr;
        p[10] = '!';
        return 0;
    }

Debería ser obvio que eliminar la referencia de un puntero a un
almacenamiento no válido provocará un comportamiento indefinido, que
puede incluir un bloqueo o simplemente generar un resultado incorrecto.

La salida del programa es un mensaje de error (dependiendo del sistema
operativo) El error es obvio en este breve ejemplo: nunca se asignó
ninguna variable dinámica a ``p``.

En un programa más grande, el puntero puede colocarse en un lugar,
usarse en otro y eliminarse luego. Puede que no sea obvio el punto donde
ocurrió el error, qué ruta de código condujo al uso de la variable sin
crearla. Es posible que los sistemas operativos embebidos no detecten
una escritura en una dirección no válida y en cambio, pueden
sobrescribir información crítica del sistema, lo que hace que el
programa integrado sea inestable en lugar de colapsar de inmediato.

El mensaje de error de falla de segmentación relativamente informativo
ocurre porque este puntero en particular se inicializó en ``nullptr``.
El sistema operativo atrapó el acceso a la memoria no asignada.

Si el puntero tuviera alguna otra dirección no válida, podría
sobrescribir variables o dañar parte del heap, lo que generaría un
mensaje de error diferente en un punto del programa que está lejos de la
fuente del problema.

Explica la salida del siguiente código en C++

**Caso: eliminar una variable dinámica y luego continuar usando el
puntero como si todavía se refiriera a esa variable dinámica.**

.. code:: c++

    #include <iostream>
    using namespace std;
    
    int main()
    {
        char* p = new char[10];
        p[0] = '!';
        delete[] p;
        cout << "p[0] = " << p[0] << endl;
        return 0;
    }


Cuando compilas y ejecutas el programa la salida en un compilador y
sistema operativo en particular es la siguiente.

::

   p[0] = p

Puede esperar que el valor de ``p[0]`` sea ``'!'`` porque puedes ver la
línea de código que lo estableció en ``'!'``. Sin embargo, cuando el
programa eliminó ``p``, indicó a C++ que el programa había terminado
utilizando el almacenamiento al que apuntaba ``p``.

Después de eso, C++ hizo algo más con él. Podría haber hecho cualquier
cosa. El hecho de que el programa imprimiera el carácter ``p`` es pura
coincidencia. Ciertamente no imprimió ``!``.

Se reserva un círculo especial del infierno para los programas que
cometen este pecado, porque el programa no falla inmediatamente. Los
síntomas de este error incluyen variables que cambian de valor
inesperadamente o un bloqueo en una ``new-expresión`` posterior o una
``delete-expresión`` que puede ocurrir en una línea que está lejos de la
línea del programa donde ocurrió el error.

Explica la salida del siguiente código en C++

**Caso: crear una variable dinámica y luego olvidar de eliminarla**.

.. code:: c++

    #include <iostream>
    using namespace std;
    int main()
    {
        char* p = new char[10];
        p[0] = '!';
        cout << "p[0] = " << p[0] << endl;
        return 0;
    }


La salida se parece a esto:

::

   p[0] = !

Tampoco hay mensajes de error. Entonces, ¿por qué es esto un problema?

El problema es que cada variable dinámica creada con una
``new-expresión`` o ``new[]-expresión`` debe ser eliminada por una
``delete-expresión`` o ``delete[]-expresión`` coincidente. De lo
contrario, el almacenamiento ocupado por la variable se volverá
inaccesible para el programa, es decir, la memoria se perderá del
programa.

Explica la salida del siguiente código en C++

**Caso: si sobrescribes un puntero válido a una variable dinámica.**.

.. code:: c++

    
    #include <iostream>
    using namespace std;
    
    class nclase
    {
        int i_;
    public:
        nclase(int i) : i_(i)
        {
            cout << "construyendo nclase " << i << endl;
        }
       ~nclase()
        {
            cout << "destruyendo  nclase" << i_ << endl;
        }
    };
    
    int main()
    {
        nclase N(1);
        nclase * p = new nclase(2);
        p = new nclase(3);
        delete p;
    
        return 0;
    }


Cuando la ejecución ingresa a ``main()`` se crea una instancia de
``nclase``. Se llama al constructor ``nclase``, imprimiendo el primer
mensaje ``construyendo nclase 1``. La siguiente declaración crea la
primera instancia dinámica ``nclase`` que produce el segundo mensaje
``construyendo nclase 2``. La siguiente declaración construye otra
instancia ``nclase``, ``nclase 3`` y asigna un puntero a esa instancia a
``p`` reemplazando el puntero a ``nclase 2``.

La siguiente declaración elimina ``p`` lo que hace que el destructor de
``nclase 3`` imprima un mensaje. Luego, ``main()`` regresa, lo que hace
que la variable local ``N`` quede fuera del alcance y active una llamada
al destructor de ``nclase 1``.

¿Qué pasó con ``nclase 2``? Se debe eliminar una variable dinámica, pero
no queda ningún puntero en el programa que apunte a ``nclase 2`` porque
fue sobrescrito por el puntero a ``nclase 3`` por lo qué ``nclase 2`` no
se puede eliminar. Sin embargo, ``nclase 2`` no deja de existir. El
sistema operativo no sabe dónde está el almacenamiento de ``nclase 2``.
El sistema operativo entregó la administración del almacenamiento de
``nclase 2`` al programa, pero el programa ha olvidado dónde está
``nclase 2``, ``nclase 2`` se ha filtrado fuera del programa.

Lo que suceda después de eso depende del sistema operativo. Linux libera
toda la memoria utilizada por un programa. Sin embargo, en los sistemas
integrados y en Windows, la memoria perdida puede desaparecer hasta la
próxima vez que se reinicie la computadora. Cada vez que se ejecuta el
programa, otra instancia de ``nclase 2`` se vuelve inaccesible. Si esto
sucede muchas veces, el sistema operativo no tendrá suficiente memoria
para ejecutar programas. Se volverá inestable y causará un pánico en el
kernel del sistema operativo.

Explica la salida del siguiente código en C++

**Caso: eliminar una variable dinámica más de una vez, es decir**.

.. code:: c++

    #include <iostream>
    using namespace std;
    
    int main()
    {
        char* p = new char[10];
        delete[] p;
        delete[] p;
        return 0;
    }

Cuando un programa elimina una variable dinámica, el almacenamiento
vuelve al heap. El puntero sin embargo, no cambia todavía apunta al
comienzo de lo que solía ser la variable dinámica. Si el programa vuelve
a eliminar el mismo puntero, C++ intenta llamar al destructor de la
variable dinámica anterior ya destruida, lo que puede bloquear el
programa.

Luego, C++ intenta colocar el almacenamiento de la variable dinámica
anterior lo que muy probablemente corromperá la lista de bloques de
almacenamiento disponibles.

No todos los compiladores de C++ generan un mensaje de error de tiempo
de ejecución para este programa; depende del compilador y de las
opciones de compilación.

Explica la salida del siguiente código en C++

**Caso: eliminar una variable dinámica con delete en lugar de
delete[]**.

.. code:: c++

    #include <iostream>
    using namespace std;
    
    struct nclase
    {
        nclase() { cout << "construyendo nclase" << endl; }
       ~nclase() { cout << "destruyendo nclase" << endl; }
    };
    
    int main()
    {
        nclase* p = new nclase[3];
        delete p;
        return 0;
    }


Se construyeron tres instancias ``nclase`` pero solo se destruyó una
instancia ``nclase`` en lugar de tres.

En un compilador y en un sistema operativo en particular, C++ detecta un
problema, imprima un mensaje y luego finaliza el programa. No hay
garantía de que C++ detecte un problema. Hay una garantía de que las
otras dos instancias de ``nclase`` no fueron destruidas, por lo que
filtraron las variables dinámicas que contenían.

También existe la posibilidad de que que parte del heap se haya dañado,
por lo que, eventualmente, el programa se volverá inestable.

Explica la salida del siguiente código en C++

**Caso: eliminar una variable dinámica con delete[] en lugar de
delete**.

.. code:: c++

    #include <iostream>
    using namespace std;
    
    struct nclase
    {
        nclase() { cout << "construyendo nclase" << endl; }
       ~nclase() { cout << "destruyendo nclase << endl; }
    };
    
    int main()
    {
        nclase* p = new nclase;
        delete[] p;
    }


-  ``new[]-expresion`` guarda el tamaño del arreglo que asigna para que
   pueda llamar a un destructor para cada instancia del arreglo. Una
   new-expresión no guarda este valor.

-  ``delete[]-expresión`` busca el número secreto de instancias y lee la
   basura. A menos que la basura sea igual a uno, lo cual es bastante
   improbable, ``delete[]`` intentará eliminar las instancias que no
   existen. El programa luego descenderá al círculo caótico del infierno
   llamado comportamiento indefinido.

Administrar variables dinámicas es lo suficientemente difícil como para
que el C++ moderno defina algo llamado puntero inteligente, es decir,
una clase que elimina automáticamente la variable dinámica cuando el
puntero inteligente queda fuera del alcance. Los punteros inteligentes
son la única buena manera de manejar variables dinámicas.

Hasta que lleguemos a los punteros inteligentes, debemos tratar las
variables dinámicas a la antigua.

Punteros inteligentes
~~~~~~~~~~~~~~~~~~~~~

En este ejercicio construye y destruye algunas instancias de
``unique_ptr<>`` que apuntan a variables dinámicas y arreglo dinámicas y
muestra cómo transferir la propiedad de una variable dinámica de un
``unique_ptr<>`` a otro.

.. code:: c++

    //

Demuestra cómo usar ``make_unique()`` para ocultar nuevas expresiones.

.. code:: c++

    //

En este ejercicio crea una clase simple cuyos miembros son punteros
inteligentes para ilustrar la sintaxis de clase simplificada que resulta
cuando los punteros inteligentes como ``unique_ptr<>`` son miembros de
clase.

.. code:: c++

    //

Desarrolla un ejemplo para mostrar cómo transferir la propiedad de una
variable dinámica devolviéndole una instancia ``unique_ptr<>``.

.. code:: c++

    // 

En este ejercicio, crea algunos punteros compartidos y llama a una
función que toma un puntero compartido como argumento y devuelve un
puntero compartido. La idea de este programa es que una función
condiciona una variable dinámica para que nunca sea ``nullptr``, por lo
que el programa no tiene que probar ``nullptr``.

.. code:: c++

    //

Escribe un ejemplo del uso ``make_shared()``.

.. code:: c++

    // 

**Referencia:** `Smart pointers (Modern
C++) <https://learn.microsoft.com/en-us/cpp/cpp/smart-pointers-modern-cpp?view=msvc-170>`__.

Estructuras
~~~~~~~~~~~

Escribe un programa en C++ que procese la fecha de nacimiento usando
estructuras. Incluye la capacidad de procesar las fechas de nacimiento
de varios estudiantes.

.. code:: c++

    #include <iostream>
    using namespace std;
    
    struct Fecha {
        int dia;
        int mes;
        int year;
    };
    
    struct Estudiante {
        string nombre;
        Fecha dob;
    };
    
    void mostrarDetallesEstudiantes(Estudiante estudiante) {
        cout << "Nombre: " << estudiante.nombre << endl;
        cout << "Cumple: " << estudiante.dob.dia<< "/"
             << estudiante.dob.mes << "/" << estudiante.dob.year << endl;
        cout << endl;
    }
    
    int main() {
        const int MAX_ESTUDIANTES = 3;
        Estudiante estudiantes[MAX_ESTUDIANTES];
    
        for (int i = 0; i < MAX_ESTUDIANTES; i++) {
            cout << "Ingresa el nombre del estudiante " << i + 1 << ": ";
            cin >> estudiantes[i].nombre;
            cout << "Ingresa la fecha del cumple (dd mm yyyy) para estudiante" << i + 1 << ": ";
            cin >> estudiantes[i].dob.dia >> estudiantes[i].dob.mes >> estudiantes[i].dob.year;
            cout << endl;
        }
    
        cout << "Detalles de estudiantes:" << endl;
        for (int i = 0; i < MAX_ESTUDIANTES; i++) {
            mostrarDetallesEstudiantes(estudiantes[i]);
        }
    
        return 0;
    }


Escribe un programa que almacene registros de empleados en un archivo.
El programa debería poder leer los registros y mostrarlos en la
pantalla.

.. code:: c++

    #include <fstream>
    #include <iostream>
    using namespace std ;
    int main( )
    {
    struct empleado
    {
        char nombre[ 20 ] ;
        int edad ;
        float basico;
        float total ;
    };
    empleado e ;
    
    char ch = 'Y' ;
        ofstream outfile ;
        outfile.open ( "empleado.txt", ios::out | ios::binary ) ;
        while ( ch == 'Y' || ch == 'y' )
        {
        cout << "Ingresa un registro:" ;
            cin >> e.nombre >> e.edad >> e.basico>> e.total ;
            outfile.write ( ( char * ) &e, sizeof ( e ) ) ;
            cout << "Agrega otro Y/N? " ;
            cin >> ch ;
    }
    outfile.close( ) ;
        
    ifstream infile ;
    infile.open ( "empleado.txt", ios::in | ios::binary ) ;
    while ( infile.read ( ( char * ) &e, sizeof ( e ) ) )
    {
        cout << e.nombre << '\t' << e.edad << '\t'
        << e.basico<< '\t' << e.total << endl ;
        }
    return 0 ;
    }

Realiza la estructura punto con los siguientes atributos, etiqueta del
punto, coordenada x, coordenada y donde se ingrese 3 coordenadas:

Se pide utilizar funciones, el operador ``new``\ y estructuras:

::

   Ingresar los 3 puntos.
   Listar los puntos.
   Validar si es isósceles.

**Ejemplo:**

::

   Listar Datos Etiqueta: A x: 2 y: 1
   Etiqueta: B x: 4 y: 1
   Etiqueta: C x: 3 y: 3

   El triángulo es Isósceles.

.. code:: c++

    #include<iostream>
    #include<cstdlib>
    #include<string>
    #include<cmath>
    #define N 3
    
    using namespace std;
    
    struct punto{
        string etiqueta;
        float x;
        float y;
    };
    void ingresar(struct punto pun[N]);
    void listar(struct punto pun[N]);
    int es_isoceles(struct punto pun[N]);
    void ingresar(struct punto pun[N]){
        struct punto p[3]={
            {"A",2,1},
            {"B",4,1},
            {"C",3,3},
    
        };
        int i = 0;
        while( i < N ) {
            pun[i].etiqueta = p[i].etiqueta;
            pun[i].x = p[i].x;
            pun[i].y = p[i].y;
            i++;
        }
    }
    int es_isoceles(struct punto pun[N]){
        float L1,L2,L3;
        L1=sqrt(pow(pun[0].x-pun[1].x,2)+pow(pun[0].y-pun[1].y,2));
        L2=sqrt(pow(pun[1].x-pun[2].x,2)+pow(pun[1].y-pun[2].y,2));
        L3=sqrt(pow(pun[2].x-pun[0].x,2)+pow(pun[2].y-pun[0].y,2));
        
        if( L1 == L2 || L1 == L3 || L2 == L3 ){
            cout<< "\nEl triangulo es Isoceles\n" ;
            return 1;
        }else{
            return 0;
            cout<< "\nEl triangulo no es Isoceles\n" ;
       }
    }
    void listar(struct punto pun[N]){
        int i;
        for(i = 0; i < N; i++){
            cout<< "Etiqueta: "<< pun[i].etiqueta << "  x: "<< pun[i].x << " y: "<< pun[i].y <<"\n";
        }
    }
    int main(){
        punto *P;
        P = new punto[N];
        ingresar(P);
        cout<<"Listar Datos \n";
        listar(P);
        if (es_isoceles(P)){
            cout<< "\nEl triangulo es Isoceles\n" ;
        }else{
            cout<< "\nEl triangulo No es Isoceles\n" ;
        }
        return 0;
        delete [] P;
    }

Implementar un programa para mantener una base de datos con información
de accesorios o partes de equipos informáticos. Para esto deberás
incluir lo siguiente:

El tipo estructura ``accesorio`` cuyos campos son el número de partes,
nombre, cantidad (stock).

Define el arreglo ``inventario`` que tendrá un tamaño límite de 100
accesorios.

En la función ``main`` se presentará un menú que pedirá el ingreso de un
código de operación, luego se realizará alguna de las siguientes tareas:

-  Insertar: intenta añadir un nuevo accesorio, si fuera posible.
   Muestra un mensaje de error si el número de parte ya existe.
-  Buscar: Solicita un número de accesorio y lo busca en la base de
   datos. Si la ubica, imprime el nombre y stock, sino imprime un
   mensaje de error.
-  Actualizar: Solicita un número de accesorio y lo busca en la base de
   datos. Si el número de parte no existe muestra un mensaje de error,
   sino le pide al usuario ingresar la cantidad y realiza el cambio.
-  Listar: Presenta todos los accesorios adquiridos. Los accesorios se
   mostrarán en el orden que fueron ingresados. El menú se presentará de
   forma interactiva hasta que el usuario presione el código 5.

::

   MENU

   (1) Insertar
   (2) Buscar
   (3) Actualizar
   (4) Listar
   (5) Salir

.. code:: c++

    #include <iostream>
    #include <cstdio>
    #include <string>
    #define TAM 80
    using namespace std;
    struct TAccesorio{
         int parte; 
         string nombre;
         int cantidad;
    };
    struct TAccesorio inv[TAM];
    int menu();
    void Listar(struct TAccesorio inv[],int n);
    int Ubicar_np(int np,struct TAccesorio inv[],int n);
    void Mostrar(int np,struct TAccesorio inv[],int n);
    void Actualizar(int np,struct TAccesorio inv[],int n);
    int main(){
       int i,j,k,n,nparte,y;
       int opc;
       char resp;
       n=0;
       do{
            opc=menu();
        switch(opc){
            case 1:  
                    cout<<"Numero de parte: ";cin>>nparte;
            y=-1;
                    if (n>0)          
                       y=Ubicar_np(nparte,inv,n);
          if (y==-1){
            cout<<"Accesorio Nro. "<<n+1<<"\n";
                inv[n].parte=nparte;
                            cin.ignore();
                cout<<"Nombre: ";
                getline(cin,inv[n].nombre);
                cout<<"Cantidad: ";cin>>inv[n].cantidad;  
                n=n+1;
                    }
            else{  
                        cout<<"Accesorio ya existe\n"; 
            }
                    break;                 
               case 2: //Ubicar    
                    cout<<"Numero de parte: ";cin>>nparte;
                    Mostrar(nparte,inv,n);                 
                    break;
               case 3: //Actualizar   
                    cout<<"Numero de parte: ";cin>>nparte;
                    Actualizar(nparte,inv,n);                 
                    break;
                case 4: 
            Listar(inv,n);
                    break;
            }
            cout<<"¿Desea continuar?(S/N): ";
            cin>>resp;
         }while (resp=='S');
       return 0;
    }
    int Ubicar_np(int np,struct TAccesorio inv[],int n){
       int hallado=-1;
       for (int i=0;i<n;i++)
           if (np==inv[i].parte)
                  hallado=i;
       return  hallado;
    }
    void Actualizar(int np,struct TAccesorio inv[],int n){
       int y,cant;
       y=Ubicar_np(np,inv,n);
       if ( y!=-1){
          cout<<"\nIngrese nueva cantidad: ";
          cin>>cant;
          inv[y].cantidad=cant;
          cout<<"Se actualizaron los datos...\n";  
       }
       else
          cout<<"Error. Codigo de accesorio no existe.\n";
    }
    void Mostrar(int np,struct TAccesorio inv[],int n){
       int y;
       y=Ubicar_np(np,inv,n);
       if ( y!=-1){
          cout<<"\nNombre: "<<inv[y].nombre;
          cout<<"\nCantidad: "<<inv[y].cantidad;  
       }
       else
          cout<<"Error. Codigo de accesorio no existe.\n";
    }
    
    void Listar(struct TAccesorio inv[],int n){
       cout<<"Datos de los accesorios\n";
       for (int i=0;i<n;i++){
         cout<<"\nAccesorio Nro. "<<i+1;
         cout<<"\nNumero de Parte: "<<inv[i].parte;
         cout<<"\nNombre: "<<inv[i].nombre;
         cout<<"\nCantidad: "<<inv[i].cantidad<<endl;
       }
    }
    int menu(){
      int opc;
      cout<<"\tMENU\n";
      cout<<"(1)Insertar\n";  
      cout<<"(2)Buscar\n";
      cout<<"(3)Actualizar\n";
      cout<<"(4)Listar\n";
      cout<<"(5)Salir\n";
      cout<<"Elija la opcion: ";cin>>opc;
      return opc;
    }


Escribe un programa en C++ para procesar números complejos. Tienes que
realizar sumas, restas, multiplicaciones y divisiones de números
complejos. Imprime los resultados en forma x+ iy.

.. code:: c++

    #include <iostream>
    using namespace std;
    
    struct Compleja {
        double real;
        double imaginaria;
    };
    
    Compleja sumaCompleja(Compleja c1,Compleja c2) {
       Compleja resultado;
        resultado.real = c1.real + c2.real;
        resultado.imaginaria = c1.imaginaria + c2.imaginaria;
        return resultado;
    }
    
    Compleja substraccionCompleja(Compleja c1,Compleja c2) {
       Compleja resultado;
        resultado.real = c1.real - c2.real;
        resultado.imaginaria = c1.imaginaria - c2.imaginaria;
        return resultado;
    }
    
    Compleja multiplicacionCompleja(Compleja c1,Compleja c2) {
       Compleja resultado;
        resultado.real = (c1.real * c2.real) - (c1.imaginaria * c2.imaginaria);
        resultado.imaginaria = (c1.real * c2.imaginaria) + (c1.imaginaria * c2.real);
        return resultado;
    }
    
    Compleja divisionCompleja(Compleja c1,Compleja c2) {
       Compleja resultado;
        double denominador = (c2.real * c2.real) + (c2.imaginaria * c2.imaginaria);
        resultado.real = ((c1.real * c2.real) + (c1.imaginaria * c2.imaginaria)) / denominador;
        resultado.imaginaria = ((c1.imaginaria * c2.real) - (c1.real * c2.imaginaria)) / denominador;
        return resultado;
    }
    
    void impresionCompleja(Compleja c) {
        cout << c.real << " + " << c.imaginaria << "i" << endl;
    }
    
    int main() {
       Compleja num1, num2;
    
        cout << "Ingresa la parte real e imaginaria del primero numero complejo: ";
        cin >> num1.real >> num1.imaginaria;
    
        cout << "Ingresa la parte real e imaginaria del segundo numero complejo: ";
        cin >> num2.real >> num2.imaginaria;
    
        cout << "Adicion de numeros complejos: ";
        impresionCompleja(sumaCompleja(num1, num2));
    
        cout << "Resta de numeros complejos: ";
        impresionCompleja(substraccionCompleja(num1, num2));
    
        cout << "Multiplicacion de numeros complejos: ";
        impresionCompleja(multiplicacionCompleja(num1, num2));
    
        cout << "Division de numeros complejos: ";
        impresionCompleja(divisionCompleja(num1, num2));
    
        return 0;
    }

