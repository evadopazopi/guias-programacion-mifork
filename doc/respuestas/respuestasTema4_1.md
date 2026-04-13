<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

En el lenguaje C, la composición de estructuras se basa en la inclusión de un `struct` previamente definido como un campo dentro de otro más complejo. Esta técnica permite organizar la información de manera jerárquica, reflejando relaciones de pertenencia donde una entidad mayor "tiene" componentes menores. Al emplear este enfoque, se favorece la modularidad, ya que las funciones diseñadas para las estructuras internas pueden ser reutilizadas por las estructuras de nivel superior.

Para representar una línea mediante composición, primero se define una estructura básica para la unidad mínima, en este caso el punto. Posteriormente, la estructura de la línea se construye declarando dos variables del tipo punto en su interior. Esta jerarquía facilita el acceso a las coordenadas individuales a través de un doble nivel de direccionamiento (por ejemplo, `linea.puntoA.x`), manteniendo la lógica de cada entidad separada y clara.

A continuación se presenta la implementación técnica de este concepto:

```c
#include <stdio.h>
#include <math.h>

// Estructura componente (Punto)
typedef struct {
    float x;
    float y;
} Punto;

// Estructura compuesta (Linea "tiene-dos" Puntos)
typedef struct {
    Punto p1;
    Punto p2;
} Linea;

// Función para calcular la distancia entre dos puntos individuales
float calcularDistanciaPuntos(Punto a, Punto b) {
    return sqrtf(powf(b.x - a.x, 2) + powf(b.y - a.y, 2));
}

// Función para hallar la longitud de una línea usando la función anterior
float calcularLongitudLinea(Linea l) {
    // Se delega la lógica de cálculo a la función de puntos
    return calcularDistanciaPuntos(l.p1, l.p2);
}
```

En el código expuesto, se observa cómo la función `calcularLongitudLinea` no necesita conocer los detalles internos de las coordenadas `x` e `y`. Simplemente extrae los componentes `p1` y `p2` de la estructura `Linea` y los delega a la función especializada en puntos. Esta práctica es fundamental para evitar la duplicación de lógica y es el antecedente directo de cómo se gestionarán las relaciones entre objetos en lenguajes de mayor nivel como Java.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

La transición de estructuras en C a clases en Java permite elevar el concepto de composición mediante el uso de modificadores de acceso y constructores. En este modelo, la clase `Punto` actúa como el bloque de construcción fundamental, encapsulando sus coordenadas privadas. Al definir los campos como `final` y no proporcionar métodos de modificación (*setters*), se garantiza la **inmutabilidad** del objeto, asegurando que su estado no cambie tras la instanciación, lo cual previene efectos secundarios no deseados en el resto del programa.

La clase `Linea` materializa la relación "tiene-un" al declarar dos atributos de tipo `Punto`. Al igual que en la clase anterior, el uso de la palabra clave `final` en los atributos de la línea asegura que la conexión entre la línea y sus puntos extremos sea permanente. Este diseño no solo organiza los datos, sino que permite que la clase `Linea` delegue la responsabilidad del cálculo matemático a la lógica interna de los objetos `Punto`, promoviendo una estructura donde cada clase resuelve su propia especialidad.

A continuación se presenta la implementación de este modelo en Java:

```java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Calcula la distancia desde este punto a otro punto recibido por parámetro
    public double calcularDistancia(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
    }

    // Getters necesarios para consultar la información
    public double getX() { return x; }
    public double getY() { return y; }
}

public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    // La longitud de la línea es la distancia entre sus puntos componentes
    public double calcularLongitud() {
        return p1.calcularDistancia(p2);
    }
}
```



Desde la perspectiva de la ocultación de información, este enfoque supera significativamente a las estructuras de C. Mientras que en C cualquier parte del código con acceso al `struct` podría alterar las coordenadas accidentalmente, en Java el acceso externo está restringido. El método `calcularLongitud` de la clase `Linea` resulta extremadamente limpio, ya que simplemente solicita al objeto `p1` que calcule la distancia hasta `p2`, demostrando cómo los objetos colaboran entre sí enviándose mensajes sin exponer su estado interno.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La **multiplicidad** es un concepto fundamental en el diseño de sistemas que define cuántas instancias de una clase pueden estar asociadas con una instancia de otra clase en un momento dado. En el contexto de la composición, este valor determina los límites inferiores y superiores de la relación, ayudando a establecer reglas de negocio claras, como por ejemplo, si un objeto puede existir sin sus componentes o cuál es el número máximo de elementos que puede contener una colección.

En el ejemplo técnico de la geometría analítica, la multiplicidad se lee de forma bidireccional para comprender totalmente el vínculo entre las entidades. De **Línea a Punto**, la multiplicidad es exactamente **2**. Esto se debe a que, bajo la definición lógica del programa, una línea requiere estrictamente un punto de origen y un punto de destino para ser considerada como tal; no puede tener ni uno solo ni tres.

Por otro lado, de **Punto a Línea**, la multiplicidad suele definirse como **0..*** (cero a muchos). Un punto, como entidad matemática independiente, puede existir sin formar parte de ninguna línea, pero también puede ser el extremo de una o de múltiples líneas simultáneamente dentro del espacio de memoria del programa.



El control de estas cantidades en Java se garantiza mediante el **constructor** y la **encapsulación**. Mientras que en la dirección de la línea se asegura la presencia de los dos puntos al instanciar el objeto, en la dirección opuesta la relación es más flexible, ya que el objeto `Punto` no guarda referencias internas hacia las líneas que lo utilizan, manteniendo así un bajo acoplamiento entre las clases.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La distinción entre composición fuerte y débil radica en el grado de dependencia y propiedad que existe entre el objeto contenedor (el "todo") y los objetos contenidos (las "partes"). En la **composición fuerte**, comúnmente llamada simplemente **composición**, existe una relación de pertenencia vital y exclusiva. Esto implica que las partes no tienen sentido de existencia por sí mismas fuera del contenedor; por ejemplo, una habitación no puede existir de forma independiente si el edificio que la contiene es demolido.

Por el contrario, la **composición débil** se identifica con los términos **agregación** o **asociación**. En este escenario, aunque un objeto "posee" a otros, estos mantienen una independencia lógica y pueden sobrevivir si el objeto principal desaparece. Un ejemplo claro sería un profesor perteneciente a un departamento universitario: si el departamento se cierra, el profesor sigue existiendo como entidad independiente y puede vincularse a otra institución.



En términos de **ciclo de vida**, estas diferencias determinan cuándo se destruyen los objetos en memoria. En la composición fuerte, el ciclo de vida de los componentes está rígidamente ligado al del contenedor: cuando el objeto "todo" es eliminado por el recolector de basura (*Garbage Collector* en Java) o destruido, sus partes mueren con él. Esto garantiza que no queden objetos huérfanos que no tienen una función lógica sin su dueño.

En la composición débil o agregación, el ciclo de vida es independiente. El objeto contenedor solo guarda una referencia o puntero hacia los componentes, pero no es responsable de su creación ni de su destrucción final. Esta separación permite que las partes sean compartidas por múltiples contenedores o que sigan existiendo en el sistema tras la eliminación del objeto que las agrupaba temporalmente.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

En estos escenarios, el término técnico adecuado es **dependencia** (también conocida como relación de "uso"). A diferencia de la composición, donde una clase mantiene una referencia permanente a otra como parte de su estado (un atributo), la dependencia es una relación transitoria y puntual. Se produce cuando una clase requiere de otra para llevar a cabo una operación específica, pero no existe un vínculo de propiedad o pertenencia duradero entre ambas.

Cuando una clase realiza un `new` dentro de un método o utiliza un objeto como variable local, el objeto creado tiene un ciclo de vida limitado estrictamente a la ejecución de dicho método. Una vez que la función finaliza, la referencia local desaparece y el objeto queda marcado para la recolección de basura. En este caso, la clase "depende" de la existencia de la otra para completar su tarea, pero no la "tiene" como un componente estructural.



Del mismo modo, al recibir o devolver objetos como parámetros, se establece un contrato de colaboración. Por ejemplo, en un sistema de gestión, un método `imprimirFactura(Cliente c)` depende de la clase `Cliente` para obtener los datos de envío, pero la factura no "compone" al cliente; simplemente lo utiliza durante el proceso de impresión. La dependencia es la forma más débil de relación en la orientación a objetos, ya que cualquier cambio en la interfaz de la clase proveedora afectará a la clase dependiente, aunque no compartan una estructura común.

En resumen, la distinción principal reside en la **permanencia**. Mientras que la composición define qué **es** o qué **tiene** un objeto (relación estructural), la dependencia define qué **hace** o qué **necesita** un objeto para funcionar (relación de comportamiento). En Java, las dependencias suelen identificarse porque la clase externa no aparece declarada en la sección de atributos, sino únicamente dentro de la firma o el cuerpo de los métodos.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

Para implementar una **composición fuerte**, la clase contenedora debe asumir la responsabilidad total de la creación de sus componentes. En este modelo, los objetos `Punto` se instancian directamente dentro del constructor de la clase `Linea` utilizando los datos primitivos (como `double x, y`) recibidos por parámetro. De esta forma, el mundo exterior no tiene acceso a las referencias originales de los puntos, y si el objeto `Linea` es destruido por el recolector de basura, sus puntos internos desaparecerán con él, ya que no existen otras referencias a ellos fuera de la línea.

En la **composición débil** (o agregación), la clase `Linea` recibe objetos `Punto` ya creados desde el exterior a través de su constructor. En este escenario, la línea simplemente almacena una referencia a objetos que tienen su propia existencia independiente. Si la instancia de `Linea` se elimina, los objetos `Punto` permanecen intactos en la memoria del programa, pues probablemente fueron creados en otra parte del código (como el método `main`) y podrían estar siendo utilizados por otras clases simultáneamente.

A continuación se contrastan ambas implementaciones en Java:

```java
// OPCIÓN A: COMPOSICIÓN FUERTE
// La Línea "es dueña" de la creación y vida de sus puntos.
public class LineaFuerte {
    private final Punto p1;
    private final Punto p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        // La línea crea sus propios componentes internamente
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}

// OPCIÓN B: COMPOSICIÓN DÉBIL (AGREGACIÓN)
// La Línea solo recibe puntos que ya existen fuera.
public class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    public LineaDebil(Punto p1, Punto p2) {
        // La línea solo guarda la referencia a puntos externos
        this.p1 = p1;
        this.p2 = p2;
    }
}
```



La elección entre un modelo u otro depende de la lógica de negocio. La composición fuerte ofrece una mayor **encapsulación**, ya que impide que agentes externos modifiquen los componentes de la línea sin su consentimiento. Por el contrario, la composición débil aporta una mayor **flexibilidad**, permitiendo que un mismo punto sea compartido por varias líneas (como el vértice de un polígono), optimizando así el uso de la memoria al reutilizar instancias de objetos existentes.


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java, a diferencia de lenguajes como C++, la destrucción de objetos no se realiza de forma explícita mediante comandos como `free` o `delete`. El programador no tiene la responsabilidad de liberar la memoria manualmente, ya que esta tarea la gestiona automáticamente un componente de la Máquina Virtual de Java (JVM) llamado **Garbage Collector** (Recolector de Basura). Este sistema se encarga de identificar qué objetos ya no son accesibles para el programa y reclama su espacio en memoria para futuros usos.

En el caso de la **composición fuerte**, cuando una instancia de `Linea` deja de ser referenciada (por ejemplo, porque la variable local que la contenía sale de su ámbito), se vuelve elegible para ser destruida. Debido a que los objetos `Punto` fueron creados internamente y solo la `Linea` tiene una referencia hacia ellos, al desaparecer el contenedor, las partes pierden su único camino de acceso. El recolector de basura detecta que esos puntos han quedado aislados y procede a eliminarlos de forma encadenada.



Esta ausencia de destrucción explícita es una de las mayores ventajas en comparación con el manejo de punteros en C. Se evitan errores comunes como las fugas de memoria (*memory leaks*) o el acceso a memoria ya liberada (*dangling pointers*). El objeto contenedor "destruye" a sus componentes de manera implícita al ser el único que posee sus referencias; una vez que el dueño muere, el sistema de limpieza de Java entiende que los componentes ya no tienen propósito y los retira de la memoria.

Por tanto, aunque en el código no se observe una instrucción de borrado, la composición fuerte garantiza que el ciclo de vida de los puntos esté atado al de la línea. El programador solo debe preocuparse de dejar de usar el objeto principal; la infraestructura de Java se encarga de la limpieza profunda de toda la estructura compuesta, asegurando una gestión eficiente y segura de los recursos del sistema.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

La implementación de un departamento con sus profesores bajo el modelo de **composición débil** (agregación) requiere una gestión cuidadosa de las referencias para mantener la integridad de los datos. En este escenario, tanto el director como el resto de los docentes preexisten al departamento y pueden seguir existiendo si este se disuelve. La clase `Departamento` actúa como un contenedor que organiza estas relaciones, utilizando un array privado para ocultar los detalles de la implementación y exponiendo métodos que validan las reglas de negocio, como la obligatoriedad de que el director pertenezca a la nómina del centro.

Para garantizar la **encapsulación**, el departamento no permite el acceso directo al array de profesores. En su lugar, ofrece una interfaz controlada para añadir elementos al final de la lista o eliminarlos por índice. La lógica interna debe verificar constantemente las **invariantes de clase**: el director siempre debe estar presente en el listado de profesores. Si se intenta eliminar al profesor que actualmente ejerce como director o si se intenta asignar a alguien externo como jefe del departamento, el sistema debe lanzar una excepción para evitar un estado inconsistente en la memoria.

A continuación se detalla la implementación en Java siguiendo estas restricciones:

```java
public class Departamento {
    private String nombre;
    private Profesor director;
    private Profesor[] nomina;
    private int numProfesores;
    private final int MAX_PROFESORES = 50;

    public Departamento(String nombre, Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El departamento debe tener un director desde el inicio.");
        }
        this.nombre = nombre;
        this.nomina = new Profesor[MAX_PROFESORES];
        this.numProfesores = 0;
        
        // El director se añade automáticamente a la lista de profesores
        this.anyadirProfesor(directorInicial);
        this.director = directorInicial;
    }

    public void anyadirProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("No se puede añadir un profesor nulo.");
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("Capacidad máxima de profesores alcanzada.");
        }
        nomina[numProfesores++] = p;
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición de profesor inválida.");
        }
        // Invariante: No se puede eliminar al profesor que es el director actual
        if (nomina[posicion] == this.director) {
            throw new IllegalStateException("No se puede eliminar al profesor mientras sea el director.");
        }

        // Desplazamiento de elementos para mantener el array compacto (estilo C)
        for (int i = posicion; i < numProfesores - 1; i++) {
            nomina[i] = nomina[i + 1];
        }
        nomina[--numProfesores] = null; // Limpiar última referencia para el Garbage Collector
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        // Invariante: El nuevo director debe estar ya en la nómina del departamento
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (nomina[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }
        
        if (!encontrado) {
            throw new IllegalArgumentException("El nuevo director debe pertenecer primero al departamento.");
        }
        this.director = nuevoDirector;
    }

    public int getCantidadProfesores() { return numProfesores; }

    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida.");
        }
        return nomina[posicion];
    }
}
```



Este diseño demuestra cómo la orientación a objetos protege la lógica del mundo real. Aunque se utiliza un array primitivo similar a los de C, la clase `Departamento` actúa como un guardián de su propio estado. Al ocultar el array y el contador `numProfesores`, se asegura que nadie desde fuera pueda corromper la lista o dejar al departamento sin un director válido, transformando una simple estructura de datos en una entidad inteligente y segura.


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

Al emplear la interfaz `List` (concretamente su implementación `ArrayList`), se delega la gestión de la memoria y el redimensionamiento dinámico a la biblioteca estándar de Java. En este nuevo esquema, el programador se ahorra toda la lógica de **gestión de índices** y el **desplazamiento manual de elementos** que era necesario en C para mantener el array compacto tras una eliminación. Además, desaparece la necesidad de gestionar un contador `numProfesores` por separado y la restricción rígida de un tamaño máximo estático (como el 50 anterior), ya que la lista crece según sea necesario.

El uso de `List` simplifica drásticamente el mantenimiento del código, permitiendo que la clase se centre en validar las reglas de negocio en lugar de preocuparse por la infraestructura de datos. A continuación se muestra la versión actualizada del departamento:

```java
import java.util.ArrayList;
import java.util.List;

public class Departamento {
    private String nombre;
    private Profesor director;
    private List<Profesor> nomina;

    public Departamento(String nombre, Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Director requerido.");
        }
        this.nombre = nombre;
        this.nomina = new ArrayList<>();
        
        // El director se añade y se asigna
        this.anyadirProfesor(directorInicial);
        this.director = directorInicial;
    }

    public void anyadirProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("Profesor nulo.");
        nomina.add(p); // Java se encarga de la posición y el tamaño
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= nomina.size()) {
            throw new IndexOutOfBoundsException("Posición inválida.");
        }
        if (nomina.get(posicion) == this.director) {
            throw new IllegalStateException("No se puede eliminar al director.");
        }
        nomina.remove(posicion); // Java se encarga de compactar el hueco
    }

    // ... resto de métodos (cambiarDirector, getCantidad, etc.)
}
```



En cuanto al método para obtener todos los profesores, devolver directamente la referencia a la lista interna (`return nomina;`) supone una grave **violación de la encapsulación**. Si un objeto externo recibe la lista original, podría añadir o eliminar profesores saltándose todas las validaciones de la clase `Departamento`. Por ejemplo, alguien podría vaciar la lista o eliminar al director desde fuera sin que el departamento pudiera lanzar una excepción para impedirlo, rompiendo la integridad del sistema.

Para resolver este problema de seguridad, se debe aplicar una técnica de **protección de la información**. La solución estándar en Java consiste en devolver una **vista inmutable** de la lista (utilizando `Collections.unmodifiableList(nomina)`) o realizar una **copia defensiva** (devolviendo `new ArrayList<>(nomina)`). De este modo, el usuario externo puede consultar los datos, pero cualquier intento de modificar la estructura de la lista resultará en un error o no afectará a la lista privada que el departamento custodia internamente.

13/04/2026
Falta por ver:
-Composciones reflexivas o recursivas.
-Composiciones bidireccionales

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

La **composición recursiva** ocurre cuando una clase tiene un atributo cuyo tipo es la propia clase que se está definiendo. Este concepto permite construir estructuras de datos jerárquicas o en cadena de profundidad arbitraria, de forma muy similar a como se gestionan las listas enlazadas en C o las excepciones anidadas en Java. En este modelo, cada objeto actúa como un eslabón que "conoce" a su predecesor, permitiendo navegar por la genealogía o la estructura de datos hasta alcanzar un elemento base que no tiene más descendencia (referencia a `null`).

Al aplicar este concepto a una clase `Persona` inmutable, se asegura que el vínculo con la madre sea permanente desde el momento del nacimiento (la instanciación). La inmutabilidad garantiza que la historia familiar no pueda ser alterada accidentalmente una vez creada la estructura. El uso de `final` en el atributo `madre` obliga a que este se asigne en el constructor, reflejando fielmente la realidad biológica donde la ascendencia es un dato fijo.

A continuación se muestra la implementación y un ejemplo de uso:

```java
public class Persona{
    private Persona madre;

    public Persona (Persona madre){
        this.madre=madre;
    }
    public Persona (){
        this.madre = null;
    }
}

public class Main {
    public static void main(String[] args) {
        // Se construye la familia desde la raíz (abuela) hacia el nieto
        Persona abuela = new Persona("Carmen");
        Persona madre = new Persona("Elena", abuela);
        Persona nieto = new Persona("Lucas", madre);

        System.out.println("Linaje detectado:");
        System.out.println(nieto);
        System.out.println(nieto.getMadre());
        System.out.println(nieto.getMadre().getMadre());
    }
}
```



Además del ejemplo genealógico y las excepciones, existen otros casos clásicos de composiciones recursivas en la informática:

* **Sistemas de archivos:** Un objeto de tipo `Carpeta` que contiene una lista de objetos de tipo `Carpeta` (subcarpetas).
* **Estructuras de datos arbóreas:** Un `Nodo` de un árbol binario que tiene como atributos un `Nodo izquierdo` y un `Nodo derecho`.
* **Interfaces gráficas (GUI):** Un `Contenedor` (como un Panel) que puede contener otros `Contenedores` dentro de él para organizar la interfaz.
* **Patrón de diseño Composite:** Empleado para tratar objetos individuales y composiciones de objetos de manera uniforme, como ocurre en los procesadores de texto donde un documento está compuesto por secciones, que a su vez contienen párrafos.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

En una relación de **composición bidireccional**, ambos objetos involucrados mantienen una referencia mutua el uno del otro. Mientras que en una relación unidireccional solo el `Departamento` conoce a su lista de `Profesor`, en la bidireccional cada `Profesor` guarda también una referencia al `Departamento` al que pertenece. Esto permite "navegar" el modelo en ambos sentidos: desde el departamento se puede saber quiénes son sus docentes, y desde un objeto docente se puede consultar directamente a qué departamento está adscrito sin necesidad de realizar búsquedas externas.

Implementar este tipo de relación introduce una complejidad añadida: la **gestión de la consistencia**. Exigen programar cuidadosamente para mantener la consistencia. Si añado un profesor al departamento, debo actualizar la referencia al Departamento desde Profesor. Si esta sincronización falla, se producirían inconsistencias graves donde un profesor cree pertenecer a un departamento que no lo tiene en su nómina (un estado de "punteros colgados" lógico).

Para implementarlo en Java, se deben realizar los siguientes cambios estructurales:

1.  **Modificar la clase Profesor:** Se debe añadir un atributo privado de tipo `Departamento` y sus respectivos métodos de acceso (*getter* y *setter*).
2.  **Sincronizar en los métodos de gestión:** El método `anyadirProfesor(Profesor p)` del departamento no solo debe incluir al docente en su lista, sino también ejecutar `p.setDepartamento(this)`. 
3.  **Gestionar la desvinculación:** Al eliminar un profesor, se debe invocar `p.setDepartamento(null)` para romper el vínculo en ambas direcciones y permitir que el objeto quede libre para ser asociado a otro departamento o ser destruido.



A continuación se muestra un esquema de cómo se vería esta lógica en los métodos clave:

```java
public class Profesor {
    private String nombre;
    private Departamento departamento; // Referencia inversa

    public void setDepartamento(Departamento d) {
        this.departamento = d;
    }
    // ... getter
}

public class Departamento {
    // ... atributos previos (List<Profesor> nomina)

    public void anyadirProfesor(Profesor p) {
        if (p != null) {
            nomina.add(p);
            p.setDepartamento(this); // Sincronización bidireccional
        }
    }
}
```

Esta técnica es muy común en entornos donde los objetos necesitan conocer su contexto, como en los motores de bases de datos o frameworks como Hibernate. Sin embargo, requiere extrema precaución para evitar **referencias circulares** que compliquen la serialización de los objetos o la visualización de datos (como métodos `toString()` que entren en bucle infinito al intentar imprimirse mutuamente).
