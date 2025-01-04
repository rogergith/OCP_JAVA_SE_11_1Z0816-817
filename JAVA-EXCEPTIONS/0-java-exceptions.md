A continuación encontrarás una guía sencilla sobre cómo crear **excepciones personalizadas** en Java (versión 11 o superior) y cómo aplicarlas en situaciones de la vida real dentro de programas.

---

## 1. ¿Por qué crear excepciones personalizadas?

- **Claridad**: Las excepciones personalizadas permiten describir de forma más precisa los errores que ocurren en tu aplicación.
- **Mantenibilidad**: Facilitan la lectura y el mantenimiento del código, ya que puedes capturar y manejar errores específicos de tu dominio (bancario, logística, reservas de vuelos, etc.).
- **Desacoplamiento**: Evitan el uso excesivo de excepciones genéricas (`Exception`, `RuntimeException`) que dificultan identificar la causa exacta de los problemas.

---

## 2. Excepciones **Checked** vs. **Unchecked**

1. **Excepciones Checked** (`extends Exception`)
    - Deben declararse en la firma del método con `throws` o manejarse en un bloque `try/catch`.
    - Se utiliza para situaciones en las que el programa “espera” que pueda ocurrir un error y quiere forzar al desarrollador a manejarlo.
    - Ejemplo común: `IOException`, `SQLException`.

2. **Excepciones Unchecked** (`extends RuntimeException`)
    - No requieren declaración en la firma del método. Pueden manejarse o no en tiempo de ejecución.
    - Se usan para errores de programación, como acceso a un índice fuera de rango, valores nulos inesperados, etc.
    - Ejemplo común: `NullPointerException`, `IndexOutOfBoundsException`.

---

## 3. Ejemplo de Creación de Excepciones Personalizadas

### 3.1 Excepción Checked

Supongamos que en un sistema bancario queremos lanzar una excepción cuando no hay suficiente saldo en la cuenta para realizar un retiro.

```java
// Archivo: InsufficientFundsException.java
public class InsufficientFundsException extends Exception {

    public InsufficientFundsException() {
        super();
    }

    public InsufficientFundsException(String message) {
        super(message);
    }

    public InsufficientFundsException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

- `InsufficientFundsException` **extiende** de `Exception`, por lo que es **checked**.
- El constructor sin parámetros y los constructores con `message` y `cause` permiten personalizar el mensaje y enlazar con otra excepción, si existiera.

### 3.2 Excepción Unchecked

Podemos crear una excepción para un error de validación que indica que el usuario ingresó un valor inválido en un formulario. Por ejemplo, un nombre con caracteres no permitidos.

```java
// Archivo: InvalidNameException.java
public class InvalidNameException extends RuntimeException {

    public InvalidNameException() {
        super();
    }

    public InvalidNameException(String message) {
        super(message);
    }

    public InvalidNameException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

- `InvalidNameException` **extiende** de `RuntimeException`, por lo que es **unchecked**.
- Puedes usarla cuando un usuario ingresa datos inválidos y deseas reportar el error sin obligar a manejarlo de forma explícita (aunque puedes hacerlo con `try/catch` si lo deseas).

---

## 4. Cómo Lanzar y Manejar Excepciones Personalizadas

### 4.1 Lanzar la excepción (throw)

Imaginemos una clase `BankAccount` donde realizamos operaciones de depósito y retiro:

```java
public class BankAccount {
    private double balance;

    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    // Excepción checked: Debemos declarar throws en el método.
    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount > balance) {
            throw new InsufficientFundsException(
                "No hay fondos suficientes para retirar " + amount
            );
        }
        balance -= amount;
    }

    public double getBalance() {
        return balance;
    }
}
```

En este caso, **InsufficientFundsException** es una excepción checked, así que el método `withdraw` la declara con `throws`.

### 4.2 Manejar la excepción (try/catch)

Un método que use `BankAccount` podría lucir así:

```java
public class BankService {

    public static void main(String[] args) {
        BankAccount account = new BankAccount(100);

        try {
            account.withdraw(150); 
        } catch (InsufficientFundsException e) {
            System.out.println("Ocurrió un error: " + e.getMessage());
            // Aquí podríamos notificar al usuario, registrar en log, etc.
        }
    }
}
```

Al ser **InsufficientFundsException** checked, el compilador **obliga** a manejarla (ya sea con `try/catch` o propagarla con `throws`).

Para el caso de `InvalidNameException` (unchecked), no estaríamos obligados a declarar `throws`. Sin embargo, podríamos capturarla igualmente:

```java
public void registerUser(String username) {
    if (!isNameValid(username)) {
        throw new InvalidNameException("El nombre contiene caracteres no válidos.");
    }
    // Lógica de registro
}
```

Si quisiéramos manejarla:

```java
try {
    registerUser("$pedro!");
} catch (InvalidNameException e) {
    System.out.println("Nombre no válido: " + e.getMessage());
}
```

---

## 5. Aplicaciones de las Excepciones en la Vida Real

### 5.1 Bancos y Finanzas

- **InsufficientFundsException**: Cuando un cliente intenta retirar más dinero del que tiene.
- **OverdraftLimitExceededException**: Si existe un límite de sobregiro y se supera.

### 5.2 Sistemas de Reservas (Vuelos, Hoteles)

- **SeatNotAvailableException**: Asiento ocupado o no disponible.
- **InvalidBookingDateException**: Fechas de reserva fuera de rango.

### 5.3 E-commerce

- **OutOfStockException**: Producto sin existencia.
- **PaymentDeclinedException**: Pago rechazado por la pasarela de pago.

### 5.4 Aplicaciones Web

- **InvalidCredentialsException**: El usuario ingresó credenciales incorrectas.
- **UnauthorizedActionException**: El usuario no tiene permisos para realizar cierta acción.

En todos estos casos, las excepciones personalizadas **describen** de manera clara el problema que ocurrió, permiten **reaccionar** adecuadamente (por ejemplo, devolviendo un mensaje al usuario) y facilitan el **mantenimiento** del código al tener tipos de error bien definidos.

---

## 6. Conclusiones

1. **Excepciones personalizadas** añaden claridad a tu código y facilitan la depuración y el manejo de errores.
2. **Excepciones checked** (`extends Exception`) obligan a un manejo explícito, mientras que las **unchecked** (`extends RuntimeException`) no.
3. En la **vida real**, se usan para modelar errores específicos de la lógica de negocio, lo que mejora la calidad y la mantenibilidad del software.
4. Piensa siempre en si tu excepción debe ser checked o unchecked, según la naturaleza y criticidad del error.

Con esto ya tienes las bases para empezar a crear y manejar tus propias excepciones en Java 11 (y versiones posteriores). ¡Practica e impleméntalas de forma que tu código sea más robusto y fácil de entender!