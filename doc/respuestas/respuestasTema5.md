# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

Polimorfismo --> Objeticvo : facilitar la extensión de los programas.
Facilita que esta extensión se haga creando código nuevo frente a editar código existente.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta


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

### Respuesta


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Override --> le pide al compilador que compruebe que se está sobreescribiendo lo que queremos.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Sí

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

final en clases --> prohíbe heredar
final en métodos --> prohíbe sobreescribir

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

### Respuesta