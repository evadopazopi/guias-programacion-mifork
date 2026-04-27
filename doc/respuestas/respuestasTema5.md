# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

Polimorfismo --> Objetivo : facilitar la extensión de los programas.
Facilita que esta extensión se haga creando código nuevo frente a editar código existente.
## 1. El Polimorfismo

El **polimorfismo** es la capacidad de un objeto de ser tratado como un tipo genérico (su clase padre) mientras mantiene su comportamiento específico. En Java, esto permite que una sola variable de referencia pueda apuntar a diferentes tipos de objetos a lo largo de la ejecución. Es una evolución sobre la programación en C, donde las estructuras son estáticas; aquí, el sistema puede gestionar diversos componentes bajo una misma identidad común.

Sirve principalmente para reducir la rigidez del código y facilitar la extensión de los programas. Al programar pensando en la interfaz común de una superclase en lugar de en las particularidades de cada subclase, se logra que el software sea más modular. Esto permite añadir nuevas funcionalidades en el futuro sin tener que modificar o recompilar las partes del sistema que ya funcionan con la clase base.

---

## 2. La Sobreescritura de Métodos

La **sobreescritura** es la acción de volver a definir en una subclase un método que ya existe en la clase superior. Para que sea efectiva, se debe respetar estrictamente la firma original del método: nombre, parámetros y tipo de retorno. Es la herramienta que permite que una clase hija "especialice" una acción general, proporcionando una implementación propia que sustituye a la heredada cuando el objeto es utilizado.

Este mecanismo es fundamental porque permite el **enlace dinámico**. Gracias a la sobreescritura, el lenguaje decide qué versión del código ejecutar basándose en el objeto real creado en memoria y no en el tipo de la variable que lo maneja. Así, aunque se llame al mismo método sobre una lista de diferentes objetos, cada uno reaccionará de la manera específica que se haya definido en su propia clase.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

## 2. Ligadura Dinámica y Enlace Tardío

La **ligadura dinámica** o **enlace tardío** (*late binding*) es el mecanismo por el cual la asociación entre la llamada a un método y su implementación real se pospone hasta el tiempo de ejecución. A diferencia del enlace estático, donde el compilador decide qué función ejecutar basándose en el tipo de la variable, en la ligadura dinámica la decisión se toma según el tipo real del objeto que reside en la memoria. Este proceso permite que un programa sea flexible y responda a la naturaleza de los datos en el momento preciso en que se utilizan.

La relación con el polimorfismo es intrínseca, ya que la ligadura dinámica es el motor técnico que lo hace posible. Sin ella, el polimorfismo sería puramente estético: aunque una variable de tipo "Figura" apuntara a un objeto "Círculo", el sistema ejecutaría siempre los métodos de "Figura". Gracias al enlace tardío, el entorno de ejecución intercepta la llamada y la redirige a la versión específica del método definida en la subclase, permitiendo que el comportamiento multiforme se manifieste.

En cuanto a su implementación, la necesidad de indicarlo explícitamente varía según el lenguaje. En **C++**, el programador debe declarar los métodos como `virtual` en la clase base para habilitar la ligadura dinámica; de lo contrario, se aplica enlace estático por defecto para priorizar la velocidad. En **Java**, por el contrario, el comportamiento es automático: todos los métodos no estáticos y no finales utilizan ligadura dinámica de forma nativa, por lo que no requiere de palabras clave adicionales para activarse.

Por último, en **Python** no existe la necesidad de indicar nada explícitamente, ya que es un lenguaje de tipado dinámico puro. En este entorno, todas las llamadas a métodos se resuelven en tiempo de ejecución de manera predeterminada. Dado que Python no realiza una comprobación de tipos estricta durante la compilación, el enlace tardío es la norma absoluta, facilitando un polimorfismo extremadamente flexible conocido comúnmente como "Duck Typing".


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

public class Soldado {
    public void saluda(){
        System.out.println ("¡A sus órdenes!);
    }
}

public class Artillero extends Soldado {
    public vois Saluda(){
        System.out.println ("Artilleros a sus ordenes);
    }
}

public class pruebaPolimorfismo {
    public staitc vois pasarRevista (Soldado[] soldados){
        ´for(Soldado soldado : soldados){
            soldado.saluda()
        }
    }
    public static void main (String [] args){
        Soldado[] soldados = new Soldado();

        soldados[0] = new Artillero();
        soldado [1] = new Soldado();
    pasarRevusta(soldados);
    }
}

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Al sobreescritar un método, es perfectamente posible invocar la implementación de la clase base para aprovechar su lógica y complementarla. Esta técnica es fundamental en programación orientada a objetos, ya que evita la duplicación de código y permite que las subclases extiendan el comportamiento heredado en lugar de reemplazarlo por completo desde cero. En Java, esto se logra mediante una referencia especial que apunta directamente al ámbito de la jerarquía inmediatamente superior.

Para que un **Zapador** salude de forma normal y luego añada su mensaje específico, se debe realizar una llamada interna dentro del método sobreescrito que apunte a la versión del método del **Soldado**. De esta manera, el programa primero ejecuta las instrucciones generales (como el saludo estándar) y, una vez finalizadas, continúa con las instrucciones adicionales propias de la especialidad del zapador.

```java
class Soldado {
    void saludar() {
        System.out.println("Presentando armas.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        super.saludar(); // Se invoca el método de la clase base
        System.out.println("ZAPADOR A SUS ORDENES.");
    }
}
```

La palabra clave utilizada para invocar al método de la clase base es **`super`**. En el contexto de Java, `super` funciona como una referencia a la instancia de la superclase, permitiendo acceder a sus métodos y constructores incluso si estos han sido sobreescritos en la clase hija. Es la herramienta que garantiza que la especialización de una clase no rompa ni ignore la funcionalidad básica ya establecida en niveles superiores de la herencia.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Override --> le pide al compilador que compruebe que se está sobreescribiendo lo que queremos.

Al sobreescriturar un método en Java, existen restricciones estrictas para mantener la coherencia en la jerarquía de clases. Los **parámetros** deben ser exactamente los mismos en cantidad, orden y tipo; si se modifica siquiera un parámetro, Java interpretará que se trata de un método distinto y no de una sobreescritura. En cuanto al **tipo de retorno**, este debe ser idéntico al del método original o, en versiones modernas de Java, un subtipo de este (lo que se conoce como retorno covariante).

La diferencia fundamental entre **sobreescritura** (*overriding*) y **sobrecarga** (*overloading*) reside en la firma del método y el momento de resolución. La sobreescritura ocurre entre una clase padre y una hija, manteniendo la firma intacta para cambiar el comportamiento (polimorfismo). Por el contrario, la sobrecarga ocurre habitualmente en la misma clase y consiste en definir métodos con el mismo nombre pero con diferentes parámetros, permitiendo que una misma operación acepte distintos tipos de datos de entrada.



La anotación **`@Override`** sirve como una instrucción para el compilador, indicándole que la intención del programador es sobreescritar un método de la superclase. No es obligatoria para que el código funcione, pero actúa como una red de seguridad. Si se intenta sobreescritar un método pero se comete un error tipográfico en el nombre o en los parámetros, el compilador generará un error de inmediato al no encontrar un método coincidente en la clase base.

Es altamente recomendable usar esta anotación siempre por una cuestión de **mantenibilidad y robustez**. Facilita la lectura del código al identificar rápidamente qué métodos son extensiones de la lógica heredada. Además, protege el programa ante cambios futuros: si alguien modifica la firma del método en la clase padre, el compilador avisará automáticamente de que todas las subclases que usaban `@Override` han quedado desactualizadas, evitando errores lógicos difíciles de detectar en tiempo de ejecución.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Efectivamente, en Java el polimorfismo se utiliza de manera implícita desde las etapas más básicas del aprendizaje. Dado que en este lenguaje todas las clases heredan automáticamente de la clase raíz `Object`, cualquier clase que se defina ya forma parte de una jerarquía de herencia. Al redefinir métodos como `toString()` o `equals()`, se está aplicando técnicamente la sobreescritura para alterar el comportamiento de la clase base universal.

Cuando se sobreescribe el método `toString()`, por ejemplo, se está haciendo uso del polimorfismo porque cualquier parte del sistema que espere un `Object` (como el método `System.out.println`) podrá llamar a dicha función. En tiempo de ejecución, gracias al enlace dinámico, Java no ejecutará la versión genérica de la clase `Object` que devuelve una dirección de memoria, sino la versión personalizada que se haya escrito para esa clase específica.


Por lo tanto, acciones tan comunes como imprimir un objeto por consola o comparar dos instancias para verificar su igualdad son manifestaciones directas de polimorfismo. Aunque al principio del estudio de Java esto pueda parecer una simple personalización de funciones, el mecanismo subyacente es exactamente el mismo que se utiliza en arquitecturas de software complejas.

Este enfoque permite que el ecosistema de Java sea extremadamente coherente. Al estar `toString` y `equals` definidos en la cúspide de la jerarquía, se garantiza que todos los objetos del lenguaje compartan una interfaz mínima común. El polimorfismo asegura que, a pesar de que cada objeto es diferente, todos puedan ser representados como texto o comparados entre sí de una forma estandarizada.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una **clase abstracta** es una clase diseñada exclusivamente para servir como base de una jerarquía, representando un concepto genérico que no debe ser materializado por sí mismo. Se utiliza para definir una estructura común y asegurar que todas sus subclases compartan ciertos métodos, pero sin ofrecer una implementación completa de todos ellos. Actúa como un molde o contrato que obliga a las clases hijas a seguir un esquema determinado.

Un **método abstracto** es una declaración de un método que no posee cuerpo (implementación). Solo define la firma del método: su nombre, parámetros y tipo de retorno. Su existencia en una clase indica que todas las subclases no abstractas están obligadas a proporcionar su propia lógica para ese método. Es la máxima expresión del polimorfismo, ya que garantiza que el mensaje pueda enviarse a cualquier objeto de la jerarquía, aunque cada uno lo resuelva a su manera.



No es posible crear instancias de una clase abstracta directamente. Si se intentara utilizar el operador `new` con una clase de este tipo, el compilador de Java generaría un error. Esta restricción es lógica: dado que la clase puede contener métodos sin definir (abstractos), permitir su creación daría lugar a objetos incompletos que no sabrían cómo reaccionar ante ciertas llamadas.

Para implementar el ejemplo del **Soldado**, la palabra clave `abstract` debe colocarse tanto en la definición de la clase como en la declaración del método que no tiene implementación. En el caso de `Soldado`, se situaría antes de la palabra `class` y antes del tipo de retorno en el método `atacar`. Las subclases como `Zapador` o `Infantería` no llevarán la palabra `abstract`, ya que ellas sí proporcionarán el código concreto para realizar el ataque.

```java
// La clase debe ser abstracta para contener métodos abstractos
abstract class Soldado {
    void saludar() {
        System.out.println("Presentando armas.");
    }

    // Método sin cuerpo: obliga a los hijos a implementarlo
    abstract void atacar(); 
}

class Zapador extends Soldado {
    @Override
    void atacar() {
        System.out.println("Colocando carga explosiva.");
    }
}
```


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

final en clases --> prohíbe heredar
final en métodos --> prohíbe sobreescribir

En Java, la palabra clave **`final`** actúa como un modificador de restricción que impide la modificación o extensión de ciertos elementos. Cuando se aplica a una **clase**, el efecto es que dicha clase no puede tener descendencia; es decir, ninguna otra clase puede utilizar la palabra `extends` sobre ella. Cuando se aplica a un **método**, se permite que las subclases lo hereden y lo utilicen, pero se les prohíbe terminantemente sobreescribirlo para cambiar su comportamiento original.

La relación de `final` con el polimorfismo es de **limitación o anulación**. Dado que el polimorfismo se basa en la capacidad de las subclases de alterar comportamientos mediante la sobreescritura, el uso de `final` en un método corta esa posibilidad de raíz. Si una clase se declara como `final`, el polimorfismo basado en la herencia de esa clase desaparece por completo, ya que no pueden existir versiones alternativas (subclases) de la misma.



El uso de este modificador suele responder a razones de **seguridad y diseño**. Al marcar una clase como final, el programador garantiza que nadie podrá alterar la lógica interna de sus métodos a través de la herencia, lo cual es vital en clases que manejan datos sensibles o comportamientos que deben ser inmutables por contrato. Es una forma de decir que la implementación actual es la definitiva y no debe ser especializada.

Un ejemplo fundamental de clase `final` en la API estándar de Java es la clase **`String`**. Los diseñadores del lenguaje decidieron que las cadenas de texto fueran inmutables y que su comportamiento no pudiera ser alterado por terceros para evitar problemas de seguridad y optimización de memoria. Otros ejemplos comunes incluyen las clases envolventes como **`Integer`** o **`Double`**, y la clase **`Math`**, las cuales están blindadas contra la herencia.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Mecanismos en Java:
-Sobreescritura
-Clases y métodos abtractos
-Interfaces --> Todos los métodos son abstractos, es decir, sin código (A partir de cierta versión de Java, se deja meter una implementación "default").
-Una clase puede implementar más de una interfaz

publi interface EntradaSalida {
    public String leerEntrada();
    public void escribirEnSalida(String salida);
}

public class TecladoPantalla implements EntradaSalida {

}

Las **interfaces** en Java son estructuras que definen un contrato de comportamiento que las clases deben cumplir. Se pueden visualizar como un conjunto de capacidades que una clase "promete" tener. A diferencia de las clases tradicionales, una interfaz no describe qué es un objeto, sino **qué puede hacer**. En su forma más pura, solo contienen firmas de métodos (sin cuerpo), obligando a cualquier clase que las implemente a proporcionar la lógica concreta para cada uno de esos métodos.

Aunque se parecen a las clases abstractas en que ninguna de las dos puede ser instanciada, existen diferencias clave. Una clase abstracta puede tener estado (variables de instancia) y métodos con código, mientras que las interfaces se enfocan en el comportamiento puro. Sin embargo, a partir de Java 8, se introdujeron los **métodos `default`**, que permiten incluir una implementación base dentro de la interfaz para evitar romper el código existente al añadir nuevas funciones, acercando ligeramente ambos conceptos.



Una de las mayores ventajas de las interfaces es que permiten la **herencia múltiple de comportamiento**. Mientras que en Java una clase solo puede heredar de una única superclase (`extends`), esa misma clase puede implementar un número ilimitado de interfaces (`implements`). Esto permite que un objeto sea visto desde múltiples perspectivas; por ejemplo, una clase `Smartphone` podría implementar `ReproductorMusica`, `Camara` y `Telefono` simultáneamente.

En el ejemplo proporcionado, la interfaz `EntradaSalida` obliga a la clase `TecladoPantalla` a definir cómo se lee una entrada y cómo se escribe una salida. Si la clase no define ambos métodos, el compilador dará error. Este mecanismo es el pilar de un diseño limpio, ya que permite que otras partes del programa interactúen con `TecladoPantalla` sabiendo únicamente que es un tipo de `EntradaSalida`, sin importar los detalles internos de cómo funciona el teclado o la pantalla.

```java
public interface EntradaSalida {
    String leerEntrada();
    void escribirEnSalida(String salida);
}

// La clase está obligada a implementar los métodos de la interfaz
public class TecladoPantalla implements EntradaSalida {
    @Override
    public String leerEntrada() {
        return "Texto desde teclado";
    }

    @Override
    public void escribirEnSalida(String salida) {
        System.out.println("Pantalla: " + salida);
    }
}
```

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

public abstract class Punto {
    public abstract double calculaDistanciaA()
}

public class Punto2D extends Punto{
    private double x,y;
    public Punto2D(douvle x, double y){
        this.x=x;
        this.y=y;
    }
    @Override
    public  double calculaDistanciaA(Punto otro){
        if(otro instanceof Punto2D otro2D){
            return Math.sqrt(Math.pow(this.x - otro2D.x,2)+Math.pow(this.y-otro2D.y,2))
        }else{
            throw new IllegalArgumentException ("No puedo calcular la distancia a otro punto de distinta dimensaionalidad");
        }
    }
}

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La **herencia de interfaces** en Java es el mecanismo mediante el cual una interfaz puede adquirir las definiciones de métodos de una o más interfaces existentes. A diferencia de las clases, donde se hereda comportamiento y estado, en las interfaces se heredan **contratos**. Esto permite crear jerarquías de capacidades, yendo de lo más general a lo más específico, facilitando que una clase pueda cumplir con un conjunto de requisitos acumulativos simplemente implementando la interfaz de nivel más alto.

En Java, a diferencia de la herencia de clases, sí existe la **herencia múltiple de interfaces**. Una interfaz puede extender varias interfaces simultáneamente utilizando la palabra clave `extends` seguida de una lista separada por comas. Esto no genera los conflictos típicos de la herencia múltiple de C++ (como el problema del diamante), ya que las interfaces originalmente no contienen implementaciones de métodos ni variables de estado que puedan colisionar, sino solo declaraciones de lo que se debe hacer.



En el ejemplo solicitado, la interfaz `Fichero` define la capacidad básica de lectura. Posteriormente, la interfaz `FicheroEscribible` extiende a `Fichero`, lo que significa que cualquier clase que decida ser un "fichero escribible" no solo tendrá que implementar los métodos de escritura y borrado, sino que también estará obligada por contrato a implementar el método de lectura original.

```java
public interface Fichero {
    String leerContenido();
}

// Herencia de interfaces: FicheroEscribible hereda el contrato de Fichero
public interface FicheroEscribible extends Fichero {
    void escribirContenido(String datos);
    void eliminar();
}

// Una clase que implemente la interfaz hija debe cumplir con todos los métodos
public class DocumentoTexto implements FicheroEscribible {
    @Override
    public String leerContenido() {
        return "Contenido del archivo...";
    }

    @Override
    public void escribirContenido(String datos) {
        // Lógica para escribir
    }

    @Override
    public void eliminar() {
        // Lógica para borrar el archivo
    }
}
```

Esta estructura es muy potente para el polimorfismo, ya que un objeto de la clase `DocumentoTexto` puede ser tratado en el programa simplemente como un `Fichero` si solo se necesita leer, o como un `FicheroEscribible` si se requieren permisos de modificación. Esto permite segregar las responsabilidades y asegurar que cada parte del sistema solo tenga acceso a las funciones que realmente necesita utilizar.