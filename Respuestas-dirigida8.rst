Algunas respuestas de la lista de ejercicios 8
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Dos alumnos del curso de fundamentos de programación, Diego Alberto y
Fernando Brian, se animaron a jugar Yan ken po, conocido también como el
juego de piedra, papel y tijera, ¿quién ganará? Número de partidas (n):
5

Un posible resultado puede ser:

::

        Diego         Fernando    Ganador
        Piedra        Tijera      Diego
        Piedra        Tijera      Diego
        Papel          Papel      Empate
        Piedra         Piedra     Empate
        Papel         Piedra      Diego

El ganador de la partida es: Diego

.. code:: ipython3

    #include<iostream>
    #include<ctime>
    using namespace std;
    
    typedef struct{
        int d;  // Diego
        int f;    // Fernando
    } Juego;
    
    int ganar(int d, int f){        // entrada: d=juego de Diego, f=juego de Fernando
                         // salida: 0=gana Diego, 1=gana Fernando, 2=empate
        if(d==f)  return 2;	
        if(d==0)      return (f==1)? 1:0;
       else if(d==1) return (f==2)? 1:0;
                return (f==0)? 1:0;
    }
    int main(){
        int n=5, i;
        Juego *partida = new Juego[n], *pp= partida;   // Memoria dinamica para el puntero pp
        srand(time(NULL));
    // Cargar partida en memoria dinamica: 0=piedra, 1=papel, 2=tijera 
        for(i=0;i<n;i++, pp++){
            pp->d = rand()%3;
            pp->f = rand()%3;
        }
    
    // Reportar la partida
        char ppt [3][10] = {"Piedra", "Papel", "Tijera"};
        char jugadores [3][10] = {"Diego", "Fernando", "Empate"};
        int ganador, resultado[3] = {0,0,0};
        cout << "Juego\tDiego\tFernando\tGanador\n";
        pp = partida;
        for(i=0;i<n;i++, pp++){
            ganador = ganar(pp->a, pp->b);
            resultado[ganador]++; 
            cout << i+1 << "\t" << ppt[pp->a] << "\t" << ppt[pp->b] << "\t"  << jugadores[ganador] << endl;
        }
    
    // Reporte del ganador
        if(resultado[0] == resultado[1]) 	 cout << "Hay empate"<<endl;
        else if(resultado[0] > resultado[1]) cout << "El ganador de la partida es: " << jugadores[0] << endl;
             else         cout << "El ganador de la partida es: " << jugadores[1] << endl;
        delete [] partida;
    }


Una empresa dedicada al mantenimiento de maquinaria pesada desea
consolidar el calendario de trabajo de sus vehículos y operarios. En
cada visita técnica los vehículos pueden llevar desde 2 a 5 operarios y
se registra el tiempo que demora la visita. El ingreso de los datos de
cada visita comienza con el código del vehículo (entre 1 y 20), el
número de operarios (entre 2 y 5), los códigos de cada operario (entre 1
y 100) y el tiempo (en horas) que toma la visita, luego el programa pide
si el usuario desea continuar con el ingreso de datos. Un vehículo
necesita mantenimiento si excede las 100 horas acumuladas de operación.

Escribe un programa de C++ que utilice un puntero para administrar un
arreglo dinámico T de estructuras con los datos de las visitas y luego
cree un arreglo dinámico V de estructuras con los códigos de los
vehículos y tiempo acumulado, ordenado en forma descendente por tiempo.

.. code:: ipython3

    #include <iostream>
    using namespace std;
    
    struct VISITA {
        int codVehiculo;
        int numOperarios;
        int numHoras;
        int *vecOperarios;
    };
    struct MANTENIMIENTO {
        int codVehiculo;
        int numHorasAcumuladas;
    };
    int maxHorasVehiculo=100;
    int main(){
        VISITA *lstVisita= new VISITA[1];
        int numVisita=0;
        // ingreso de datos
        while (1){
            char rpta='x';
            int codVehiculo, numOperarios, numHoras;
            cout<<"Ingrese codigo del vehiculo: ";
            cin>>codVehiculo;
            lstVisita[numVisita].codVehiculo=codVehiculo;
            cout<<"Ingrese numero de operarios: ";
                cin>>numOperarios;
            lstVisita[numVisita].numOperarios=numOperarios;
            lstVisita[numVisita].vecOperarios = new int[numOperarios];
            for (int i=0;i<numOperarios;i++){
                cout<<"\tIngrese codigo del operario "<<i+1<<": ";
                    cin>>lstVisita[numVisita].vecOperarios[i];
            }
            cout<<"Ingrese numero de horas de la visita: ";
            cin>>numHoras;
            lstVisita[numVisita].numHoras=numHoras;
            numVisita++;
            // preguntamos si el usuario continua el ingreso
            while ((rpta!='n') && (rpta!='s')){
                cout<<"Ingresa una nueva visita (s)i o (n)o ?: ";
                cin>>rpta;
                cin.ignore();
            }
            if (rpta=='n') 	{
                break;
            } else {
            // reasignacion de memoria
                VISITA *p = new VISITA[numVisita+1];
                for(int i=0; i<numVisita; i++){
                    p[i]=lstVisita[i];
                }
                lstVisita=p;
            }
        }
        // totalizamos las horas de cada vehiculo
        int maxNumVehiculo=20;
        int *horasVehiculo = new int[maxNumVehiculo];
        for(int i=0; i<maxNumVehiculo; horasVehiculo[i++]=0);
        for(int i=0; i<numVisita; i++){
            int codVehiculo = lstVisita[i].codVehiculo;
                horasVehiculo[codVehiculo-1]+=lstVisita[i].numHoras;
        }
        // contamos cuantos vehiculos van a mantenimento
        int numVehiculoMantenimiento=0;
        for(int i=0; i<maxNumVehiculo; i++){
            numVehiculoMantenimiento += (horasVehiculo[i]>maxHorasVehiculo)?1:0;
        }
        // creamos la lista de vehiculos que van a mantenimento
        MANTENIMIENTO *lstVehiculos= new MANTENIMIENTO[numVehiculoMantenimiento];
        for(int i=0; i<maxNumVehiculo; i++){
            if (horasVehiculo[i]>maxHorasVehiculo){
              lstVehiculos[i].codVehiculo=i+1;
                lstVehiculos[i].numHorasAcumuladas=horasVehiculo[i];
            }
        }
        // ordenamos la lista de vehiculos que van a mantenimento
        for(int i=0; i<numVehiculoMantenimiento-1; i++){
            for(int j=0; j<numVehiculoMantenimiento-i-1; j++){
                if (lstVehiculos[j].numHorasAcumuladas<lstVehiculos[j+1].numHorasAcumuladas){
                    MANTENIMIENTO aux = lstVehiculos[j];
                    lstVehiculos[j] =lstVehiculos[j+1];
                    lstVehiculos[j+1] = aux;
                }
            }
        }
        // imprimimos la lista de vehiculos que van a mantenimento
        for(int i=0; i<numVehiculoMantenimiento; i++){
            cout<<"Codigo de vehiculo en mantenimiento: "
                <<lstVehiculos[i].codVehiculo<<endl;
            cout<<"Horas acumuladas de operacion: "
                <<lstVehiculos[i].numHorasAcumuladas<<endl;
        }
        cin.get();
    }

Escribe un programa C++ para buscar y reemplazar una palabra específica
en un archivo de texto.

.. code:: ipython3

    #include <iostream>
    #include <fstream>
    #include <string>
    
    void displayFileContent(const std::string & filename) {
      std::ifstream file(filename);
      std::string linea;
    
      if (file.is_open()) {
        std::cout << "Contenido del archivo:" << std::endl;
        while (std::getline(file, linea)) {
          std::cout << linea << std::endl;
        }
        file.close();
      } else {
        std::cout << " Fallo en abrir el archivo." << std::endl;
      }
    }
    
    int main() {
      std::ifstream inputFile("prueba.txt");
      std::ofstream outputFile("nueva_prueba.txt");
    
      if (inputFile.is_open() && outputFile.is_open()) {
        std::string linea;
        std::string searchWord = "C++";
        std::string replaceWord = "CPP";
        std::cout << "Busqueda la palabra:" << searchWord << std::endl;
        std::cout << "Reemplazar la palabra:" << replaceWord << std::endl;
        // Display the content of the input file before find and replace
        std::cout << "\nAntes de encontrar y reemplazar :" << std::endl;
        displayFileContent("prueba.txt");
        std::cout << std::endl;
    
        while (std::getline(inputFile, linea)) {
          size_t pos = linea.find(searchWord);     
          while (pos != std::string::npos) {
            linea.replace(pos, searchWord.length(), replaceWord);
            pos = linea.find(searchWord, pos + replaceWord.length());
          }
          outputFile << linea << "\n";
        }
    
        inputFile.close();
        outputFile.close();
    
        std::cout << "Despues de encontrar y reemplazar:" << std::endl;
        displayFileContent("nueva_prueba.txt");
    
        std::cout << "\nPalabra reeemplazada correctamente." << std::endl;
      } else {
        std::cout << "\nFallo la apertura de los archivos." << std::endl;
      }
    
      return 0;
    }


En el ejercicio anterior :

Se define una función ``displayFileContent`` que toma un nombre de
archivo como argumento. Esta función abre el archivo especificado, lee
su contenido línea por línea y muestra cada línea en la consola.

Dentro de la función ``main()``, abre un archivo de entrada llamado
``prueba.txt`` usando ``std::ifstream`` y un archivo de salida llamado
``nueva_prueba.txt`` usando ``std::ofstream``. Comprueba si ambos
archivos se abrieron con éxito utilizando la función ``is_open``. Llama
a la función ``displayFileContent`` para mostrar el contenido del
archivo de entrada antes de la operación de búsqueda y reemplazo. Entra
en un ciclo while para leer cada línea del archivo de entrada usando
``std::getline``.

Dentro del bucle, se utiliza la función de búsqueda ``searchWord`` para
buscar la palabra dentro de la línea. Si encuentra una coincidencia,
ingresa a otro ciclo para reemplazar todas las apariciones de la palabra
de búsqueda con ``replaceWord`` la palabra de reemplazo usando la
función ``replace``.

Después de modificar la línea, escribe la línea modificada en el archivo
de salida utilizando el operador de flujo de salida ``<<``. Una vez que
finaliza el ciclo, cierra los archivos de entrada y salida.

Vuelve a llamar a la función ``displayFileContent`` para mostrar el
contenido del archivo de salida después de la operación de búsqueda y
reemplazo.

Finalmente, imprime un mensaje indicando que la palabra fue reemplazada
con éxito.

Si el contenido del archivo ``entrada.txt`` es:

::

     Hoy temprano estuve en Plaza Vea comprando fruta para la casa. 
     De retorno a casa tuve la última clase de Fundamentos de Programación. 
     Y aunque no tenía ganas de salir tuve que ir a la panadería a comprar pan para la casa. 

La salida es:

Ingrese la palabra a buscar: casa

Número de ocurrencias: 3

Escribe un programa en C++, que lea el archivo de texto ``entrada.txt``
y cuenta las ocurrencias en el archivo de una palabra especificada
ingresada durante la ejecución del programa.

.. code:: ipython3

    #include <fstream>                      // ifstream, ofstream
    #include <cassert>                      // assert()
    #include <iostream>
    #include <cstring>
    using namespace std;
    
    int obtener_ocurrencias(char* cad1, char* cad2);
    
    int main() {
        char bufer_de_lectura[255];         // memoria temporal para la lectura del archivo
        char palabra[20];                    // cadena a buscar en el archivo
        int ocurrencias = 0;
    
        cout << "Ingrese la palabra a buscar: ";
        cin >> palabra;
    
        ifstream fin("entrada.txt");       // estableciendo coneccion con el disco duro,
        assert( fin.is_open() );            // y verificando si la operacion se realizo satisfactoriamente
    
        while (fin.getline(bufer_de_lectura,255)) {
            ocurrencias += obtener_ocurrencias(bufer_de_lectura, palabra);
        }
        cout << "Numero de ocurrencias: " << ocurrencias << endl;
        fin.close();                        // cerrando la conexion con el disco duro
    
        return 0;
    }
    
    // contador de ocurrencias de cad2 en cad1
    int obtener_ocurrencias(char* cad1, char* cad2) {
        int M = strlen(cad1);
        int N = strlen(cad2);
        int contador = 0;
        int k = 0;
        for (int j = 0; j <= M - N; ++j) {
            // verificacion para el indice actual j
            for (k = 0; k < N; ++k)
                if (cad1[j+k] != cad2[k]) break;
    
            if (k == N) ++contador; // si cad1[j, j+1, ...j+M-1] = cad2[0...M-1]
        }
        return contador;
    }

Escribe un programa en C++ para agregar nuevos datos a un archivo de
texto existente.

.. code:: ipython3

    #include <iostream>
    #include <fstream>
    #include <string>
    
    // funcion para mostrar el contenido de un archivo
    void displayFileContent(const std::string & filename) {
      std::ifstream file(filename);
      std::string linea;
    
      if (file.is_open()) {
        std::cout << "Contenido de archivo:" << std::endl;
        while (std::getline(file, linea)) {
          std::cout << linea << std::endl;
        }
        file.close();
      } else {
        std::cout << " Falla en abrir el archivo. " << std::endl;
      }
    }
    
    int main() {
    
      displayFileContent("nueva_prueba.txt");
      std::cout << std::endl;
      std::ofstream outputFile;
      // abrir el archivo en modo append
      outputFile.open("nueva_prueba.txt", std::ios::app);
      displayFileContent("nueva_prueba.txt");
      std::cout << std::endl;
      if (outputFile.is_open()) {
        std::string newData;
    
        std::cout << "Ingresa la data para agregar: ";
        // Leer la data para el archivo
        std::getline(std::cin, newData);
        // Agregar los datos en el archivo
        outputFile << newData << std::endl;
        outputFile.close();
        std::cout << "Data agregada correctamente." << std::endl;
        displayFileContent("nueva_prueba.txt");
        std::cout << std::endl;
    
      } else {
        std::cout << "Falla en abrir el archivo." << std::endl;
      }
    
      return 0;
    }


En el ejercicio anterior:

Se utiliza la ``clase std::ofstream`` para abrir el archivo en modo
``append``. El indicador ``std::ios::app`` se pasa como segundo
argumento a la función ``open()``, que garantiza que se agreguen nuevos
datos al contenido del archivo existente. Se comprueba si el archivo se
abrió correctamente con la función ``is_open()``. Si el archivo está
abierto, solicita al usuario que ingrese los datos que desea agregar al
archivo. La función ``std::getline()`` lee nuevos datos del usuario.
Luego usa el operador de flujo de salida ``<<`` para agregar los nuevos
datos al archivo.

El ``std::endl`` inserta un carácter de nueva línea después de los
nuevos datos. Después de agregar los datos, se cierra el archivo con
``outputFile.close()`` y muestra un mensaje.

El contenido de un archivo se muestra mediante
``displayFileContent(archivo.txt)``.

Escribe un programa en C++ para leer un archivo CSV y mostrar su
contenido en forma tabular.

.. code:: ipython3

    #include <iostream>
    #include <fstream>
    #include <string>
    #include <vector>
    #include <sstream>
    
    // Funcion para dividir una cadena en tokens según un delimitador
    
    std::vector < std::string > splitString(const std::string & str, char delimiter) {
      std::vector < std::string > tokens;
      std::stringstream ss(str);
      std::string token;
    
      while (std::getline(ss, token, delimiter)) {
        tokens.push_back(token);
      }
    
      return tokens;
    }
    
    // Funcion para mostrar el contenido de un archivo CSV en forma tabular
    
    void displayCSVContents(const std::string & filename) {
      std::ifstream file(filename);
      std::string linea;
    
      if (file.is_open()) {
        while (std::getline(file, linea)) {
          std::vector < std::string > tokens = splitString(linea, ',');
          for (const std::string & token: tokens) {
            std::cout << token << "\t";
          }
          std::cout << std::endl;
        }
    
        file.close();
      } else {
        std::cout << "Falla en abrir un archivo." << std::endl;
      }
    }
    
    int main() {
      std::string filename = "data.csv"; // archivo csv para leer
    
      displayCSVContents(filename);
    
      return 0;
    }


En el ejercicio anterior:

La función ``splitString()`` toma una cadena y un carácter delimitador
(``delimiter``) y divide la cadena en tokens según el delimitador.
Devuelve un vector de cadenas que representan tokens. La función
``displayCSVContents()`` toma un nombre de archivo y lee el archivo CSV
línea por línea usando ``std::ifstream`` y ``std::getline``.

Para cada línea, utiliza la función ``splitString`` para dividir la
línea en tokens según el delimitador de coma ``(,)``. Luego recorre los
tokens y los muestra usando ``std::cout``. Cada token está separado por
un carácter de tabulación ``(\t)``, lo que crea un formulario tabular.

Si deseas obtener una estadística del archivo de caracteres
(``archivo txt``). Escribe un programa en C++ para contar el número de
palabras de que consta el archivo, así como una estadística de cada
longitud de palabra. Se sabe que la longitud de cada palabra no es mayor
a 30 caracteres y que el texto no contenga signos de puntuación o
acentos.

Si el contenido del archivo ``archivo.txt`` es:

::

    Hoy temprano estuve en Plaza Vea comprando fruta para la casa 
    De retorno a casa tuve la última clase de Fundamentos de Programación 

La salida es:

::

   Número de palabras del texto: 23 
   Número de palabras con 1 caracteres de longitud: 1 
   Número de palabras con 2 caracteres de longitud: 6 
   Número de palabras con 3 caracteres de longitud: 2 
   Número de palabras con 4 caracteres de longitud: 4 
   Número de palabras con 5 caracteres de longitud: 3 
   Número de palabras con 6 caracteres de longitud: 2 

.. code:: ipython3

    #include <iostream>
    #include <fstream>
    #include <cstring>
    
    using namespace std;
    
    int main () {
        ifstream file("archivo.txt");
        if (!file.good()) {
            cout << "Error, no se pudo abrir el archivo!" << endl;
            return 1;
        }
        char word[31];
        int len_max, len;
        
        len_max = 0;
        int cont = 0;
        while( file >> word ) {
            len = strlen(word);
            if (len > len_max)
                len_max = len;
            cont++;
        }
        file.clear();
        file.seekg(0);
        
        int *statist = new int[len_max + 1];
        
        for(int i = 0; i <= len_max; i++)
            statist[i] = 0; 
        
        while(file >> word) {
            statist[strlen(word)]++;
        }
        
        cout << "Numero de palabras del texto: " << cont << endl;
        
        for(int i = 0; i <= len_max; i++)
            if (statist[i] > 0)
                cout << "Numero de palabras con " << i << " caracteres de longitud: " << statist[i] << endl; 
            
        file.close();
        return 0;
    }

Explica el resultado del siguiente código:

.. code:: ipython3

    #include <fstream> 
    #include <iostream> 
    #include <string> 
    
    int main() 
    { 
        std::ifstream inf{ "Muestra.txt" }; 
        if (!inf) 
        { 
            std::cerr << "Muestra.txt no puede ser abierto para lectura !\n"; 
    
            return 1; 
        } 
        std::string strData; 
    
        inf.seekg(5);     
        std::getline(inf, strData); 
        std::cout << strData << '\n'; 
    
        inf.seekg(8, std::ios::cur);      
        std::getline(inf, strData); 
        std::cout << strData << '\n'; 
    
        inf.seekg(-14, std::ios::end);     
        std::getline(inf, strData); 
        std::cout << strData << '\n'; 
    
        return 0; 
    
    } 

El acceso aleatorio al archivo se realiza manipulando el puntero del
archivo utilizando la función ``seekg()`` (para entrada) y la función
``seekp()`` (para salida). En caso de que te lo estés preguntando, la
``g`` significa ``get`` y la ``p`` ``put``.

Para algunos tipos de secuencias, ``seekg()`` (cambiar la posición de
lectura) y ``seekp()`` (cambiar la posición de escritura) funcionan de
forma independiente, sin embargo, con las secuencias de archivos, las
posiciones de lectura y escritura son siempre idénticas, por lo que
``seekg`` y ``seekp`` pueden ser usado indistintamente.

Las funciones ``seekg()`` y ``seekp()`` toman dos parámetros. El primer
parámetro es un desplazamiento que determina cuántos ``bytes`` mover el
puntero del archivo. El segundo parámetro es un indicador de ios que
especifica como se desaplaza el parámetro ``offset``.

-  ``beg`` el desplazamiento es relativo al comienzo del archivo
   (predeterminado)
-  ``cur`` el desplazamiento es relativo a la ubicación actual del
   puntero del archivo
-  ``end`` el desplazamiento es relativo al final del archivo

.. code:: ipython3

    inf.seekg(14, std::ios::cur); // avanzar 14 bytes 
    inf.seekg(-18, std::ios::cur); //mover hacia atras 18 bytes 
    inf.seekg(22, std::ios::beg); //pasar al byte 22 en el archivo
    
    inf.seekg(24); //pasar al byte 24 en el archivo
    
    inf.seekg(-28, std::ios::end); //pasar al byte 28 antes del final del archivo


Mover al principio o al final del archivo es fácil:

.. code:: ipython3

    inf.seekg(0, std::ios::beg); // mover al principio del archivo
    inf.seekg(0, std::ios::end); // mover al final del archivo

Escribe un programa que escriba la segunda palabra (y su longitud) en un
arreglo de caracteres. Las palabras están separadas por espacios,
puntuación y tabulaciones.

.. code:: ipython3

    #include<iostream>
    bool isSeparator(char c, char* separators) {
        char endOfString = '\0';
        char* sepPtr = separators;
        bool ret = false;
        while(*sepPtr != endOfString) {
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
        char endOfString = '\0';
        while(*a != endOfString) {
            ret++;
            a++;
        }
        return ret;
    }
    int main(int argc, char** argv) {
        using namespace std;
        int ret = 0;
        
        char endOfString = '\0';
        char theSentence[] = "Kapu es a detective in Motita Club";
        char separators[] = " \t,.;:?!";
        char* strPtr = theSentence;
        char* strEnd;
    
        // find the beginning of the second word
        bool isSecondWord = false;
        while(*strPtr != endOfString) {
            while(isSeparator(*strPtr, separators)) {
            // while trata con mayusculas y minusculas con separadores consecutivos
                strPtr++;
                isSecondWord = true;
            }
            if (isSecondWord) {
            // fin de los separadores consecutivos, significa que la segunda palabra comienza en strPtr
                break;
           } else {
           // el caracter no era un separador, continuar
             strPtr++;
        }
    }
    
    strEnd = strPtr;
    while(*strEnd != endOfString) {
        if (isSeparator(*strEnd, separators)) {
            // primer separador encontrado despues de la segunda palabra, marca el final
            *strEnd = endOfString;
            break;
        }
        strEnd++;
    }
        // imprimir la segunda palabra y su longitud
    cout << strPtr << "(" << charArrayLength(strPtr) << ")" << endl;
        return ret;
    }

Escribe un programa que lea un archivo de texto e imprima la suma de los
números que se encuentran en la segunda columna (una palabra está en la
segunda columna de un archivo si su primer carácter es el primer
carácter que no es un espacio después del primer carácter de espacio en
el línea).

.. code:: ipython3

    #include <iostream>
    #include <fstream>
    #include <cstdlib>
    #include <cstring>
    
    
    const int BUFSIZE = 1024;
    const int ARRAYSIZE = 50;
    int main(int argc, char** argv) {
        using namespace std;
        int ret = 0;
        if (argc < 2) {
          cerr << argv[0] << ": archivo en la linea cmd" << endl;
          exit(1);
        }
        
        char buffer[BUFSIZE];
    
        char* startPtr;
        char* endPtr;
        
        char myArray[ARRAYSIZE][ARRAYSIZE][BUFSIZE];
    
        for(int i = 0; i < ARRAYSIZE; i++) {
         for(int j = 0; j < ARRAYSIZE; j++) {
            *(myArray[i][j]) = '\0';
            }
        }
    
        ifstream myStream(argv[1]);
        
        bool isEndLine = false;
      
        bool isSpace = false;
        
        int row = 0;
        int col = 0;
        int maxRow = 0;
        int maxCol = 0;
        
        while(!myStream.eof()) {
            myStream.getline(buffer, (streamsize) BUFSIZE-1);
            startPtr = buffer;
            endPtr = startPtr;
    
            isEndLine = false;
            while(!isEndLine) {
                endPtr++;
                isSpace = false;
                if (*endPtr == ' ') {
       *endPtr = '\0';
       isSpace = true;
           }
            if (*endPtr == '\0') {
       
        isEndLine = true;
        strncpy(myArray[row][col], startPtr, BUFSIZE-1);
        col++;
            }
            if (isSpace) {
        *endPtr = ' ';
        
        while(*endPtr == ' ') {
            endPtr++;
        }
        startPtr = endPtr;
        isEndLine = false;
            }
          }
          if (col > maxCol) {
            maxCol = col;
          }
          row++;
          col = 0;
        }
        maxRow = row;
        double sum = 0;
        for(int i = 0; i < maxRow; i++) {
           sum += atof(myArray[i][1]);
        }
        cout << "suma = " << sum << endl;
    
      #ifdef DEBUG
        cout << "maxFila = " << maxRow << ", maxCol = " << maxCol << endl;
        for(int i = 0; i < maxRow; i++) {
            for(int j = 0; j < maxCol; j++) {
               cout << myArray[i][j] << " ";
            }
            cout << endl;
       }
        #endif
         return ret;
    }          

Escribe un programa que imprima su propio tamaño de archivo (ejecutable)
en bytes y en número de caracteres imprimibles (un carácter char c es
imprimible si ``(isalnum(c) > 0 || ispunct(c) > 0 || isspace(c) > 0))``.

.. code:: ipython3

    #include<iostream>
    #include<fstream>
    int main(int argc, char** argv) {
    
     using namespace std;
    
     ifstream ifs(argv[0]);
     char c;
     int characters = 0;
     int printables = 0;
     while(true) {
         ifs.get(c);
         if (ifs.eof()) {
           break;
       }
        characters++;
        if (isalnum(c) > 0 || ispunct(c) > 0 || isspace(c) > 0) {
        printables++;
         }
       }
    
        ifs.close();
        cout << "Tam archivos: bytes = " << characters << ", caracteres imprimibles= "
            << printables << endl;
        return 0;
    }

Una sonda está calibrada para pruebas de analogía minera en la
superficie del planeta Marte. Una forma eficiente de recorrer el área es
realizar una espiral cuadrada. Una espiral cuadrada que parte del origen
de coordenadas ``(0,0)`` y se desplaza consecutivamente a través de los
puntos:

::

   (1,0), (1,1), (0,1), (-1,1), (-1,0), (-1,-1), (0,-1), (1,-1), (2,-1), 

   (2,0), (2,1), (2,2), (1,2), (0,2), etc. 

Una coordenada está en el paso n, si ocupa la posición n en la lista de
puntos visitados por la espiral.

El origen ``(0,0)`` es la posición cero ``(0)``, el ``(1,0)`` es la
posición uno ``(1)``, etc.

La tarea a realizar es desarrollar un programa en C++ que dado un valor
de ``n``, determina cuáles son las coordenadas ``x`` e y del punto
visitado en el paso n, para ``1 < n < 1000``.

El archivo debe contener dos enteros separados los espacios que indican
las coordenadas ``x`` e ``y`` del punto en el paso ``n``.

Entrada por teclado: Ingrese el valor de n: 69

Salida (salida.txt) -4 -1

.. code:: ipython3

    #include <iostream>
    #include <fstream>
    #include <cmath>
    using namespace std;
    
    void espiral(int N)
      {
      ofstream salida;   
      int par,cuadrado,impar,i,Y,X;
      
      i = sqrt(N);
      if( i%2 == 1 )
        {
        impar = i;
        par = i + 1;
        }
      else
        {
        impar = i + 1;
        par = i;
        }
    
      cuadrado = impar*impar;
    
      if( cuadrado - impar <= N  && N <= cuadrado  )
        {
        X = cuadrado - N;
        X = (impar+1)/2 - X;
        Y = -(impar-1)/2;
        }
      else if( cuadrado < N  && N <= cuadrado + impar  )
        {
        X = (impar+1)/2;
        Y = N - cuadrado;
        Y = -(impar-1)/2 + Y;
        }
    
      cuadrado = par*par;
    
      if( cuadrado - par < N  && N <= cuadrado )
        {
        X = cuadrado - N;
        X = - (par/2) + X;
        Y = par/2;
        }
      else if( cuadrado < N  && N < cuadrado + par  )
        {
        X = -(par/2);
        Y = N - cuadrado;
        Y = (par/2) - Y ;
        }
      salida.open("salida.txt", ios ::out);
      salida<<X<<" "<<Y<<endl;
      salida.close();
    }
    
    int main()
      {
      int N;
      cout<<" Ingrese el valor de N: ";      
      cin>>N;
      espiral(N);
      return 0;
    }

Desarrolla código en C++ que compare dos archivos de texto:
``entrada1.txt`` e ``entrada2.txt``. Se deben mostrar las diferencias
entre los archivos, precedidas del número de línea y de columna.

Considera que los dos archivos tienen el mismo número de líneas y que
cada línea no contiene más de 100 caracteres

.. code:: ipython3

    #include <iostream>
    #include <cstring>
    #include <fstream>
    
    using namespace std;
    
    int main() {
        ifstream i1("entrada1.txt", ios::in);
        ifstream i2("entrada2.txt", ios::in);
        
        if ( !i1.good() || !i2.good() ) {
            cout << "Error abriendo los archivos" << endl;
            return 1;
        }
        
        char linea1[100], linea2[100];
        int sz1, sz2;
        int linea = 0;
        while(i1.getline(linea1, 100)) {
            int search = 0;
            if(i2.getline(linea2, 100)) {
                if (strcmp(linea1, linea2)) {
                    sz1 = strlen(linea1);
                    sz2 = strlen(linea2);
                    int colum = -1;
                    for(int i = 0; i < sz1 && i < sz2 && colum == -1; i++) {
                        if (linea1[i] != linea2[i]) colum = i;
                    }
                    if (colum == -1) {
                        colum = sz1;
                        if (sz1 >= sz2) colum = sz2;
                    }
                    cout << "Diferencia en la linea y columna: (" << linea << ", " << colum << ")" << endl;
                }
                search = 1;
            }
            if (!search) {
                cout << "Diferencia en la linea y columna: (" << linea << ", 0)"<<endl;
            } 
            linea++;    
        }
        return 0;
    }

¿Cómo se logra el enlazado dinámico en tu sistema?. ¿Qué restricciones
se imponen al código enlazado dinámicamente? ¿Qué requisitos se imponen
al código para que se enlace dinámicamente?

Cuando se trata de crear aplicaciones de software, una de las decisiones
clave que deben tomar los desarrolladores es si utilizar enlazados
estáticos o dinámicos.

Ambos enfoques tienen sus propias ventajas y desventajas según los
requisitos del proyecto.

**Enlace estático**

El enlazado estático compila un programa y sus librerías necesarias en
un archivo ejecutable. Cuando alguien ejecuta este programa en su
computadora o servidor, no necesita instalar ningún software especial.

Una ventaja del enlace estático es la fácil distribución, ya que solo
tiene un archivo para administrar en lugar de múltiples dependencias.
Sin embargo, actualizar una librería utilizada por muchos programas
requiere volver a compilar cada uno individualmente.

**Enlazado dinámico**

El enlazado dinámico almacena librerías por separado del archivo
ejecutable principal en tiempo de ejecución cuando es necesario.

Esto significa que si es necesario realizar una actualización en una
librería utilizada por muchos programas, solo esa librería necesita
actualizarse en lugar de que cada programa individual que la use
necesite una recompilación como con el enlazado estática.

En términos de rendimiento, las aplicaciones enlazadas dinámicamente
utilizan menos memoria porque las librerías compartidas se cargan una
vez por sistema, independientemente de cuántas veces se las solicite,
mientras que las aplicaciones enlazadas estáticamente cargan todos los
recursos necesarios por adelantado, lo que genera un mayor uso de
memoria en general, pero un tiempo de ejecución más rápido debido a una
menor cantidad de recursos.

Los enlaces dinámicos son mejores para actualizar librerías compartidas,
mientras que los enlaces estáticos son mejores para facilitar la
distribución.

