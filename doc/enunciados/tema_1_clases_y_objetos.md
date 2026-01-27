<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

Las cuatro características fundamentales de la programación orientada a objetos (POO) son la abstracción, el encapsulamiento, la herencia y el polimorfismo. Estos pilares permiten pasar del modelo de funciones secuenciales de C a un sistema basado en entidades que interactúan entre sí de forma más robusta y escalable.

### 1. Abstracción

La abstracción consiste en identificar las características esenciales de un objeto y omitir los detalles irrelevantes para el contexto del problema. En lugar de lidiar con la complejidad total de un sistema, se crean modelos simplificados (clases) que solo contienen los datos y comportamientos necesarios. Por ejemplo, en un sistema de gestión académica, de una `Persona` solo interesan el nombre y las notas, ignorando otros detalles físicos como su altura o color de ojos.

### 2. Encapsulamiento

El encapsulamiento es el mecanismo que oculta los detalles internos de cómo funciona un objeto y protege su estado de accesos indebidos desde el exterior. Se logra definiendo los atributos como privados y proporcionando métodos públicos para interactuar con ellos. A diferencia de C, donde cualquier función puede modificar una `struct` si tiene el puntero, en Java el objeto "decide" qué datos muestra y cómo permite que se modifiquen, garantizando la integridad de la información.

### 3. Herencia

La herencia permite crear nuevas clases a partir de clases ya existentes, heredando sus atributos y métodos. Esto facilita la reutilización de código y la creación de jerarquías lógicas; por ejemplo, una clase `Estudiante` puede heredar de una clase `Persona`, obteniendo automáticamente campos como `nombre` y `edad` sin tener que redefinirlos. Es una evolución sobre la composición de `structs` en C, ya que establece una relación de tipo "es-un" (un Estudiante *es una* Persona).

### 4. Polimorfismo

El polimorfismo es la capacidad que tienen diferentes objetos de responder de manera distinta a un mismo mensaje o llamado de método. En términos prácticos, permite tratar a objetos de diferentes subclases como si fueran de una clase común, pero ejecutando el comportamiento específico de cada uno en tiempo de ejecución. Mientras que en C se necesitarían múltiples `if` o `switch` para gestionar diferentes tipos de datos, en Java el sistema identifica automáticamente qué versión del método debe ejecutarse según el objeto real.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Existen numerosos lenguajes que implementan la orientación a objetos, cada uno con un enfoque distinto según el área de aplicación. A continuación, se presentan cuatro de los más populares y relevantes en la industria actual:

### 1. Java

Java es probablemente el lenguaje más icónico de la POO. Fue diseñado para ser "portable", permitiendo que el código se ejecute en cualquier dispositivo gracias a la Máquina Virtual de Java (JVM). A diferencia de C, en Java casi todo debe estar dentro de una clase, lo que obliga al programador a seguir principios de orientación a objetos desde la primera línea de código. Es el estándar en aplicaciones empresariales bancarias y en el desarrollo nativo de Android.

### 2. Python

Aunque permite programar de forma secuencial como en C, Python es un lenguaje orientado a objetos por naturaleza (en Python, incluso los números y las cadenas son objetos). Es sumamente popular en inteligencia artificial y ciencia de datos debido a su sintaxis simplificada y legible. Su enfoque de la POO es más flexible y menos rígido que el de Java, lo que permite un desarrollo muy rápido y ágil.

### 3. C++

Este lenguaje es la evolución directa de C con soporte para clases (originalmente se llamó "C con clases"). Es un lenguaje híbrido, lo que significa que permite mezclar la programación estructurada que ya conoces con la orientación a objetos. Se utiliza principalmente en sistemas donde el rendimiento es crítico, como motores de videojuegos (Unreal Engine), navegadores web y software de edición de vídeo, ya que ofrece un control del hardware similar a C pero con las ventajas organizativas de los objetos.

### 4. C# (C-Sharp)

Desarrollado por Microsoft, es el lenguaje principal del ecosistema .NET y del motor de videojuegos Unity. Combina la potencia de C++ con la simplicidad de Java. Para un programador de C, C# resulta familiar en sintaxis, pero introduce conceptos modernos de gestión de memoria automática (Garbage Collector) y una estructura de clases muy robusta que facilita la creación de aplicaciones de escritorio y servicios web a gran escala.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

Para comprender la Programación Orientada a Objetos, es fundamental analizar los cimientos sobre los que se construye: los paradigmas que ya se manejan en el lenguaje C. Estos enfoques sentaron las bases para organizar el código de manera lógica y eficiente antes de la llegada de las clases.

### Programación Estructurada

La programación estructurada es un paradigma que se basa en la división de un programa en bloques lógicos utilizando únicamente tres estructuras de control: secuencia, selección (`if`, `switch`) e iteración (`for`, `while`). Este enfoque surgió para eliminar el uso de saltos incondicionales (como el `goto`), que hacían el código difícil de seguir. Se fundamenta en el teorema de la estructura, que demuestra que cualquier algoritmo puede ser representado mediante estas tres formas de control.

En la práctica de C, esto se traduce en una ejecución de arriba hacia abajo, donde el flujo es predecible y el código se organiza en funciones que realizan tareas específicas. Sin embargo, su principal limitación es que los datos y las funciones están separados; los datos suelen viajar de una función a otra como argumentos, lo que puede volverse inmanejable en sistemas de gran tamaño.

### Programación Modular

La programación modular da un paso más allá al proponer la división de un programa en partes independientes denominadas **módulos**. Cada módulo es un archivo o unidad de código que agrupa funciones relacionadas y que puede ser compilado o probado por separado. En el contexto de C, esto se visualiza claramente con el uso de archivos `.h` (cabeceras) y archivos `.c` (implementaciones), permitiendo que un programador use funciones de una librería sin necesidad de conocer su código interno.

Este paradigma introduce el concepto de "ocultación de información" a nivel de archivo. Un módulo puede tener funciones internas que no son visibles para el resto del programa, exponiendo solo una interfaz pública. Aunque la programación modular facilita enormemente el trabajo en equipo y la reutilización, sigue careciendo de la capacidad de los objetos para encapsular datos y comportamientos en una única entidad autorregulada.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

En la programación orientada a objetos, un objeto no es simplemente una colección de datos, sino una entidad integral definida por tres componentes esenciales: **estado**, **comportamiento** e **identidad**. Estos tres elementos permiten que el software modele objetos del mundo real de manera precisa y funcional.

### 1. El Estado

El estado está compuesto por los datos o valores que el objeto almacena en un momento determinado. En la práctica, se representa mediante los **atributos** (variables). Por ejemplo, si se tiene un objeto de la clase `Libro`, su estado vendría definido por valores como su título, el número de páginas o si se encuentra actualmente prestado o disponible.

A diferencia de las variables locales en C que desaparecen al terminar una función, el estado de un objeto persiste mientras el objeto exista en la memoria. Es importante notar que el estado de un objeto puede cambiar a lo largo del tiempo (por ejemplo, al marcar el libro como "prestado"), pero siempre bajo las reglas que defina su propia clase.

### 2. El Comportamiento

El comportamiento define las operaciones que el objeto puede realizar o las reacciones que tiene ante estímulos externos. Se implementa a través de los **métodos** (funciones internas). Siguiendo el ejemplo del `Libro`, su comportamiento incluiría acciones como `prestar()`, `devolver()` o `consultarInformacion()`.

El comportamiento suele estar estrechamente ligado al estado: un método puede consultar los atributos para tomar decisiones o modificarlos para cambiar el estado del objeto. Esta es la diferencia clave con C: en lugar de tener funciones globales que procesan datos, aquí el objeto es un "agente activo" que sabe cómo gestionarse a sí mismo.

### 3. La Identidad

La identidad es la propiedad que permite distinguir a un objeto de todos los demás, incluso si tienen exactamente el mismo estado (los mismos valores en sus atributos). En términos de memoria, la identidad suele estar asociada a la dirección de memoria única donde reside el objeto.

Para un programador de C, esto es similar a tener dos estructuras de tipo `Alumno` con el mismo nombre y edad, pero ubicadas en punteros diferentes. Aunque visualmente parezcan iguales, son dos entidades distintas. En Java, la identidad garantiza que, si se modifica el estado de un objeto, no se altere accidentalmente el de otro que resulte tener los mismos valores de forma casual.


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Para comprender la arquitectura de Java, es fundamental distinguir entre la definición teórica y la entidad práctica que se ejecuta en el ordenador. Estos conceptos se entrelazan para dar estructura al código.

### ¿Qué es una clase y es lo mismo que un objeto?

Una clase es una entidad puramente lógica; es un modelo, plantilla o "blueprint" que define las propiedades y comportamientos comunes a todos los elementos de un tipo determinado. No ocupa espacio en la memoria como dato ejecutable, sino que sirve para que el compilador sepa qué estructura tendrán los objetos que se creen a partir de ella. No es lo mismo que un objeto, del mismo modo que el plano de un motor no es el motor físico: el plano te dice cómo construirlo, pero no puedes arrancarlo ni usarlo para mover un vehículo.

### El concepto de instancia

Una **instancia** es el nombre técnico que recibe un objeto en el momento en que se le asigna memoria. El término "instanciar" proviene de la idea de crear un "caso concreto" (una instancia) de una regla general (la clase). En la práctica, los términos "objeto" e "instancia" se usan a menudo como sinónimos, aunque técnicamente "objeto" se refiere a la entidad en diseño y "instancia" a su existencia real en el **Heap** (la zona de memoria dinámica) durante la ejecución.

### Clases en otros lenguajes orientados a objetos

No todos los lenguajes de programación orientados a objetos utilizan el concepto de clase. Aunque Java, C++, C# y Python son lenguajes "basados en clases", existe otra vertiente denominada **POO basada en prototipos**, cuyo exponente más famoso es JavaScript. En estos lenguajes no existe un "plano" previo; los objetos se crean duplicando otros objetos ya existentes (prototipos) y añadiéndoles nuevas capacidades sobre la marcha.

En resumen, mientras que en Java (basado en clases) primero debes definir la estructura obligatoriamente, en los lenguajes basados en prototipos la estructura es mucho más fluida. Sin embargo, para el aprendizaje de Java, el modelo de clases es el estándar absoluto y rígido que se debe seguir para organizar cualquier programa.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En la gestión de memoria de Java, los objetos se almacenan en una región específica denominada **Heap** (o montón). A diferencia de las variables locales y las llamadas a funciones en C, que se gestionan automáticamente en el **Stack** (pila), el Heap es un área de memoria dinámica de mayor tamaño donde los objetos residen durante toda su vida útil. Cuando se crea un objeto en Java, la variable que manejamos no contiene el objeto en sí, sino una referencia (similar a un puntero en C) que apunta a la ubicación del objeto dentro del Heap.

Esta organización no es idéntica en todos los lenguajes. En C++, por ejemplo, el programador puede elegir si desea almacenar un objeto en el Stack o en el Heap. Si se usa el Heap en C o C++, es responsabilidad del programador liberar esa memoria manualmente usando `free()` o `delete`. Si se olvida hacerlo, se producen las conocidas "fugas de memoria" (*memory leaks*), que agotan los recursos del sistema. En cambio, en Java, el programador no tiene control directo sobre la liberación de la memoria de los objetos.

### La Recolección de Basura (Garbage Collection)

La **recolección de basura** es un proceso automático del entorno de ejecución de Java (la JVM) que se encarga de gestionar la memoria por el programador. Su función principal es identificar y destruir los objetos que ya no son accesibles, es decir, aquellos que ya no tienen ninguna referencia apuntando a ellos. Cuando el recolector detecta un objeto "huérfano" en el Heap, libera el espacio que ocupaba para que pueda ser reutilizado por nuevos objetos.

Este mecanismo simplifica enormemente el desarrollo, ya que elimina los errores de gestión de punteros y fugas de memoria tan comunes en C. Sin embargo, tiene un pequeño coste en el rendimiento, ya que el recolector de basura consume ciclos de CPU mientras analiza la memoria. En lenguajes de bajo nivel como C, este proceso no existe, delegando toda la eficiencia y seguridad de la memoria exclusivamente en la pericia del programador.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 
Un método es un bloque de código definido dentro de una clase que establece el comportamiento de los objetos. En términos de programación en C, se puede visualizar como una función, con la diferencia fundamental de que el método está indisolublemente ligado a los datos de la clase. Esto significa que un método tiene acceso automático a los atributos del objeto sin necesidad de recibirlos como parámetros externos, simplificando la lógica de manipulación del estado.

La estructura de un método incluye un modificador de acceso (como `public` o `private`), un tipo de retorno, el nombre del método y una lista de parámetros. Al invocar un método en Java, se utiliza la "notación de punto" sobre una instancia (`objeto.metodo()`), lo que refuerza la idea de que estamos solicitando al objeto que realice una acción específica.

### La Sobrecarga de Métodos

La sobrecarga es la capacidad de definir, dentro de una misma clase, varios métodos que comparten el mismo nombre pero poseen diferentes **firmas**. La firma de un método en Java está determinada por su nombre y su lista de parámetros (cantidad, tipo y orden). Para un programador de C, esto es una novedad importante, ya que en C no se pueden tener dos funciones con el mismo nombre en el mismo ámbito, obligando a crear nombres como `sumarInt`, `sumarFloat`, etc.

La utilidad de la sobrecarga radica en la legibilidad y la flexibilidad. Permite que un mismo concepto (como "inicializar" o "calcular") se aplique de distintas formas según los datos disponibles. Por ejemplo, se puede tener un método `dibujar()` que no reciba parámetros para usar valores por defecto, y otro `dibujar(int color)` que permita especificar una propiedad. El compilador de Java es quien decide, en tiempo de compilación, qué versión del método ejecutar basándose en los argumentos proporcionados en la llamada.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

Para implementar este ejemplo, se debe definir una clase que actúe como molde para coordenadas bidimensionales. En Java, los atributos `x` e `y` se declaran directamente dentro del cuerpo de la clase. Al no especificar un modificador como `public` o `private`, el lenguaje asigna automáticamente la **visibilidad por defecto** (o de paquete), lo que permite que otras clases en el mismo directorio accedan a ellos de forma directa, de manera similar a cómo se accedería a los miembros de una `struct` en C.

El método `calculaDistanciaAOrigen` utiliza la fórmula de la distancia euclidiana. En Java, las operaciones matemáticas avanzadas, como la raíz cuadrada o la potenciación, se realizan invocando métodos de la clase predefinida `Math`. Es importante notar que, a diferencia de una función en C donde habría que pasar el punto como argumento, aquí el método utiliza `x` e `y` directamente porque ya pertenecen al contexto del objeto.

### Definición de la clase

```java
public class Punto {
    // Atributos con visibilidad por defecto
    double x;
    double y;

    // Método para calcular la distancia al punto (0,0)
    public double calculaDistanciaAOrigen() {
        // Se aplica la fórmula: raíz cuadrada de (x^2 + y^2)
        return Math.sqrt(Math.pow(x, 2) + Math.pow(y, 2));
    }
}

```

### Ejemplo de uso (Instanciación)

Para utilizar la clase, se requiere una clase adicional (o un método `main`) donde se reserve memoria para el objeto. En este proceso, se utiliza el operador `new`, el cual devuelve una referencia al espacio reservado en el **Heap**. Una vez creado el objeto, se accede a sus atributos y métodos utilizando el operador punto, siguiendo una lógica muy parecida a la de las estructuras en C pero con una sintaxis más orientada a la interacción con el objeto.

```java
public class Principal {
    public static void main(String[] args) {
        // 1. Creación de la instancia (reserva de memoria)
        Punto miPunto = new Punto();

        // 2. Asignación de valores a los atributos
        miPunto.x = 3.0;
        miPunto.y = 4.0;

        // 3. Uso del método y obtención del resultado
        double distancia = miPunto.calculaDistanciaAOrigen();

        System.out.println("La distancia al origen es: " + distancia);
    }
}

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada de cualquier aplicación en Java es el método `public static void main(String[] args)`. Al igual que la función `main` en C, este método es el primer bloque de código que busca la Máquina Virtual de Java (JVM) para iniciar la ejecución del programa. Sin esta firma exacta, el programa puede compilar, pero no podrá ejecutarse como una aplicación independiente.

### El modificador `static`

La palabra reservada `static` indica que un miembro (ya sea un método o un atributo) pertenece a la **clase** en sí y no a una **instancia** (objeto) en particular. En un programa normal, para usar un método se necesita crear un objeto con `new`, pero la JVM necesita arrancar el programa antes de que existan objetos; por eso, el `main` debe ser `static`. Esto permite que se pueda invocar sin que el programador haya instanciado la clase previamente.

El uso de `static` no se limita al `main`. Se emplea con frecuencia para crear variables globales compartidas por todos los objetos de una misma clase. Por ejemplo, si en la clase `Punto` se añade un atributo `static int contador`, todos los puntos creados compartirán esa misma variable. Si un objeto la modifica, el cambio será visible para todos los demás. Es lo más parecido a una variable global en C, pero encapsulada dentro del ámbito de una clase.

### Combinación de `static` y `final`

Cuando se combinan `static` y `final`, se están definiendo **constantes de clase**. El modificador `final` en Java equivale a `const` en C: impide que el valor de la variable sea modificado después de su asignación inicial. Al ser también `static`, la constante está disponible para todo el programa sin ocupar memoria extra en cada objeto individual.

```java
public class Matematicas {
    // Definición de una constante global de clase
    public static final double PI = 3.14159;
}

```

Esta combinación es fundamental para la eficiencia y la seguridad. Se utiliza para valores fijos que no deben cambiar nunca, como configuraciones del sistema, límites de capacidad o constantes matemáticas. En C, esto solía hacerse con `#define`, pero en Java se prefiere este enfoque porque respeta el tipado de datos y las reglas de visibilidad de la orientación a objetos.


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para ejecutar un programa en Java desde la terminal, se deben seguir dos pasos diferenciados que reflejan la naturaleza híbrida de este lenguaje. Primero, se utiliza el comando `javac NombreArchivo.java` para compilar el código fuente. Si no hay errores, este comando genera un nuevo archivo con la extensión `.class`. Posteriormente, para iniciar el programa, se emplea el comando `java NombreArchivo` (sin la extensión), lo que arranca el entorno de ejecución y procesa el archivo generado.

### ¿Java es compilado? El Byte-code y archivos .class

Java no es un lenguaje puramente compilado como C, ni puramente interpretado como Python; es ambos. El proceso de compilación de `javac` no traduce el código a lenguaje máquina binario (específico para un procesador como Intel o ARM), sino a un lenguaje intermedio llamado **Byte-code**. Este código se almacena en los ficheros `.class` y es una instrucción universal que no depende del sistema operativo, lo que permite que el mismo archivo funcione en Windows, Linux o macOS.

### La Máquina Virtual de Java (JVM)

La **Máquina Virtual de Java (JVM)** es el software encargado de ejecutar el Byte-code. Se puede imaginar como un "ordenador ficticio" que se instala sobre el sistema operativo real. Su función es leer las instrucciones del archivo `.class` y traducirlas en tiempo real a las instrucciones específicas del hardware que se esté utilizando. Esta capa de abstracción es la que proporciona a Java su famosa independencia de plataforma: "escribe una vez, ejecuta en cualquier lugar" (*Write Once, Run Anywhere*).

Para un programador de C, esta es la diferencia más radical. En C, el compilador genera un ejecutable `.exe` o binario que solo funciona en la arquitectura para la que fue compilado. En Java, el programador distribuye los archivos `.class` y es la JVM de cada usuario la que se encarga de que ese código "encaje" con su procesador y sistema operativo, gestionando además la memoria de forma automática durante el proceso.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

En la programación Java, la palabra reservada `new` es el operador de **instanciación**. Su función es solicitar a la Máquina Virtual de Java que reserve un espacio de memoria en el **Heap** para almacenar un nuevo objeto de la clase especificada. Para un programador de C, `new` realiza una tarea similar a `malloc()`, con la gran diferencia de que, además de reservar memoria, invoca automáticamente al constructor para inicializar el objeto y devuelve una referencia (un puntero gestionado) hacia esa posición de memoria.

El **constructor** es un método especial cuya única misión es asegurar que el objeto nazca con un estado válido y coherente. Se distingue de los métodos normales porque debe llamarse exactamente igual que la clase y carece por completo de tipo de retorno (ni siquiera se pone `void`). Si no se define un constructor explícitamente, Java crea uno vacío por defecto, pero lo habitual es definir uno propio para obligar a que los datos esenciales se asignen en el momento de la creación.

A continuación, se presenta la clase `Empleado` con un constructor que inicializa sus tres atributos. Se utiliza la palabra clave `this` para diferenciar los atributos de la clase de los parámetros que recibe el constructor cuando ambos tienen el mismo nombre.

```java
public class Empleado {
    // Atributos
    String dni;
    String nombre;
    String apellidos;

    // Constructor: inicializa el objeto con datos obligatorios
    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

```

Para utilizar este constructor, se debe pasar la información requerida entre los paréntesis al usar `new`. Esto garantiza que no pueda existir un objeto `Empleado` en el sistema que no tenga, al menos, un DNI, un nombre y unos apellidos asignados. En C, esto requeriría llamar a una función de inicialización manualmente después de crear la `struct`, mientras que en Java es un paso atómico y obligatorio durante la creación.

```java
// Ejemplo de uso:
Empleado emp = new Empleado("12345678X", "Juan", "Pérez García");


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

La referencia `this` es una variable especial que existe de forma implícita dentro de cada método o constructor de una clase. Su función es actuar como un "puntero" que apunta al objeto actual que está ejecutando el código. En términos de C, se puede comparar con el puntero que pasaríamos manualmente a una función para modificar una estructura (`func(struct MiStruct *self, ...)`), con la diferencia de que en Java este puntero se gestiona automáticamente y siempre está disponible bajo el nombre `this`.

El uso principal de `this` es resolver ambigüedades de nombres cuando los parámetros de un método o constructor se llaman igual que los atributos de la clase. Al escribir `this.atributo`, se le indica explícitamente al compilador que se desea acceder a la variable del objeto y no a la variable local o al parámetro recibido. Además, permite que un objeto se pase a sí mismo como argumento a otros métodos o para encadenar llamadas.

En cuanto a otros lenguajes, el concepto es universal en la POO, aunque el nombre varía. Mientras que Java, C++, C# y JavaScript utilizan `this`, otros lenguajes como Python o Rust emplean la palabra `self` de manera explícita como primer parámetro de sus métodos. A pesar del cambio de nombre, la finalidad sigue siendo la misma: permitir que el código del método sepa exactamente sobre qué instancia de memoria debe operar.

### Ejemplo de uso en la clase `Punto`

A continuación se muestra cómo se aplicaría `this` en el constructor y en un método de la clase `Punto`. Es especialmente útil para que el código sea más legible, permitiendo que los parámetros tengan nombres descriptivos idénticos a los atributos:

```java
public class Punto {
    double x;
    double y;

    // Uso de 'this' para distinguir atributos de parámetros
    public Punto(double x, double y) {
        this.x = x; // 'this.x' es el atributo, 'x' es el parámetro
        this.y = y;
    }

    // Uso de 'this' para mayor claridad al modificar el estado
    public void moverAPosicion(double x, double y) {
        this.x = x;
        this.y = y;
    }
}


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

Para calcular la distancia entre dos puntos en un plano cartesiano, se debe aplicar una generalización de la fórmula utilizada anteriormente para el origen. En este caso, el método `distanciaA` interactúa con dos instancias de la clase `Punto`: la instancia actual (representada por la referencia `this`) y una instancia externa que se recibe como argumento del método.

Este escenario es un excelente ejemplo de cómo los objetos colaboran entre sí. Mientras que en C tendrías una función que recibe dos estructuras por valor o puntero, en Java le pides a un objeto que calcule su distancia respecto a otro. Internamente, el método accede a las coordenadas privadas del objeto pasado por parámetro utilizando la notación de punto, al mismo tiempo que accede a sus propias coordenadas mediante `this`.

### Implementación del método en la clase `Punto`

A continuación se muestra cómo se integra este método en la estructura de la clase. Se observa que el parámetro es de tipo `Punto`, lo que demuestra que una clase puede ser utilizada como un tipo de dato para los argumentos de sus propios métodos:

```java
public class Punto {
    double x;
    double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método que calcula la distancia entre el objeto actual y otro punto
    public double distanciaA(Punto otroPunto) {
        // Diferencia de las coordenadas X e Y
        double dx = this.x - otroPunto.x;
        double dy = this.y - otroPunto.y;
        
        // Aplicación del Teorema de Pitágoras: sqrt(dx^2 + dy^2)
        return Math.sqrt(Math.pow(dx, 2) + Math.pow(dy, 2));
    }
}

```

### Ejemplo de uso

En el código de ejecución, se crean dos instancias independientes. Al llamar a `p1.distanciaA(p2)`, el código dentro del método identificará a `p1` como `this` y a `p2` como `otroPunto`. Es importante notar que el resultado sería el mismo si se llamara a `p2.distanciaA(p1)`, lo que refleja la propiedad conmutativa de la distancia.

```java
public class Principal {
    public static void main(String[] args) {
        Punto p1 = new Punto(1.0, 1.0);
        Punto p2 = new Punto(4.0, 5.0);

        double distancia = p1.distanciaA(p2);
        System.out.println("La distancia entre los puntos es: " + distancia);
    }
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
