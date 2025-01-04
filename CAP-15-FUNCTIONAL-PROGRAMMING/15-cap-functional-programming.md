
## Working with Built‐in Functional Interfaces
En la Tabla 14.1, presentamos algunas interfaces funcionales básicas que utilizamos con el Collections Framework. Ahora las estudiaremos con más detalle y de manera más profunda. Como se explicó en el Capítulo 12, una interfaz funcional tiene exactamente un método abstracto. Nos centraremos en ese método aquí.

Todas las interfaces funcionales de la Tabla 15.1 se encuentran en el paquete java.util.function. La convención es usar el tipo genérico T como parámetro de tipo. Si se necesita un segundo parámetro de tipo, se utiliza la siguiente letra, U. Si se requiere un tipo de retorno distinto, se emplea R (de return) para el tipo genérico.


| Functional interface     | Return type | Method name    | # of parameters |
|--------------------------|------------|----------------|-----------------|
| `Supplier<T>`            | `T`        | `get()`        | 0               |
| `Consumer<T>`            | `void`     | `accept(T)`    | 1 (T)           |
| `BiConsumer<T, U>`       | `void`     | `accept(T,U)`  | 2 (T, U)        |
| `Predicate<T>`           | `boolean`  | `test(T)`      | 1 (T)           |
| `BiPredicate<T, U>`      | `boolean`  | `test(T,U)`    | 2 (T, U)        |
| `Function<T, R>`         | `R`        | `apply(T)`     | 1 (T)           |
| `BiFunction<T, U, R>`    | `R`        | `apply(T,U)`   | 2 (T, U)        |
| `UnaryOperator<T>`       | `T`        | `apply(T)`     | 1 (T)           |
| `BinaryOperator<T>`      | `T`        | `apply(T,T)`   | 2 (T, T)        |


Hay una interfaz funcional aquí que no estaba en la Tabla 14.1 (BinaryOperator). Solo presentamos lo que necesitabas en el Capítulo 14 en ese momento. Incluso la Tabla 15.1 es un subconjunto de lo que necesitas saber. Muchas interfaces funcionales están definidas en el paquete `java.util.function`. De hecho, hay interfaces funcionales para manejar tipos primitivos, que verás más adelante en el capítulo.

Si bien necesitas conocer muchas interfaces funcionales para el examen, por suerte muchas comparten nombres con las que aparecen en la Tabla 15.1. Teniendo esto en cuenta, debes memorizar la Tabla 15.1. Te daremos mucha práctica en esta sección para ayudarte a recordarla. Antes de que preguntes, la mayoría de las veces no asignamos realmente la implementación de la interfaz a una variable. El nombre de la interfaz está implícito y se pasa directamente al método que la necesita. Presentamos estos nombres para que puedas entender y recordar mejor lo que está sucediendo. Una vez que lleguemos a la sección de flujos (*streams*) de este capítulo, asumiremos que ya comprendes todo esto y dejaremos de crear la variable intermedia.

Como viste en el Capítulo 12, puedes nombrar una interfaz funcional como quieras. Los únicos requisitos son que sea un nombre de interfaz válido y que contenga un único método abstracto. La Tabla 15.1 es importante porque estas interfaces se utilizan con frecuencia en los streams y en otras clases que vienen con Java, razón por la cual es necesario memorizarlas para el examen.

> **Note:** Como aprenderás en el Capítulo 18, "Concurrencia", existen dos interfaces funcionales más llamadas Runnable y Callable, que necesitas conocer para el examen. Se utilizan principalmente para la concurrencia. No obstante, pueden aparecer en el examen cuando te pidan reconocer qué interfaz funcional usar. Lo único que necesitas saber es que Runnable y Callable no reciben parámetros; Runnable devuelve void y Callable devuelve un tipo genérico.

Veamos cómo implementar cada una de estas interfaces. Dado que tanto las lambdas como las referencias a métodos aparecen en gran parte del examen, mostraremos una implementación con ambas cuando sea posible. Después de presentar las interfaces, también cubriremos algunos métodos de conveniencia disponibles en ellas.

## IMPLEMENTING SUPPLIER

Un Supplier se utiliza cuando deseas generar o proporcionar valores sin recibir ninguna entrada. La interfaz Supplier se define de la siguiente manera:

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```

Puedes crear un objeto LocalDate utilizando el método de fábrica now(). Este ejemplo muestra cómo usar un Supplier para llamar a este método de fábrica:

```java
Supplier<LocalDate> s1 = LocalDate::now;
Supplier<LocalDate> s2 = () -> LocalDate.now();

LocalDate d1 = s1.get();
LocalDate d2 = s2.get();

System.out.println(d1);
System.out.println(d2);
```

Este ejemplo imprime una fecha como 2020–02–20 dos veces. También es una buena oportunidad para repasar las referencias a métodos estáticos. La referencia al método LocalDate::now se utiliza para crear un Supplier y asignarlo a una variable intermedia s1. Un Supplier se usa con frecuencia cuando se construyen nuevos objetos. Por ejemplo, podemos imprimir dos objetos StringBuilder vacíos:

```java
Supplier<StringBuilder> s1 = StringBuilder::new;
Supplier<StringBuilder> s2 = () -> new StringBuilder();
System.out.println(s1.get());
System.out.println(s2.get());
```

Esta vez, usamos una referencia al constructor para crear el objeto. Hemos estado usando genéricos para declarar qué tipo de Supplier estamos utilizando. Esto puede volverse un poco extenso de leer. ¿Puedes darte cuenta de lo que hace el siguiente fragmento? Solo tómalo paso a paso.

```java
Supplier<ArrayList<String>> s3 = ArrayList<String>::new;
ArrayList<String> a1 = s3.get();
System.out.println(a1);
```
Tenemos un Supplier de un cierto tipo. Ese tipo resulta ser ArrayList<String>. Luego, al llamar a get(), se crea una nueva instancia de ArrayList<String>, que es el tipo genérico del Supplier, es decir, un genérico que contiene otro genérico. No es difícil de entender; solo debes revisar el código con atención cuando aparezca este tipo de situación.

Observa cómo llamamos a get() en la interfaz funcional. ¿Qué sucedería si tratáramos de imprimir s3 directamente?

```java
System.out.println(s3);
```

El código imprime algo como esto:
```java
functionalinterface.BuiltIns$$Lambda$1/0x0000000800066840@4909b8da
```

Ese es el resultado de llamar a `toString()` en una lambda. ¡Puaj! De hecho, esto sí significa algo. Nuestra clase de prueba se llama `BuiltIns` y está en un paquete que creamos llamado `functionalinterface`. Luego aparece `$$`, lo que significa que la clase no existe en un archivo `.class` en el sistema de archivos, sino que existe solo en memoria. No tienes que preocuparte por el resto.

## IMPLEMENTING CONSUMER AND BICONSUMER

Usas un `Consumer` cuando quieres hacer algo con un parámetro, pero sin devolver nada. `BiConsumer` hace lo mismo, excepto que toma dos parámetros. Las interfaces se definen de la siguiente manera:

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
    // omitted default method
}

@FunctionalInterface
public interface BiConsumer<T, U> {
    void accept(T t, U u);
    // omitted default method
}
```

> **Note:** Te darás cuenta de este patrón. “Bi” significa “dos”. Proviene del latín, pero puedes recordarlo con palabras en inglés como *binary* (0 o 1) o *bicycle* (dos ruedas). Siempre agrega otro parámetro cuando veas que aparece “Bi”.

Usaste un `Consumer` en el Capítulo 14 con `forEach()`. Aquí tienes el ejemplo asignado realmente a la interfaz `Consumer`:

```java
Consumer<String> c1 = System.out::println;
Consumer<String> c2 = x -> System.out.println(x);
c1.accept("Annie");
c2.accept("Annie");
```

Este ejemplo imprime `Annie` dos veces. `BiConsumer` se llama con dos parámetros. No tienen que ser del mismo tipo. Por ejemplo, podemos colocar una clave y un valor en un mapa usando esta interfaz:

```java
var map = new HashMap<String, Integer>();
BiConsumer<String, Integer> b1 = map::put;
BiConsumer<String, Integer> b2 = (k, v) -> map.put(k, v);
b1.accept("chicken", 7);
b2.accept("chick", 1);
System.out.println(map);
```

La salida es `{chicken=7, chick=1}`, lo cual muestra que se llamaron ambas implementaciones de `BiConsumer`. Al declarar `b1`, usamos una referencia a un método de instancia sobre un objeto, porque queremos llamar a un método en la variable local `map`. El código para instanciar `b1` es bastante más corto que el de `b2`. Probablemente por eso al examen le gustan tanto las referencias a métodos.

Como otro ejemplo, usamos el mismo tipo para ambos parámetros genéricos:

```java
var map = new HashMap<String, String>();
BiConsumer<String, String> b1 = map::put;
BiConsumer<String, String> b2 = (k, v) -> map.put(k, v);
b1.accept("chicken", "Cluck");
b2.accept("chick", "Tweep");
System.out.println(map);
```

La salida es `{chicken=Cluck, chick=Tweep}`, lo que demuestra que un `BiConsumer` puede usar el mismo tipo para ambos parámetros genéricos `T` y `U`.

## IMPLEMENTING PREDICATE AND BIPREDICATE
Has visto `Predicate` con `removeIf()` en el Capítulo 14. `Predicate` se usa a menudo al filtrar o al hacer coincidencias. Ambas son operaciones comunes. Un `BiPredicate` es igual que un `Predicate`, excepto que toma dos parámetros en lugar de uno. Las interfaces se definen de la siguiente manera:

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
    // omitted default and static methods
}

@FunctionalInterface
public interface BiPredicate<T, U> {
    boolean test(T t, U u);
    // omitted default methods
}
```

Debe ser ya noticia antigua que puedes usar un `Predicate` para probar una condición.

```java
Predicate<String> p1 = String::isEmpty;
Predicate<String> p2 = x -> x.isEmpty();
System.out.println(p1.test("")); // true
System.out.println(p2.test("")); // true
```

Esto imprime `true` dos veces. Más interesante es un `BiPredicate`. Este ejemplo también imprime `true` dos veces:

```java
BiPredicate<String, String> b1 = String::startsWith;
BiPredicate<String, String> b2 =
    (string, prefix) -> string.startsWith(prefix);
System.out.println(b1.test("chicken", "chick")); // true
System.out.println(b2.test("chicken", "chick")); // true
```

La referencia al método incluye tanto la variable de instancia como el parámetro para `startsWith()`. Este es un buen ejemplo de cómo las referencias a métodos ahorran bastante tipeo. La desventaja es que son menos explícitas, y realmente tienes que entender lo que está sucediendo.

## IMPLEMENTING FUNCTION AND BIFUNCTION

En el Capítulo 14, utilizamos `Function` con el método `merge()`. Una `Function` es responsable de convertir un parámetro en un valor de un tipo potencialmente diferente y devolverlo. De manera similar, una `BiFunction` es responsable de convertir dos parámetros en un valor y devolverlo. Las interfaces se definen de la siguiente manera:

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
    // omitted default and static methods
}

@FunctionalInterface
public interface BiFunction<T, U, R> {
    R apply(T t, U u);
    // omitted default method
}
```

Por ejemplo, esta función convierte una `String` en la longitud de la `String`:

```java
Function<String, Integer> f1 = String::length;
Function<String, Integer> f2 = x -> x.length();
System.out.println(f1.apply("cluck")); // 5
System.out.println(f2.apply("cluck")); // 5
```

Esta función convierte una `String` en un `Integer`. Bueno, técnicamente convierte la `String` en un `int`, que se autoboxea en un `Integer`. Los tipos no tienen que ser diferentes. Lo siguiente combina dos objetos `String` y produce otra `String`:

```java
BiFunction<String, String, String> b1 = String::concat;
BiFunction<String, String, String> b2 =
    (string, toAdd) -> string.concat(toAdd);
System.out.println(b1.apply("baby ", "chick")); // baby chick
System.out.println(b2.apply("baby ", "chick")); // baby chick
```

Los dos primeros tipos en la `BiFunction` son los tipos de entrada. El tercero es el tipo de resultado. Para la referencia al método, el primer parámetro es la instancia sobre la que se llama a `concat()`, y el segundo se pasa a `concat()`.

CREANDO TUS PROPIAS INTERFACES FUNCIONALES

Java proporciona una interfaz incorporada para funciones con uno o dos parámetros. ¿Qué pasa si necesitas más? No hay problema. Supongamos que quieres crear una interfaz funcional para la velocidad de las ruedas de cada rueda en un triciclo. Podrías crear una interfaz funcional como esta:

```java
@FunctionalInterface
interface TriFunction<T, U, V, R> {
    R apply(T t, U u, V v);
}
```

Hay cuatro parámetros de tipo. Los primeros tres proporcionan los tipos de las tres velocidades de las ruedas. El cuarto es el tipo de retorno. Ahora supongamos que quieres crear una función para determinar qué tan rápido va tu cuadricóptero dado el poder de los cuatro motores. Podrías crear una interfaz funcional como la siguiente:

```java
@FunctionalInterface
interface QuadFunction<T, U, V, W, R> {
    R apply(T t, U u, V v, W w);
}
```

Aquí hay cinco parámetros de tipo. Los primeros cuatro proporcionan los tipos de los cuatro motores. Idealmente, estos serían del mismo tipo, pero nunca se sabe. El quinto es el tipo de retorno en este ejemplo.

Las interfaces incorporadas de Java están destinadas a facilitar las interfaces funcionales más comunes que necesitarás. De ninguna manera es una lista exhaustiva. Recuerda que puedes agregar las interfaces funcionales que desees, y Java las empareja cuando usas lambdas o referencias a métodos.




























