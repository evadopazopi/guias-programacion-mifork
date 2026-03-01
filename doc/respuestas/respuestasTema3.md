<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En el lenguaje C, al carecer de un mecanismo nativo de excepciones, la gestión de errores se basa en la comunicación mediante el flujo de datos convencional. Para que la función `raiz` informe de un error al exterior sin imprimir mensajes directamente, se debe recurrir a convenciones de diseño donde el valor de retorno o parámetros adicionales actúen como indicadores de estado.

A continuación, se presentan dos formas comunes de diseñar este control de errores:

### Opción 1: Uso de un valor de retorno reservado (Centinela)

Esta técnica consiste en devolver un valor que sea legal según el tipo de dato (`float`), pero que sea lógicamente imposible como resultado de la operación. En el caso de una raíz cuadrada, dado que el resultado nunca puede ser negativo, se puede devolver un número negativo (como `-1.0`) o la constante `NAN` (Not a Number) definida en la librería `math.h` para indicar que la entrada fue inválida.

El programa que llama a la función debe comprobar sistemáticamente este valor antes de proceder con el uso del dato. Es el método más sencillo, pero requiere que exista un rango de valores "imposibles" que puedan sacrificarse para representar el error, algo que no siempre es posible en todas las funciones matemáticas.

```c
#include <stdio.h>
#include <math.h>

float raiz(float n) {
    if (n < 0) {
        return -1.0; // Valor centinela para indicar error
    }
    return sqrt(n);
}

int main() {
    float num = -5.0;
    float res = raiz(num);
    
    if (res < 0) {
        printf("Error: No se puede calcular la raiz de un numero negativo.\n");
    } else {
        printf("Resultado: %f\n", res);
    }
    return 0;
}

```

---

### Opción 2: Paso de parámetros por referencia para el estado

En este diseño, la función devuelve un código de error (habitualmente un entero) y el resultado real de la operación se "devuelve" a través de un puntero pasado como argumento. Esta aproximación es más robusta, ya que separa claramente el **estado de la ejecución** (éxito o fallo) del **valor calculado**.

Si la función devuelve `0`, se asume éxito; si devuelve un código distinto de cero, el valor almacenado en el puntero debe ignorarse. Este método es el estándar en muchas librerías de sistemas operativos y permite devolver múltiples tipos de errores de forma descriptiva, sin comprometer el rango de valores del resultado matemático.

```c
#include <stdio.h>
#include <math.h>

// Devuelve 0 si es correcto, 1 si hay error
int raiz_segura(float n, float *resultado) {
    if (n < 0) {
        return 1; // Código de error
    }
    *resultado = sqrt(n);
    return 0; // Éxito
}

int main() {
    float num = -9.0;
    float res;
    
    if (raiz_segura(num, &res) != 0) {
        printf("Error detectado: Entrada negativa no permitida.\n");
    } else {
        printf("La raiz es: %f\n", res);
    }
    return 0;
}

```


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una **excepción** es un objeto que representa un error o evento inesperado durante la ejecución. A diferencia de C, donde el error es un simple número (como `-1`), aquí es una estructura con información detallada sobre qué falló y en qué línea de código ocurrió.

Al **implementar una función**, el programador las usa para delegar la responsabilidad. En lugar de decidir si imprimir un mensaje o abortar, la función lanza la excepción y deja que quien la invocó decida cómo reaccionar. Esto mantiene la lógica del cálculo separada de la gestión de errores.

Al **llamar a una función**, el objetivo es la robustez. Las excepciones obligan al programador a considerar los casos de falla, evitando que un error pase desapercibido por olvidar un `if`. Permiten agrupar el manejo de múltiples errores en un solo bloque, manteniendo el código principal limpio y legible.



## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java, para informar de un error desde una función sin usar valores de retorno especiales, se utiliza la palabra clave `throw` seguida de un objeto de excepción. En este caso, se emplea `IllegalArgumentException`, una excepción predefinida para indicar que un parámetro no es válido (como un número negativo para una raíz).

Al llamar al método desde el `main`, el código se encierra en un bloque `try`. Si la función lanza la excepción, el flujo se desvía inmediatamente al bloque `catch`, donde se captura el objeto de error. Esto permite que el programa informe al usuario y continúe su ejecución en lugar de finalizar abruptamente por un error matemático.

```java
public class Calculadora {

    public double raiz(double n) {
        if (n < 0) {
            // Se lanza la excepción para que el llamador la gestione
            throw new IllegalArgumentException("No se puede calcular la raíz de un número negativo.");
        }
        return Math.sqrt(n);
    }

    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        double numero = -9.0;

        try {
            double resultado = calc.raiz(numero);
            System.out.println("El resultado es: " + resultado);
        } catch (IllegalArgumentException e) {
            // Se captura el error lanzado por el método raiz
            System.out.println("Error detectado: " + e.getMessage());
        }
        
        System.out.println("El programa continúa su ejecución normal.");
    }
}

```

Este enfoque separa la lógica de cálculo (dentro del método `raiz`) de la lógica de interfaz de usuario (el `println` del error en el `main`). A diferencia de C, el valor de retorno de `raiz` es siempre un número válido, y el error viaja por un canal paralelo de comunicación.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

**Lanzar** una excepción es el acto de señalizar un error mediante la palabra clave `throw`. En el ejemplo de la raíz, cuando se detecta un número negativo, la función "crea" un objeto de error y lo expulsa de su flujo normal. Esto detiene inmediatamente la ejecución de esa función, enviando el objeto de error hacia arriba, a quien la haya invocado.

**Capturar o controlar** una excepción consiste en encerrar una llamada a un método dentro de un bloque `try-catch`. El `try` vigila si ocurre un error y, si sucede, el `catch` lo atrapa. Al capturarla, el programador toma el control del problema, evitando que el programa se detenga y permitiendo que la ejecución continúe de forma segura después del bloque de captura.

La **propagación** ocurre cuando una función lanza una excepción y nadie la captura en ese nivel. El error viaja "hacia atrás" por la pila de llamadas (del método `raiz` al `main`, y del `main` al sistema operativo). Si una función en la pila no tiene un bloque `catch`, esa función finaliza abruptamente en ese mismo instante, liberando sus variables locales pero sin completar el resto de sus instrucciones.

Las funciones que no controlan la excepción **no se reanudan jamás**. Una vez que la excepción atraviesa una función sin ser capturada, esa función se considera terminada por error. El flujo de ejecución solo se retoma en el punto exacto donde alguien finalmente use un `catch`. Si nadie la captura en toda la jerarquía de llamadas, la Máquina Virtual de Java detiene el programa por completo y muestra el error por consola.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

En C, la propagación de un error es **manual y obligatoria** en cada paso. Si una función `A` llama a `B`, y `B` llama a `raiz`, cada una de esas funciones debe incluir sentencias `if` para capturar el error de la anterior y volver a devolver un código de error hacia arriba. Si un programador olvida una sola comprobación en la cadena, el error se "silencia" y el programa continúa con datos inválidos, lo que suele derivar en fallos catastróficos difíciles de depurar.

La **propagación natural** de Java permite que el error "salte" automáticamente a través de múltiples niveles de la pila hasta encontrar a alguien capacitado para manejarlo. Esto significa que las funciones intermedias no necesitan código específico de gestión de errores si no pueden aportar una solución; simplemente se cierran y dejan que la excepción fluya. Esto reduce drásticamente el "ruido" en el código, permitiendo que la lógica principal sea mucho más limpia y directa.

Otra ventaja fundamental es la **imposibilidad de ignorar el error por descuido**. En C, si no se comprueba un valor de retorno, el programa sigue adelante. En Java, si una excepción se propaga y nadie la controla, el programa se detiene informando exactamente qué falló y dónde (el *stack trace*). Esto garantiza que los errores se hagan visibles de inmediato durante el desarrollo, en lugar de convertirse en comportamientos erráticos e inexplicables en producción.

Por último, esta jerarquía permite una **gestión centralizada**. Un programador puede escribir diez funciones que realicen operaciones matemáticas y controlarlas todas con un único bloque `try-catch` en el método principal. En lugar de diez comprobaciones manuales, se tiene un solo punto de control que decide cómo informar al usuario, simplificando enormemente el mantenimiento del software.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En Java, efectivamente, las **excepciones son objetos**. Esto significa que cada vez que ocurre un error, se instancia una clase específica (como `ArithmeticException` o `NullPointerException`). Al ser objetos, heredan de una jerarquía común cuya raíz es la clase `Throwable`, lo que permite tratarlas de forma genérica o específica según convenga en los bloques `catch`.

En términos de **encapsulación**, esta naturaleza de objeto permite que la excepción transporte consigo toda la información relevante del error sin "ensuciar" el valor de retorno de la función. Un objeto de excepción puede contener mensajes de texto, códigos de error numéricos, el estado de las variables en el momento del fallo e incluso la traza completa de la pila de llamadas (*stack trace*), manteniendo todos estos detalles empaquetados y protegidos dentro del objeto.

Por supuesto, es posible y muy recomendable **crear excepciones personalizadas**. Para ello, simplemente se debe crear una nueva clase que herede de `Exception` (para excepciones que el compilador obliga a revisar) o de `RuntimeException` (para errores de lógica). Esto permite al programador definir errores con nombres que tengan sentido en el dominio de su aplicación, como `SaldoInsuficienteException` o `UsuarioNoEncontradoException`.

Crear excepciones propias mejora la legibilidad del código, ya que quien llama a la función sabe exactamente qué tipo de problema de "negocio" ha ocurrido, en lugar de recibir un error genérico de Java. Además, al ser una clase propia, se le pueden añadir métodos o atributos adicionales para ayudar a la recuperación del error, manteniendo la coherencia con los principios de la orientación a objetos.

```java
// Ejemplo de excepción personalizada
public class NumeroNegativoException extends RuntimeException {
    public NumeroNegativoException(double valor) {
        super("El valor " + valor + " no es válido para esta operación.");
    }
}

```


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

En el ejemplo de C, cuando se detecta un error, el manejador solo recibe un número o un puntero nulo. No hay rastro de qué función llamó a cuál, ni en qué línea exacta falló el programa. El objeto **excepción en Java**, en cambio, encapsula una "fotografía" del estado del sistema en el instante del error, lo que permite una depuración mucho más precisa.

La información más valiosa que transporta es el **StackTrace** (traza de la pila). Este es un registro detallado de toda la jerarquía de llamadas que estaban activas: desde el `main`, pasando por las funciones intermedias, hasta llegar a la línea exacta de la clase `Calculadora` donde saltó el error. Sin esto, en proyectos grandes, sería casi imposible adivinar qué camino tomó el código para llegar al fallo.

Además, el objeto incluye un **mensaje descriptivo** y, opcionalmente, la **causa original** (si una excepción provocó otra). Al ser un objeto, el manejador en el `catch` puede interrogarlo mediante métodos como `e.getMessage()` o `e.printStackTrace()`. Esto permite que el programador registre en un archivo de log no solo que "algo falló", sino el contexto completo del problema, facilitando su corrección posterior.

Finalmente, gracias a la herencia, el objeto lleva consigo su propia **identidad de tipo**. Esto permite que un solo bloque de código pueda distinguir automáticamente entre un error de red, uno de base de datos o uno de lógica matemática (`IllegalArgumentException`), simplemente verificando la clase del objeto recibido. En C, esto requeriría complejas estructuras de control o códigos de error globales difíciles de gestionar.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

