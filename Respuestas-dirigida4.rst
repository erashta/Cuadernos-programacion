Algunas respuestas de la lista de ejercicios 4
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Escribe un programa en C ++ para extraer una sub-cadena de una cadena
dada.

.. code:: c++

    #include <iostream>
    using namespace std;
    
    int main()
    {
       char str[100], sstr[100];
       int pos, l, c = 0;
       
       cout<<"\n\nExtraer una subcadena de una cadena dada:\n";
       cout<<"Ingrese la cadena : ";
      
       fgets(str, sizeof str, stdin); // fgets
       cout<<"Ingrese la posición para iniciar la extraccion:";
       cin>>pos;
       cout<<"Ingrese la longitud de la subcadena:";
       cin>>l;
       
     
       while (c < l)
       {
          sstr[c] = str[pos+c-1];
          c++;
       }
       sstr[c] = '\0';
       cout<<"La subcadena recuperada de la cadena es : \n"<<sstr;
       
       return 0;
    }


Implementa la función: ``void rotar(int r, char *p, char *q)`` la cual
rota r posiciones a las letras de la cadena apuntada por p y las
almacena en una cadena apuntada por q.

.. code:: c++

    #include<iostream>
    #include<cstring>
    using namespace std;
    
    void rotar(int r, char *p, char *q);
    int main(){
        char p[] = "Hola Carlitos", q[20];
        rotar(3, p, q);
        cout<<p<<endl;
        cout<<q<<endl;
    }
    void rotar(int r, char *p, char *q){
        int i, n = strlen(p);
        char *ip;
        for(i=0; *p; i++) q[(i+r)%n] = *p++;
        q[n] = 0;
    }


Escribe una función para imprimir una cadena estilo C carácter por
carácter. Utiliza un puntero para recorrer cada carácter de la cadena e
imprimir ese carácter. Detente cuando llegues a null. Escriba una
función ``main`` que pruebe la función con el literal de cadena “Hello
world!”.

.. code:: c++

    #include <iostream>
    
    void imprimeCString(const char* str)
    {
    
      while (*str != '\0')
      {
    
        std::cout << *str;
    
        ++str;
      }
    }
    
    int main()
    {
      imprimeCString("Hello world!");
    
      std::cout << '\n';
    
      return 0;
    }


Defina un arreglo de cadenas en las que las cadenas contengan los
nombres de los meses. Imprime esas cadenas. Pasa el arreglo a una
función que imprima esas cadenas.

.. code:: c++

    #include <iostream>
    #include <string>
    using namespace std;
    
    void imprime_meses(string a[],int i = 12)
    {
        for (auto j = 0; j < i; j++)
            cout << a[j] << endl;
        
    }
    
    int main()
    {
        string meses[]{ "Enero","Febrero","Marzo","Abril",
            "Mayo","Junio","Julio","Agosto","Setiembre",
                "Octubre","Noviembre","Diciembre" };
        imprime_meses(meses);
        return 0;
    }

Leer una secuencia de palabras de la entrada. Usa ``Salir`` como una
palabra que termina la entrada. Escriba las palabras en el orden en que
fueron ingresadas. No escribas una palabra dos veces.

Modifica el programa para ordenar las palabras antes de imprimirlas. Usa
para simplificar ``<vector>`` y ``<algorithm>``. (Ejercicio opcional)

.. code:: c++

    #include <iostream>
    #include <string>
    #include <vector>
    #include <algorithm>
    
    using namespace std;
    
    int main()
    {
        vector<string> vs;
        for (string s; cin >> s; )
            if (s != "salir" && find(vs.begin(),vs.end(),s)==vs.end())
                vs.push_back(s);
       else break;
        cout << "orden desordenado：" << endl;
        for (auto i : vs)
        cout << "    " << i << endl;
            cout << "orden ordenado:" << endl;
        sort(vs.begin(), vs.end());
        for (auto i : vs)
           cout << "    " << i << endl;
        return 0;
    }


Escribe una función ``cat()`` que tome dos argumentos de cadena de
estilo C y devuelva una cadena que sea la concatenación de los
argumentos. Usa ``new`` para encontrar almacenamiento para el resultado.

.. code:: c++

    #include <iostream>
    #include <string>
    using namespace std;
    
    string* cat(const char *a, const char *b)
    {
        string as = string(a);
        string bs = string(b);
        return new string(as + bs);
    }
    
    int main()
    {
        const char *a = { "qwerty" };
        const char *b = { "zxcvbn" };
        auto s = cat(a, b);
        cout << *s << endl;
        return 0;
    }


Todos los procesadores de textos, tienen la opción de “buscar y
reemplazar”. Escribe un programa en C++, implementando la función
``void reemplazar(char *linea, char *strbusc, char *streemp)`` tal que
se leen la cadena a ``buscar(strbusc)`` y la cadena que va a
reemplazarla ``(streemp)``.

Luego se lee un texto (línea). Si la cadena a buscar se encuentra en la
línea, se reemplazará por la segunda cadena tantas veces como se
encuentre. Las líneas nunca superarán los 100 caracteres.

Sugerencia: Usa las funciones ``strlen``, ``strcpy`` y ``strcat``.

::

   Ingrese la cadena a buscar  :  Amazon
   Ingrese la cadena a reemplazar : CC
   * Ingrese el texto :
   Amazon tiene cuentas en AWS Educate para experimentos
   * Texto reemplazado :
    CC  tiene cuentas en AWS  Educate para experimentos
    

.. code:: c++

    #include<iostream>
    #include<cstdio>
    #include<cstdlib>
    #include<cstring>
    
    using namespace std;
     
    void reemplazar(char *, char *, char *);
    #define Max_Tam 100
    int main ()
    {
       char strbusc[20], streemp[20];
       char linea[Max_Tam];
    
       cout<<"Ingrese la cadena a buscar : ";
       cin.getline(strbusc,20);
       cout<<"Ingrese la cadena a reemplazar : ";
       cin.getline(streemp,20);
    
       cout<<"* Ingrese el texto : "<<endl;
       cin.getline(linea,Max_Tam);
       reemplazar(linea, strbusc, streemp);
       cout<<"* Texto reemplazado : "<<endl;
       puts(linea);
       return 0;
    }
    
     void reemplazar(char *linea, char *strbusc, char *streemp)
      {  char *ptLinea=linea, *pos, aux[Max_Tam];
     
         while (1)
           pos = strstr(ptLinea,strbusc);
            if (pos==NULL) return;
            *pos=0;
            strcpy(aux,linea); // copiando la parte izquierda de la cadena
            strcat(aux,streemp); // agregando "aux" la cadena de reemplazo
            strcat(aux,pos+strlen(strbusc)); // copiando la parte derecha de la cadena
            strcpy(linea,aux);
            ptLinea= pos+strlen(streemp);  // desplazando el puntero para permitir ubicar una nueva ocurrencia
          }
     }


Escribe una función ``atoi(const char∗)`` que tome una cadena estilo C
que contenga dígitos y devuelva el int correspondiente. Por ejemplo,
``atoi("123")`` es ``123`` . Modifica ``atoi()`` para manejar la
notación octal y hexadecimal de C++ además de números decimales simples.
Modifica ``atoi()`` para manejar la notación constante de caracteres de
C++.

.. code:: c++

    #include<iostream>
    #include<string>
    
    int atoi(const char* c)
    {
        int r{};
        for (auto i = 0; *(c + i) != '\0'; i++)
        {
            r *= 10;
           r += *(c + i) - '0';
        }
        return r;
    }
    
    int atoi(int x)
    {
        return x;
    }
    
    int atoi(const char c)
    {
        return c - '0';
    }
    
    int main()
    {
        int a{ 017 };
        int b{ 0x17 };
        int c{ 17 };
        const char *p = "123456789";
        const char d{ '1' };
        std::cout << a << std::endl;
        std::cout << atoi(a) << std::endl;
        std::cout << b << std::endl;
        std::cout << atoi(b) << std::endl;
        std::cout << c << std::endl;
        std::cout << atoi(c) << std::endl;
        std::cout << p << std::endl;
        std::cout << atoi(p) << std::endl;
        std::cout << d << std::endl;
        std::cout << atoi(d) << std::endl;
        return 0;
    }


Escribe una función ``itoa(int i, char b[])`` que cree una
representación de cadena de ``i`` en ``b`` y devuelva ``b`` .

.. code:: c++

    #include <iostream>
    #include <string>
    
    using namespace std;
    
    char * itoa(int i, char b[])
    {
        int temp[i];
        int pa[1];
        while (temp /= 10)
            pa++;
        for (auto j = 0; j < pa; j++)
        {
           int k = i % 10;
            b[p - 1 - j] = k+'0';
               i /= 10;
        }
        return b;
    }
    
    int main()
    {
        int i{1234567890};
        char p[20]{};
        std::cout << itoa(i,p) << std::endl;
        return 0;
    }

En un salón de clase hay un letrero de 4 líneas:

::

   La hora es la hora
   Ante de la hora no es la hora
   Después de la hora tampoco es la hora.
   La hora es la hora.

Escribe un programa en C++que cuente las letras mayúsculas y las
minúsculas, así como el número de espacios blancos, puntos y finalmente
el número total de caracteres.

Sugerencia: Guarda el letrero en char letrero[4][40]. La salida será:

::

   Número de mayúsculas: 4
   Número de minúsculas: 75
   Número de  espacios en blanco: 22
   Número de puntos: 1
   Número total de caracteres: 102

.. code:: c++

    #include<iostream>
    #include<cstring>
    #include<cctype>  // nuevos encabezados
    using namespace std;
    
    main(){
        char letrero[4][40] = {"La hora es la hora",
           "Antes de la hora no es la hora",
               "Despues de la hora tampoco es la hora",
               "La hora es la hora."}, 
        *pinicio=letrero[0], *pi;    
        int nl = 4, nMay=0, nMin=0, nBlan=0, nPuntos=0, nTotal, i;
        for(i=0; i<nl; i++) {
           for(pi= pinicio+i*40; *pi; pi++){
            if(isupper(*pi)) nMay++;
           if(islower(*pi)) nMin++;
             if(*pi == ' ')   nBlan++;
           if(*pi == '.')   nPuntos++;
       }
           nTotal = nMay + nMin + nBlan + nPuntos;
        }
        
        cout<<"Numero de mayusculas: "<<nMay<<endl;
        cout<<"Numero de minusculas: "<<nMin<<endl;
        cout<<"Numero de blancos: "<<nBlan<<endl;
        cout<<"Numero de puntos: "<<nPuntos<<endl;
        cout<<"Numero total de caracteres: "<<nTotal<<endl;
        
    }


Escribe un programa que elimine los comentarios de un programa C++. Es
decir, lea desde ``cin``, elimine los comentarios ``//`` y los
comentarios ``/∗ ∗/`` y escriba el resultado en ``cout``.

No te preocupes por hacer que el diseño de la salida se vea bien (ese
sería otro ejercicio y mucho más difícil). No te preocupes por los
programas incorrectos. Ten cuidado con ``//`` , ``/∗`` y ``∗/`` en
loscomentarios, cadenas y constantes de caracteres.

.. code:: c++

    #include <iostream>
    #include <string>
    
    using namespace std;
    
    
    string& strip(string& s)
    {
        while (s.find("//") or  s.find_first_of("/*"))
        {
           auto pos1 = s.find_first_of("//");
           if (pos1==std::string::npos)
               break;
        auto pos2 = s.find_first_of("\n", pos1);
        s.erase(pos1, pos2-pos1+1);
        auto pos3 = s.find_first_of("/*");
           if (pos3 == std::string::npos)
               break;
            auto pos4 = s.find_first_of("*/",pos3);
                s.erase(pos3, pos4 - pos3 + 1);
        }
        return s;
    }
    
    int main()
    {
        string s{ };
        cin >> s;
        cout << strip(s) << endl;
        return 0;
    }


En un salón de clase se dicta una conferencia sobre Programación en C++,
los asistentes anotan sus nombres en una lista, al final se rifa (número
aleatorio de 1 a número de asistentes), un premio y se anuncia el número
y nombre del ganador. Escribe un programa en C++para controlar el
ingreso. Suponga que los asistentes son:

::

   char lista[30][10]= {"Juan", "María", "José", "Elena", "Fiore", "Rosita"}, nl= 0;

Cálculo de ``nl = número de asistentes``:

::

   for(i=0;i<10;i++) if(strlen(lista[i])) nl++ ;

La salida puede ser:

::

   El ganador es: 3 José

.. code:: c++

    #include<iostream>
    #include<cstring>
    #include<cstdlib>
    #include<ctime>
    using namespace std;
    
    int  main(){
        char lista[30][10] = {"Juan", "María", "José", "Elena", "Fiore", "Rosita"};
        int nl=0, i, nGanador;
        for(i=0;i<10;i++) if(strlen(lista[i])) nl++ ;
        srand(time(NULL));
        nGanador = rand()%nl;
        for(i=0;i<nl;i++) if(nGanador==i) break;
        cout<<"El ganador es: \n"<<nGanador+1<<" "<<lista[nGanador];
        //printf("El ganador es: %d  %s\n", nGanador+1, lista[nGanador]);
    }


Escribe un programa que escriba la segunda palabra (y su longitud) en un
arreglo de caracteres. Las palabras están separadas por espacios,
puntuación y tabulaciones.

.. code:: c++

    #include<iostream>
    bool esSeparador(char c, char* separadores) {
        char finDeCadena = '\0';
        char* sepPtr = separadores;
        bool ret = false;
        while(*sepPtr != finDeCadena) {
           if (c == *sepPtr) {
             ret = true;
               break;
            }
           sepPtr++;
        }
        return ret;
    }
    int charArrayLength(char* a) {
        int ret = 0;
        char finDeCadena = '\0';
        while(*a != finDeCadena) {
           ret++;
        a++;
        }
        return ret;
    }
    
    int main(int argc, char** argv) {
    using namespace std;
    
     int ret = 0;
     char finDeCadena = '\0';
     char laOracion[] = "Deberias estar aquí ?";
     char separadores[] = " \t,.;:?!";
     char* strPtr = laOracion;
     char*strFinal;
    
    
    bool esSegundaPalabra = false;
    while(*strPtr != finDeCadena) {
        while(esSeparador(*strPtr, separadores)) {
        strPtr++;
        esSegundaPalabra = true;
        }
        if (esSegundaPalabra) {
        break;
        } else {
          strPtr++;
        }
    }
    
    strEnd = strPtr;
    while(*strEnd != finDeCadena) {
        if (esSeparador(*strEnd, separadores)) {
    
           *strEnd = finDeCadena;
        break;
        }
    
       strFinal++;
        }
        cout << strPtr << "(" << charArrayLength(strPtr) << ")" << endl;
        return ret;
    }
    


