
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
