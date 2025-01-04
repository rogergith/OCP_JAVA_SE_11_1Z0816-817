
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
