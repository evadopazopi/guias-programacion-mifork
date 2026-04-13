<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

La **herencia** es un mecanismo fundamental de la programación orientada a objetos que permite definir una nueva clase a partir de una ya existente. Se establece así una relación jerárquica donde la clase derivada (subclase) se considera una especialización de la clase base (superclase), lo que se traduce en la frase lógica **"A es-un B"**. A diferencia de la composición, donde un objeto "tiene" a otro, aquí el objeto hijo comparte la naturaleza esencial del padre, adquiriendo automáticamente sus capacidades y estructura.

La primera implicación crítica es la **herencia de estado y comportamiento**. Esto significa que los atributos (estado) y los métodos (comportamiento) definidos en la superclase están presentes en las subclases sin necesidad de volver a programarlos. Aunque los atributos sean privados en el padre para mantener la encapsulación, el hijo los posee internamente, lo que reduce drásticamente la duplicación de código y facilita el mantenimiento, ya que cualquier cambio en la lógica general del padre se propaga automáticamente a todos sus descendientes.

La segunda implicación es la **compatibilidad de tipos** (o polimorfismo de subtipo). En Java, un objeto de una subclase puede ser tratado legalmente como si fuera del tipo de su superclase. Esto permite crear estructuras de datos genéricas, como arrays de la clase base, que pueden almacenar cualquier objeto derivado. Esta flexibilidad es vital para escribir código extensible: se puede programar una función que reciba un tipo general y esta funcionará correctamente con cualquier variante específica que se cree en el futuro.



A continuación se muestra la implementación del ejemplo solicitado:

```java
// Clase Base
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soy el soldado " + nombre + ", ¡a sus órdenes!");
    }
}

// Subclases
class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre); // Llama al constructor del padre
        this.cohetes = cohetes;
    }

    public int getCohetes() { return cohetes; }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() { return minas; }
}

// Ejemplo de uso y compatibilidad de tipos
public class Main {
    public static void main(String[] args) {
        // Array de tipo general que acepta cualquier subtipo
        Soldado[] peloton = new Soldado[3];
        
        peloton[0] = new Artillero("Ramiro", 5);
        peloton[1] = new Zapador("Lucía", 10);
        peloton[2] = new Soldado("Genérico");

        // Recorrido polimórfico
        for (Soldado s : peloton) {
            s.saludar(); // Todos saben saludar por herencia
        }
    }
}
```

En el código anterior, se observa cómo el array `peloton` trata a todos sus integrantes como `Soldado`. Al ejecutar el bucle, se aprovecha la compatibilidad de tipos para invocar el método `saludar()`, el cual está garantizado por la herencia. Este enfoque permite gestionar una colección diversa de objetos de forma uniforme, simplificando la estructura del programa principal.


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Al instanciar un objeto de una clase derivada, se produce una ejecución en cadena de constructores que asciende por la jerarquía de herencia. En el ejemplo del `Artillero`, se ejecutan **dos constructores**: primero el de la superclase (`Soldado`) y, una vez finalizado este, el de la subclase (`Artillero`). Este orden es imperativo porque la especialización no puede construirse sobre la nada; es necesario que los cimientos del objeto (el estado heredado del padre) estén correctamente inicializados antes de añadir las particularidades del hijo.

La palabra clave **`super`** dentro de un constructor actúa como una llamada explícita al constructor de la clase base. Su función es delegar la inicialización de los atributos heredados al responsable original de los mismos. En Java, esta llamada debe ser siempre la **primera instrucción** del constructor de la subclase. Si no se escribe manualmente, el compilador intenta insertar de forma invisible una llamada a `super()`, pero esto solo funciona si la clase base dispone de un constructor sin parámetros.



Si la clase base carece de un constructor sin parámetros (como ocurre en `Soldado`, donde el constructor requiere obligatoriamente un `nombre`), es **obligatorio** llamar a `super` de forma explícita en todas las subclases. El compilador de Java lanzará un error si no se hace, ya que no sabría cómo inicializar la parte de "padre" del objeto. Esta restricción garantiza la integridad del sistema: un `Artillero` no puede existir si no se define primero su identidad básica como `Soldado`.



En resumen, la presencia de `super` asegura que la encapsulación no se rompa. Aunque el atributo `nombre` sea privado en `Soldado` y el `Artillero` no pueda acceder a él directamente, el uso de `super(nombre)` permite que el objeto hijo se configure correctamente a través de la interfaz que su padre ha definido. Este mecanismo refuerza la estructura jerárquica y evita que las subclases tengan que conocer los detalles internos de implementación de sus ancestros.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Cuando se crea una instancia de una subclase en memoria, como un objeto de tipo `Artillero`, este contiene **todos los atributos** definidos en su jerarquía de herencia. Esto significa que el atributo privado `nombre` de la clase `Soldado` ocupa un espacio físico dentro del objeto `Artillero` en la memoria RAM. Desde el punto de vista de la estructura de datos, el objeto hijo es una extensión del padre y, por lo tanto, "lleva consigo" toda la carga de estado que el padre definió, independientemente de los modificadores de acceso aplicados.

Sin embargo, que un atributo **forme parte** del objeto en memoria no implica que sea **accesible** directamente desde el código de la subclase. La encapsulación sigue vigente: el modificador `private` restringe la visibilidad del atributo estrictamente a los métodos de la clase donde fue declarado. Por tanto, un programador que escriba código dentro de la clase `Artillero` no podrá hacer algo como `this.nombre = "Ramiro";`, ya que el compilador de Java protegerá la privacidad de la superclase y lanzará un error de acceso.



Para que la subclase pueda interactuar con ese estado privado que posee en su interior, debe recurrir a la **interfaz pública o protegida** que el padre haya proporcionado. En nuestro ejemplo, el `Artillero` inicializa su nombre a través del constructor `super(nombre)` y, si necesitara consultarlo para una operación específica de artillería, debería hacerlo mediante un método *getter* público definido en `Soldado`. Este diseño asegura que, aunque el hijo herede la estructura, el padre mantenga el control total sobre cómo se manipulan sus datos sensibles.

En resumen, existe una distinción clara entre la **existencia física** (el atributo está ahí, en los bytes del objeto) y la **visibilidad lógica** (el código del hijo no tiene permiso para tocarlo). Esta separación es lo que permite que la herencia sea segura: el padre puede cambiar la implementación interna de su estado privado en el futuro sin temor a que las subclases, que dependen de él, rompan su lógica interna por haber manipulado esos datos de forma directa y no autorizada.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La **extensibilidad** es una de las mayores ventajas competitivas de la orientación a objetos frente al paradigma procedimental tradicional de C. Gracias a la compatibilidad de tipos, el sistema permite añadir nuevas funcionalidades o variantes de objetos sin necesidad de alterar el código que ya funciona. Esto se conoce como el principio de "abierto para la extensión, pero cerrado para la modificación": se pueden introducir nuevos comportamientos en el sistema simplemente creando nuevas subclases, mientras que los algoritmos que gestionan la clase base permanecen intactos.

En un programa escrito en C, añadir un nuevo tipo de entidad solía requerir la modificación de estructuras `switch` o cadenas de `if-else` repartidas por todo el código para reconocer el nuevo tipo de dato. En Java, al ser todos los subtipos compatibles con la clase base `Soldado`, cualquier método o estructura de datos que acepte un `Soldado` aceptará automáticamente cualquier clase que herede de ella en el futuro. Esto reduce drásticamente el riesgo de introducir errores en el código existente al realizar ampliaciones.



Para ilustrar esta capacidad, se puede añadir un nuevo tipo de soldado, por ejemplo un `Medico`, que se encarga de curar compañeros. A pesar de ser una clase totalmente nueva con sus propios atributos, el código encargado de gestionar el pelotón y solicitar los saludos no requiere ni una sola línea de cambio para integrarlo.

```java
// Nueva extensión del sistema sin tocar las clases anteriores
class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() { return botiquines; }
}

// Demostración de extensibilidad en el Main
public class MainExtensible {
    public static void main(String[] args) {
        // El código que maneja el array NO CAMBIA, aunque añadamos tipos nuevos
        Soldado[] peloton = new Soldado[4];
        
        peloton[0] = new Artillero("Ramiro", 5);
        peloton[1] = new Zapador("Lucía", 10);
        peloton[2] = new Medico("Sonia", 3); // Nuevo tipo añadido sin problemas
        peloton[3] = new Soldado("Genérico");

        // Este bucle es "ciego" a los tipos concretos, solo sabe que son Soldados
        // No hay que modificarlo para que Sonia salude correctamente
        for (Soldado s : peloton) {
            s.saludar(); 
        }
    }
}
```



Como se observa en el ejemplo, la lógica de recorrido del array es agnóstica a la existencia del `Medico`. La compatibilidad de tipos permite que el objeto de tipo `Medico` se "disfrace" de `Soldado` para entrar en el array y responder al método `saludar()`. Esta arquitectura permite que los sistemas crezcan de forma modular y orgánica, facilitando que diferentes programadores añadan tipos nuevos sin interferir en el núcleo del motor del programa.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

En Java, es perfectamente válido y común tener una referencia de un **supertipo** (clase padre) apuntando a un objeto real de un **subtipo** (clase hija). Esta es la base del polimorfismo: una variable de tipo `Soldado` puede almacenar la dirección de memoria de un `Artillero`. Sin embargo, existe una limitación importante: a través de esa referencia de supertipo, solo se pueden invocar los métodos que estén definidos en la clase padre. El compilador "solo ve" lo que la clase de la referencia declara, por lo que no permitiría llamar a métodos específicos del hijo (como `getCohetes()`) aunque sepamos que el objeto real los tiene.

Para gestionar estas situaciones, se utilizan los conceptos de **upcasting** y **downcasting**. El *upcasting* es la conversión de una referencia de subtipo a supertipo (por ejemplo, tratar un `Artillero` como `Soldado`); es automático y seguro porque un hijo siempre "es-un" padre. El *downcasting*, en cambio, es la conversión inversa: tratar una referencia de supertipo como si fuera de un subtipo. Esta operación es arriesgada y requiere un "cast" explícito, ya que el compilador no puede asegurar que el `Soldado` al que apuntamos sea realmente un `Artillero` y no un `Zapador` o un soldado genérico.



Para realizar un *downcasting* de forma segura, Java proporciona el operador **`instanceof`**. Este operador permite comprobar en tiempo de ejecución si un objeto pertenece a una clase determinada o a una de sus subclases. Si se intenta realizar un cast hacia un tipo incompatible, el programa lanzaría una excepción (`ClassCastException`), por lo que el uso de `instanceof` actúa como una guarda o protección necesaria antes de acceder a las funcionalidades específicas de la subclase.

A continuación se muestra cómo aplicar estos conceptos para acceder a datos específicos dentro de una colección general:

```java
public class MainCast {
    public static void main(String[] args) {
        Soldado[] peloton = {
            new Artillero("Ramiro", 5),
            new Zapador("Lucía", 10),
            new Soldado("Genérico")
        };

        for (Soldado s : peloton) {
            s.saludar(); // Método común (accesible para todos)

            // Comprobación de tipo real para acceder a lo específico
            if (s instanceof Artillero) {
                // Downcasting seguro tras la comprobación
                Artillero a = (Artillero) s; 
                System.out.println("-> Munición especial: " + a.getCohetes() + " cohetes.");
            }
            
            // En versiones modernas de Java (16+), se puede simplificar así:
            // if (s instanceof Zapador z) {
            //     System.out.println("-> Minas: " + z.getMinas());
            // }
        }
    }
}
```



En el ejemplo anterior, la variable `s` es una referencia de supertipo. Cuando el bucle encuentra a "Ramiro", `instanceof` confirma que el objeto real es un `Artillero`. Solo entonces se realiza el cast para "engañar" al compilador y permitirle ver los métodos que no existen en la clase `Soldado`. Sin esta técnica, la información específica de las subclases quedaría inaccesible una vez que los objetos se guardan en una estructura de datos genérica.


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El modificador de acceso **`protected`** representa un nivel de visibilidad intermedio entre el rigor del `private` y la apertura total del `public`. Su propósito fundamental en la orientación a objetos es permitir que los miembros de una clase (atributos o métodos) sean accesibles para sus **subclases**, incluso si estas se encuentran en paquetes distintos, y también para cualquier otra clase dentro del mismo paquete. Es la herramienta diseñada específicamente para romper la barrera de la encapsulación estrictamente en favor de la jerarquía de herencia.

En Java, se implementa sustituyendo la palabra clave `private` por **`protected`** en la declaración del miembro. Al hacer esto, el atributo o método deja de ser invisible para los hijos. En términos de diseño, esto se utiliza cuando se desea que las subclases manipulen directamente ciertos datos para optimizar el rendimiento o simplificar la lógica, sin exponer esos mismos datos al resto del mundo exterior (clases que no heredan ni pertenecen al mismo paquete).



A continuación se muestra cómo el `Zapador` puede utilizar el nombre de su padre de forma directa gracias a este modificador:

```java
// Clase Base
class Soldado {
    // Al ser protected, las subclases pueden leerlo y escribirlo directamente
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soldado " + nombre + " presente.");
    }
}

// Subclase que hace uso del acceso protegido
class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void colocarMina() {
        if (minas > 0) {
            minas--;
            // Acceso directo a 'nombre' sin necesidad de getter
            System.out.println(nombre + " ha colocado una mina. Quedan: " + minas);
        }
    }
}
```



A pesar de su utilidad, el uso de atributos `protected` debe hacerse con cautela. Al permitir que las subclases modifiquen directamente los atributos del padre, se crea un **acoplamiento fuerte** entre ellos. Si en el futuro se decide cambiar la forma en que el `Soldado` almacena su identidad, todas las subclases que accedían directamente al atributo `nombre` se romperían. Por esta razón, muchos desarrolladores prefieren mantener los atributos como `private` y marcar como `protected` únicamente los métodos que permiten una manipulación controlada de los mismos.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

En la mayoría de los lenguajes orientados a objetos modernos, existe el concepto de una **clase raíz** de la cual derivan todas las demás de forma directa o indirecta. Esta arquitectura crea un árbol jerárquico unificado donde cualquier instancia, sin importar su propósito, comparte un conjunto mínimo de capacidades. Sin embargo, esto **no ocurre en todos los lenguajes**. En C++, por ejemplo, no existe una clase base común; se pueden crear múltiples jerarquías independientes (bosques de árboles), lo que otorga máxima eficiencia pero complica la creación de colecciones que puedan almacenar "cualquier cosa".

En **Java**, por el contrario, existe una clase base universal denominada **`Object`**. Pertenece al paquete `java.lang` y es la cúspide de toda la jerarquía. Si al definir una clase no se utiliza la palabra clave `extends`, el compilador de Java inserta automáticamente `extends Object`. Esto garantiza que cada objeto en el ecosistema Java, desde un simple `String` hasta nuestro complejo `Soldado`, herede métodos fundamentales como `toString()` (para representación textual), `equals()` (para comparación lógica) o `hashCode()`.



Esta estructura unificada en Java tiene implicaciones profundas para la **compatibilidad de tipos**. Gracias a que todo "es-un" `Object`, es posible diseñar métodos y estructuras de datos extremadamente genéricos. Por ejemplo, antes de la llegada de los Genéricos en Java, la única forma de crear una lista que aceptara cualquier tipo de dato era declararla como una lista de referencias a `Object`. Esto permite que el lenguaje sea muy cohesivo, ya que cualquier herramienta del sistema puede asumir que todos los datos con los que trabaja poseen los métodos básicos de la clase raíz.



Finalmente, esta herencia universal facilita el **polimorfismo extremo**. Se puede pasar cualquier objeto a un método que reciba un `Object`, permitiendo tareas como la serialización, el almacenamiento en bases de datos o la inspección en tiempo de ejecución (reflexión) de manera uniforme. En resumen, mientras que en lenguajes como C++ el programador decide si quiere jerarquías, en Java la jerarquía es una infraestructura global que asegura que todo elemento del lenguaje hable el mismo "idioma básico" de objetos.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La **herencia múltiple** es una característica de algunos lenguajes de programación (como C++ o Python) que permite que una clase derivada herede atributos y comportamientos de **más de una clase base** simultáneamente. En el mundo real, esto equivaldría a decir que un objeto "C" es, al mismo tiempo, un "A" y un "B". Por ejemplo, una clase `CocheAnfibio` podría heredar de la clase `VehiculoTerrestre` y de la clase `VehiculoMaritimo`, obteniendo las propiedades de ambas jerarquías en una sola definición.

A pesar de su aparente utilidad, la herencia múltiple introduce un problema crítico conocido como el **"Problema del Diamante"** (*Diamond Problem*). Este conflicto ocurre cuando dos superclases tienen un método con el mismo nombre pero con implementaciones distintas, y la subclase hereda de ambas. En ese escenario, el compilador no puede determinar de forma ambigua qué versión del método debe ejecutar el objeto hijo, lo que suele generar errores complejos de memoria y ambigüedades en la estructura de los objetos.



En **Java, no existe la herencia múltiple de clases**. Los diseñadores del lenguaje decidieron omitirla deliberadamente para priorizar la simplicidad y la robustez del código. En Java, una clase solo puede tener **un único padre** directo (herencia simple). Esto elimina de raíz los conflictos de ambigüedad y asegura que la jerarquía de objetos sea siempre un árbol claro y predecible, facilitando enormemente la depuración y el mantenimiento del software a gran escala.

Para suplir la necesidad de que un objeto cumpla con varios roles o contratos distintos, Java utiliza las **Interfaces**. Una clase en Java puede implementar múltiples interfaces, lo que le permite garantizar que posee ciertos comportamientos (como ser "Dibujable", "Serializable" o "Comparable") sin heredar la implementación interna de múltiples padres. De esta forma, Java consigue la flexibilidad de la herencia múltiple (un objeto puede ser tratado como varios tipos diferentes) sin heredar los problemas estructurales y de colisión de nombres que esta conlleva.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

En Java, las excepciones son ciudadanos de primera clase, lo que significa que son objetos que heredan de la jerarquía de `Throwable`. Para crear una **excepción personalizada**, basta con definir una clase que herede de `Exception` (si se desea que sea controlada/checked) o de **`RuntimeException`** (si se desea que sea **no controlada**/unchecked). Las excepciones no controladas son aquellas que el compilador no obliga a capturar o declarar, siendo ideales para errores de lógica de programación o estados inválidos que no pueden recuperarse fácilmente.

Al ser objetos convencionales, las excepciones pueden aplicar el concepto de **composición**. Se puede añadir un atributo de tipo `Usuario` dentro de la excepción para que, cuando el error sea capturado en un bloque `catch`, el manejador no solo reciba un mensaje de texto, sino también el objeto real que causó el conflicto. Esto permite una depuración mucho más precisa, ya que se pueden inspeccionar los atributos del usuario (ID, email, etc.) directamente desde el objeto de la excepción.



Para permitir que nuestra excepción encierre una **causa** (otra excepción previa que originó el fallo), se debe aprovechar la herencia y los constructores de la superclase. La clase `Throwable` ya dispone de lógica para almacenar una causa; por tanto, en nuestra clase personalizada simplemente sobrecargamos el constructor para invocar a `super(mensaje, causa)`. Esta técnica se conoce como *exception chaining* y es fundamental para no perder el rastro del error original en sistemas complejos.

A continuación se presenta la implementación de la excepción personalizada solicitada:

```java
// Heredamos de RuntimeException para que sea "no controlada"
public class UsuarioNoEncontradoException extends RuntimeException {
    
    // Composición: la excepción contiene al usuario problemático
    private final Usuario usuario;

    // Constructor estándar
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        super(mensaje);
        this.usuario = usuario;
    }

    // Constructor sobrecargado para permitir incluir la causa (recursividad)
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario, Throwable causa) {
        super(mensaje, causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```



En este diseño, se observa cómo la herencia permite que `UsuarioNoEncontradoException` se comporte como cualquier otra excepción del sistema, mientras que la composición y la sobrecarga de constructores le añaden semántica específica de nuestra aplicación. Al capturar esta excepción, se tendría acceso tanto a la traza del error como al estado del objeto `Usuario`, facilitando enormemente la resolución de problemas en tiempo de ejecución.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

El uso de la herencia como herramienta principal para la reutilización de código es una de las trampas más comunes en el diseño orientado a objetos. Aunque heredar permite aprovechar métodos y atributos de una clase base, este enfoque introduce un **acoplamiento fuerte** entre el padre y el hijo. Cualquier cambio en la superclase puede romper el funcionamiento de todas sus subclases de manera inesperada (el llamado "problema de la clase base frágil"). En C, esto equivaldría a modificar una estructura global de la que dependen cientos de funciones: el riesgo de efectos colaterales es altísimo.

Además, la herencia es una relación estática que se define en tiempo de compilación. Una vez que una clase hereda de otra, esa relación no puede cambiarse mientras el programa se ejecuta. Si se utiliza la herencia solo para "copiar" funcionalidad, se está forzando una relación jerárquica que puede no ser lógica. Por ejemplo, si una clase `Coche` hereda de `Motor` solo para usar sus métodos, estaríamos diciendo que un "Coche es un Motor", lo cual es falso y confunde la arquitectura del sistema.



Por el contrario, la **composición** (la relación "tiene-un") ofrece una flexibilidad mucho mayor para la reutilización. Al incluir una instancia de otra clase como un atributo, se puede cambiar el comportamiento del objeto en tiempo de ejecución (por ejemplo, cambiando un motor de combustión por uno eléctrico). La composición mantiene las clases **débilmente acopladas**, ya que la clase contenedora solo interactúa con la interfaz pública del objeto contenido, sin depender de sus detalles internos de implementación.

En el diseño moderno, se aplica el principio de **"Favorecer la composición sobre la herencia"**. La herencia debe reservarse estrictamente para cuando existe una relación semántica real de especialización ("es-un") y se desea aprovechar el polimorfismo. Para todo lo demás, la composición resulta ser una opción más robusta, fácil de testear y menos propensa a errores en sistemas que evolucionan con el tiempo.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

La máxima de **"favorecer la composición frente a la herencia"** es uno de los principios de diseño fundamentales en la orientación a objetos moderna. Aunque la herencia es una herramienta potente, su uso indiscriminado para reutilizar código suele conducir a sistemas rígidos y difíciles de mantener. La razón principal es que la herencia establece una relación de **acoplamiento fuerte** (o "caja blanca"): la subclase depende de los detalles internos de la superclase, lo que rompe en cierta medida la encapsulación. Si la clase padre cambia su implementación, es muy probable que todas sus hijas dejen de funcionar correctamente de forma inesperada.

Por el contrario, la **composición** permite construir funcionalidad compleja mediante la agrupación de objetos simples, estableciendo una relación de **acoplamiento débil** (o "caja negra"). En este modelo, una clase no "es" otra, sino que "tiene" una instancia de otra clase para delegar tareas. Esto respeta estrictamente la encapsulación, ya que la clase contenedora solo interactúa con la interfaz pública del objeto contenido, sin importarle cómo está implementado por dentro.



Otra ventaja crítica de la composición es la **flexibilidad en tiempo de ejecución**. La herencia es una relación estática que se define al escribir el código; un objeto no puede cambiar de "padre" mientras el programa se ejecuta. Sin embargo, con la composición, un atributo que referencia a otro objeto puede ser sustituido por una implementación distinta en cualquier momento. Por ejemplo, un objeto `Guerrero` que *tiene un* `Arma` puede cambiar su lanza por un arco dinámicamente, algo que sería imposible o extremadamente farragoso de gestionar mediante una jerarquía de herencia rígida.



En conclusión, se debe usar la herencia únicamente cuando existe una relación semántica clara de **"es-un"** y se desea aprovechar el polimorfismo (tratar a diferentes hijos como un mismo padre). Para todos los demás casos donde el objetivo es simplemente dotar a una clase de nuevas capacidades o reutilizar lógica existente, la composición resulta ser una arquitectura mucho más robusta, fácil de testear y preparada para el cambio.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?
La afirmación de que la **herencia rompe la encapsulación** se refiere a que, al heredar, la subclase se vuelve dependiente de los detalles internos de implementación de su superclase. En el modelo de objetos ideal, una clase es una "caja negra" que oculta su funcionamiento interno y solo expone una interfaz pública. Sin embargo, la herencia crea una relación de **"caja blanca"**, donde el programador de la subclase a menudo necesita conocer cómo están implementados los métodos del padre para no corromper el estado del objeto al sobrescribirlos o extenderlos.

El problema fundamental es el **acoplamiento fuerte**. Si la superclase cambia su lógica interna en una actualización (por ejemplo, cambia el orden en que sus métodos se llaman entre sí), todas las subclases pueden dejar de funcionar correctamente de forma silenciosa. En C, esto sería equivalente a que el cambio en el cuerpo de una función de una librería alterara el comportamiento de tus funciones locales porque ambas comparten variables globales ocultas. En Java, este fenómeno se conoce como el problema de la **clase base frágil**.



Un ejemplo clásico ocurre cuando una superclase tiene un método `añadir()` y otro `añadirTodos()`, donde el segundo llama internamente al primero. Si una subclase decide sobrescribir ambos para llevar un contador de elementos, podría acabar contando doble si no sabe que `añadirTodos()` ya invoca a `añadir()`. La encapsulación ha fallado porque el autor de la subclase ha tenido que "mirar dentro" del código del padre para entender cómo extenderlo sin introducir errores lógicos, algo que no ocurre con la composición.



Por esta razón, la herencia debe usarse con extrema cautela. Mientras que la **composición** respeta la encapsulación al tratar al otro objeto como una entidad externa e independiente que solo se comunica mediante mensajes públicos, la herencia funde ambas clases en una sola unidad de memoria. Esta unión hace que cualquier vulnerabilidad o decisión de diseño errónea en el ancestro se propague inevitablemente a toda su descendencia, dificultando la evolución del software a largo plazo.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

Este ejercicio permite comparar visualmente cómo se estructura la información y la relación entre objetos dependiendo del mecanismo elegido. En ambos casos el objetivo es el mismo (reutilizar el DNI y el nombre), pero la semántica y la flexibilidad resultantes son profundamente distintas.

En la **solución por herencia**, se define una jerarquía vertical. `Estudiante` y `Trabajador` se consideran especializaciones de `Persona`. Esto implica que un objeto `Estudiante` **es** una `Persona` en todos los contextos. La ventaja es que podemos usar polimorfismo (un array de `Persona` que contenga ambos), pero el inconveniente es la rigidez: si un `Estudiante` empieza a trabajar, no puede "itinerar" fácilmente a ser también un `Trabajador` sin crear un tercer tipo de clase o duplicar datos, ya que la herencia en Java es simple y estática.



```java
// OPCIÓN 1: HERENCIA ("Es-un")
class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class Estudiante extends Persona {
    private String universidad;

    public Estudiante(String dni, String nombre, String universidad) {
        super(dni, nombre);
        this.universidad = universidad;
    }
}
```

En la **solución por composición**, se define una relación horizontal. Aquí, el `Estudiante` no hereda de nadie, sino que **tiene** un objeto de tipo `DatosPersonales` como un atributo más. Esta aproximación es mucho más flexible: los `DatosPersonales` son una pieza modular (una "caja negra") que se inyecta en el constructor. Si en el futuro necesitáramos que un objeto represente a alguien que estudia y trabaja a la vez, podríamos simplemente pasarle la misma instancia de `DatosPersonales` a ambos roles, manteniendo la identidad del sujeto sincronizada sin jerarquías forzadas.



```java
// OPCIÓN 2: COMPOSICIÓN ("Tiene-un")
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
    // Getters necesarios para acceder a los datos
}

class Trabajador {
    private DatosPersonales datos; // El trabajador TIENE datos personales
    private double sueldo;

    public Trabajador(DatosPersonales datos, double sueldo) {
        this.datos = datos;
        this.sueldo = sueldo;
    }
}
```

En resumen, mientras que la herencia funde los conceptos en una sola unidad de memoria indivisible, la composición mantiene los datos personales como una entidad independiente. Para un sistema que requiera que los roles de las personas cambien dinámicamente (un estudiante que se gradúa y pasa a ser trabajador), la composición resulta ser una arquitectura mucho más limpia y adaptable a largo plazo.