<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.


### La **encapsulación** y la **ocultación** buscan proteger la integridad de los datos, asegurando que el estado interno de un objeto solo sea accesible y modificable a través de una interfaz controlada. En lugar de permitir el acceso libre a las variables (como en un `struct` de C), se crea un perímetro de seguridad que impide estados inconsistentes o erróneos causados por agentes externos.

### El objetivo final es el **desacoplamiento**: que el resto del programa no dependa de *cómo* están implementados los datos, sino solo de *qué* funciones realiza el objeto. Esto facilita enormemente la evolución del sistema, ya que se pueden realizar cambios internos sin afectar a los usuarios de la clase.

### Las ventajas principales son:

### * **Protección de datos:** Impide modificaciones accidentales o valores inválidos.
### * **Modularidad:** Permite cambiar la implementación interna sin romper el código externo.
### * **Simplicidad:** Reduce la complejidad al mostrar solo lo necesario para operar el objeto.
### * **Control de acceso:** Permite definir datos como solo lectura o solo escritura de forma selectiva.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### La **interfaz pública** de una clase es el conjunto de métodos y constantes que se definen con el modificador `public`. Representa el "contrato" o el manual de instrucciones que el objeto ofrece al mundo exterior para que otros componentes puedan interactuar con él. A diferencia de un archivo de cabecera en C, donde se exponen firmas de funciones y estructuras, la interfaz pública en Java define estrictamente qué acciones están permitidas sobre los datos ocultos.

### Su relación con la **ocultación de información** es de complementariedad: mientras la ocultación "esconde" los detalles técnicos y los atributos (el *cómo*), la interfaz pública "muestra" las capacidades funcionales (el *qué*). Es el único punto de contacto autorizado; todo lo que no forme parte de esta interfaz permanece inaccesible, garantizando que el usuario del objeto no necesite conocer su complejidad interna para utilizarlo correctamente.

### Esta separación permite que la interfaz permanezca estable mientras la implementación interna evoluciona. Si se modifica la lógica de un método o el tipo de una variable privada, el resto del programa no se ve afectado siempre que la firma del método público se mantenga igual, logrando así un sistema mucho más flexible y fácil de mantener.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Diseñar con cuidado la **interfaz pública** es crucial porque representa un compromiso permanente con otros programadores o módulos del sistema. Una vez que un método se marca como público, otros componentes empezarán a depender de su nombre, sus parámetros y su comportamiento. Si se expone demasiado o se diseña de forma descuidada, se corre el riesgo de "filtrar" detalles de implementación que deberían haber sido privados, limitando la libertad futura para mejorar la clase.

### No es fácil cambiar una interfaz pública una vez que el sistema está en uso o en producción. Si se modifica la firma de un método público (por ejemplo, cambiando el tipo de un parámetro o eliminando el método), se provocará un error de compilación en todas las partes del programa que lo utilizaban. Esto es similar a cambiar la firma de una función en una librería compartida en C: obliga a refactorizar y recompilar todo el código dependiente, lo cual es costoso y propenso a errores.

### Por ello, se recomienda seguir el principio de **exposición mínima**: publicar solo aquello que sea estrictamente necesario. Es sencillo convertir un método privado en público más adelante si se requiere, pero el proceso inverso (hacer privado algo que antes era público) suele ser traumático para la arquitectura del software, ya que implica romper la compatibilidad con el resto del código existente.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Las **invariantes de clase** son condiciones o reglas lógicas que deben cumplirse siempre para que un objeto se considere válido. Se pueden comparar con las restricciones de integridad en una base de datos o con las suposiciones de seguridad en una estructura de C; por ejemplo, en una clase `Fecha`, una invariante sería que el valor del mes siempre esté en el rango . Estas reglas deben ser ciertas desde que el objeto termina de construirse hasta que es destruido.

### La ocultación de información es la herramienta fundamental para proteger estas invariantes. Al marcar los atributos como `private`, se impide que un código externo asigne valores que rompan la lógica del objeto (como asignar el mes 13). Al centralizar todas las modificaciones en métodos específicos, la clase puede actuar como un "filtro" que valida cada cambio antes de aplicarlo, asegurando que ninguna operación deje al objeto en un estado inconsistente.

### Sin ocultación de información, el programador tendría que confiar en que todas las partes del sistema respeten las reglas de los datos de forma manual, lo cual es propenso a errores humanos. En Java, la encapsulación garantiza que, si los métodos de la clase están bien programados, es físicamente imposible que el objeto adopte un estado inválido, independientemente de quién o cómo se use la clase desde el exterior.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Para implementar la clase `Punto` siguiendo los principios de la POO, se declaran los atributos como privados y los métodos necesarios como públicos. Esto asegura que el estado interno del punto no sea manipulado directamente desde el exterior.

#### ```java
#### public class Punto {
####    // Atributos privados: no accesibles desde fuera de la clase
####    private double x;
####    private double y;
####
####    // Constructor para inicializar el objeto
####    public Punto(double x, double y) {
####        this.x = x;
####        this.y = y;
####    }
####
####    // Método público para calcular la distancia al origen (0,0)
####    public double calcularDistanciaAOrigen() {
####        return Math.sqrt(x * x + y * y);
####    }
####
####    // Getters para permitir el acceso controlado a los datos
####    public double getX() { return x; }
####    public double getY() { return y; }
#### }
####
#### ```

### La **interfaz pública** de esta clase está compuesta por el constructor `Punto(double x, double y)`, el método `calcularDistanciaAOrigen()`, y los métodos `getX()` e `getY()`. Estos son los únicos "puntos de contacto" que un programador tiene disponibles para interactuar con un objeto de tipo `Punto`. A diferencia de C, no se puede hacer algo como `punto.x = 10;`, obligando a usar la interfaz definida.

### En este contexto, **`private`** actúa como un muro de privacidad: indica que las variables `x` e `y` solo existen y son visibles dentro de las llaves de la clase `Punto`. Por el contrario, **`public`** funciona como una puerta abierta: permite que cualquier otra clase del programa pueda invocar esos métodos para obtener información o realizar cálculos, sin necesidad de saber cómo están almacenados los datos.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
