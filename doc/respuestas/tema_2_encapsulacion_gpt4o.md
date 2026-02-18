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

### En Java, los modificadores de acceso `public` y `private` se utilizan para controlar la visibilidad de los elementos que forman una clase. Estos modificadores permiten definir qué partes del programa pueden acceder a determinados componentes, estableciendo así el nivel de ocultación de la información y reforzando la encapsulación.

### El modificador `public` puede aplicarse a **clases**, **atributos** (variables miembro), **métodos** y **constructores**. Cuando un elemento se declara como `public`, puede ser utilizado desde cualquier otra clase del programa, incluso si se encuentra en otro paquete. Esto indica que forma parte de la interfaz visible de la clase y que está pensado para ser usado desde el exterior.

### El modificador `private`, en cambio, solo puede aplicarse a **atributos**, **métodos** y **constructores**, pero no a clases de nivel superior. Un elemento `private` solo es accesible desde dentro de la propia clase donde se declara. Esto impide que otras clases accedan directamente a esos componentes, obligando a interactuar con ellos mediante métodos públicos definidos por la clase.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### En Programación Orientada a Objetos, además de la visibilidad **pública** y **privada**, existen otros niveles intermedios que permiten un control más fino sobre qué partes del programa pueden acceder a los miembros de una clase. Estos niveles intermedios buscan equilibrar la ocultación de información con la necesidad de compartir ciertos elementos entre clases relacionadas, sin exponerlos completamente al resto del programa.

### En Java, además de `public` y `private`, existen dos niveles adicionales: la visibilidad **por defecto** (también llamada *package-private*) y la visibilidad **protegida** (`protected`). La visibilidad por defecto permite que los miembros sean accesibles solo desde clases que pertenecen al mismo paquete. Por su parte, `protected` permite el acceso desde el mismo paquete y también desde clases que heredan de esa clase, aunque se encuentren en otros paquetes.

### En otros lenguajes orientados a objetos también aparecen niveles similares. Por ejemplo, en C++ existe el modificador `protected`, y además se pueden definir clases y miembros como `public`, `private` o `protected` dentro de una misma clase. En lenguajes como C# aparecen incluso más variantes, como `internal`, que limita el acceso al mismo ensamblado. Esto muestra que la gestión de la visibilidad es un aspecto común y relevante en la mayoría de lenguajes orientados a objetos.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Los miembros de instancia declarados como private están ocultos para otras clases, pero no para otras instancias de la misma clase. Es decir, cualquier objeto puede acceder a los atributos privados de otro objeto siempre que ambos pertenezcan a la misma clase. Esto ocurre porque la restricción private se aplica a nivel de clase, no a nivel de objeto individual.

### En Java, cuando se define un método dentro de una clase, dicho método tiene acceso completo a todos los atributos privados de cualquier instancia de esa misma clase. Esto puede resultar llamativo desde una perspectiva similar a C con estructuras, donde no existe este tipo de restricción por clase. Sin embargo, en POO se entiende que la clase es la unidad que controla el acceso a sus datos, por lo que todos sus métodos pueden operar sobre cualquier objeto de su mismo tipo.

#### public class Punto {
####    private double x;
####    private double y;
####
####    public Punto(double x, double y) {
####        this.x = x;
####        this.y = y;
####    }
####
####    public double calcularDistanciaAPunto(Punto otro) {
####        double dx = this.x - otro.x;  // Acceso al atributo privado de otro objeto
####        double dy = this.y - otro.y;  // También permitido
####        return Math.sqrt(dx * dx + dy * dy);
####    }
#### }

### En este ejemplo, el método calcularDistanciaAPunto accede directamente a otro.x y otro.y, a pesar de que son atributos private. Esto es posible porque el acceso se realiza desde dentro de la misma clase Punto. Por tanto, la respuesta correcta es que los miembros privados están ocultos para otras clases, pero no para otras instancias de la misma clase.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### En los lenguajes orientados a objetos, los métodos getter y setter son métodos públicos que permiten acceder y modificar, de forma controlada, los atributos privados de una clase. Se utilizan como parte de la técnica de encapsulación para evitar que los datos internos del objeto se manipulen directamente desde el exterior. En lugar de acceder al atributo, se invoca un método que gestiona ese acceso.

### Un método getter se encarga de devolver el valor de un atributo, mientras que un método setter permite asignar un nuevo valor a dicho atributo. Aunque su funcionamiento puede parecer similar a acceder directamente a una variable, la diferencia es que estos métodos permiten introducir validaciones, restricciones o acciones adicionales antes de devolver o modificar el valor. De este modo, la clase mantiene el control sobre su estado interno.

#### public class Persona {
####    private int edad;
####
####    public int getEdad() {
####        return edad;
####    }
####
####    public void setEdad(int edad) {
####        if (edad >= 0) {
####            this.edad = edad;
####        }
####    }
#### }

### En este ejemplo, el atributo edad es privado y solo puede leerse o modificarse mediante los métodos getEdad y setEdad. Esto permite, por ejemplo, impedir que se asigne una edad negativa, algo que no sería posible si el atributo fuese público.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Cuando se afirma que la ocultación de información mejora la “seguridad” del programa, no se está haciendo referencia a la seguridad frente a ataques externos, hackers o problemas de ciberseguridad. El término “seguridad” en este contexto se utiliza en un sentido más interno, relacionado con la fiabilidad y el uso correcto del código dentro del propio programa.

### La ocultación de información evita que otras partes del programa accedan directamente a los datos internos de un objeto y los modifiquen de forma inadecuada. Al obligar a utilizar métodos controlados por la clase, se reduce la posibilidad de cometer errores de programación que dejen al objeto en un estado inconsistente. Por tanto, se protege la coherencia interna del objeto frente a usos incorrectos por parte de otros programadores o de otras clases.

### Esta “seguridad” se entiende como una protección frente a errores lógicos y malas prácticas en el desarrollo del software, no como una barrera frente a accesos malintencionados desde el exterior. Se trata de garantizar que el objeto solo pueda ser utilizado de la manera prevista por su diseño, aumentando la robustez y mantenibilidad del programa.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Un **miembro de instancia** es aquel que pertenece a cada objeto creado a partir de una clase. Esto significa que cada instancia tiene su propia copia de ese atributo o método, y su valor puede ser distinto en cada objeto. En Java, los atributos normales y los métodos que operan sobre ellos son miembros de instancia, ya que trabajan con el estado particular de cada objeto.

### Un **miembro de clase**, en cambio, es compartido por todas las instancias de esa clase. En Java se declaran con la palabra clave `static`, y existe una única copia de ese miembro, independientemente del número de objetos creados. Este tipo de miembros se utiliza cuando la información o el comportamiento no depende de un objeto concreto, sino que es común a toda la clase.

### Los miembros de clase también pueden ocultarse utilizando los modificadores de acceso como `private` o `public`. Aunque sean `static`, siguen perteneciendo a la clase y, por tanto, se les puede aplicar encapsulación igual que a los miembros de instancia. De este modo, se puede impedir que otras clases accedan directamente a ellos y obligar a que su uso se realice a través de métodos controlados.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Sí, tiene sentido que los constructores sean `private` en determinados diseños. Aunque un constructor suele utilizarse para permitir la creación de objetos desde el exterior, declararlo como privado impide que otras clases puedan instanciar directamente esa clase. De este modo, se controla estrictamente cómo y cuándo se crean los objetos.

### Esta técnica se utiliza cuando se desea que la creación de instancias se realice a través de métodos estáticos de la propia clase. Así, la clase puede decidir si devuelve siempre el mismo objeto, si limita el número de instancias creadas o si realiza comprobaciones antes de permitir la creación. Un caso típico es el patrón conocido como *Singleton*, donde solo debe existir un único objeto de esa clase.

### Por tanto, un constructor privado no impide el uso de la clase, sino que obliga a que la creación de objetos esté completamente controlada por ella misma, reforzando la encapsulación y el control sobre su uso.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### En Java, los **miembros de clase** se indican mediante la palabra clave `static`. Esto significa que dichos atributos o métodos no pertenecen a un objeto concreto, sino a la clase en sí misma. Por tanto, existe una única copia compartida por todas las instancias creadas a partir de esa clase. Se accede a ellos utilizando el nombre de la clase en lugar del nombre de un objeto.

### Este tipo de miembros resulta útil cuando se desea almacenar información común a todos los objetos. En este caso, se puede utilizar para registrar los valores máximos de `x` e `y` que se han ido asignando a los distintos puntos creados. Cada vez que se construye un nuevo objeto `Punto`, se pueden actualizar estos valores estáticos si el nuevo punto supera los máximos anteriores.

#### public class Punto {
####    private double x;
####    private double y;
####
####    private static double maxX = Double.MIN_VALUE;
####    private static double maxY = Double.MIN_VALUE;
####
####    public Punto(double x, double y) {
####        this.x = x;
####        this.y = y;
####
####        if (x > maxX) {
####            maxX = x;
####        }
####        if (y > maxY) {
####            maxY = y;
####        }
####    }
####
####    public static double getMaxX() {
####        return maxX;
####    }
####
####    public static double getMaxY() {
####        return maxY;
####    }
#### }

#### En este ejemplo, `maxX` y `maxY` son miembros de clase porque se declaran como `static`. Todos los objetos `Punto` comparten estos valores y los actualizan al crearse. Los métodos `getMaxX` y `getMaxY` también son estáticos, ya que permiten consultar esta información sin necesidad de disponer de un objeto concreto.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

#### public static Punto crearPuntoRedondeado(double x, double y) {
####    int xr = (int) Math.round(x);
####    int yr = (int) Math.round(y);
####    return new Punto(xr, yr);
#### }

### Sí, se ha utilizado `static`. Un método factoría debe ser miembro de clase para poder invocarse sin disponer previamente de un objeto `Punto`, ya que precisamente su finalidad es crear y devolver nuevas instancias.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Se puede modificar la representación interna de la clase sin cambiar su interfaz pública gracias a la encapsulación. En lugar de almacenar las coordenadas en dos atributos `double` separados, se puede utilizar un array interno de dos posiciones. Desde el exterior, la clase seguirá ofreciendo los mismos constructores y métodos, por lo que el resto del programa no se verá afectado por este cambio.

### Este cambio demuestra una de las ventajas de la ocultación de información: la implementación interna puede alterarse sin necesidad de modificar el código que utiliza la clase. Como los atributos siguen siendo privados, ninguna otra clase depende de cómo se almacenan realmente las coordenadas.

#### public class Punto {
####    private double[] coord = new double[2];
####
####    public Punto(double x, double y) {
####        coord[0] = x;
####        coord[1] = y;
####    }
####
####    public double getX() {
####        return coord[0];
####    }
####
####    public double getY() {
####        return coord[1];
####    }
####
####    public void setX(double x) {
####        coord[0] = x;
####    }
####
####    public void setY(double y) {
####        coord[1] = y;
####    }
#### }




## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Aunque un atributo disponga de métodos `getter` y `setter` públicos, no es recomendable declararlo directamente como público. Si el atributo fuese público, cualquier parte del programa podría modificarlo sin ningún tipo de control. En cambio, mediante métodos de acceso se conserva la posibilidad de añadir validaciones, comprobaciones o lógica adicional sin cambiar la interfaz externa de la clase.

### La convención más habitual en Java y en la mayoría de lenguajes orientados a objetos es declarar los atributos como `private`. De este modo, se refuerza la encapsulación y se garantiza que el estado interno del objeto solo pueda modificarse a través de los métodos definidos por la clase. Incluso cuando el `getter` y el `setter` simplemente leen o asignan el valor sin más lógica, mantener el atributo privado permite modificar su implementación en el futuro sin afectar al código cliente.

### Esto está directamente relacionado con las **invariantes de clase**, es decir, las condiciones que deben cumplirse siempre para que un objeto se encuentre en un estado válido. Si los atributos fueran públicos, sería fácil romper esas invariantes asignando valores incorrectos. En cambio, al obligar a pasar por los métodos de la clase, se puede comprobar que cada modificación mantiene dichas condiciones, preservando la coherencia interna del objeto.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Una clase es **inmutable** cuando los objetos que se crean a partir de ella no pueden cambiar su estado una vez construidos. Es decir, después de ejecutar el constructor, los valores internos del objeto permanecen constantes durante toda su vida. Para lograrlo, normalmente los atributos se declaran como `private` y no se proporcionan métodos que permitan modificarlos, o bien se devuelven copias en lugar de referencias directas a estructuras internas.

### Un **método modificador** es cualquier método que altera el estado interno del objeto. Esto significa que cambia el valor de uno o más atributos de instancia. Aunque muchos métodos modificadores adoptan la forma típica de un `setter` (por ejemplo, `setEdad(int edad)`), no todos los métodos modificadores son necesariamente setters. Por ejemplo, un método como `incrementarContador()` o `mover(double dx, double dy)` también modifica el estado, aunque no siga el patrón clásico de asignación directa.

### Las clases inmutables presentan varias ventajas. Facilitan el razonamiento sobre el programa, ya que un objeto no cambia inesperadamente su estado. Además, reducen errores relacionados con efectos secundarios y resultan especialmente útiles en entornos concurrentes, donde varios hilos pueden acceder al mismo objeto sin necesidad de sincronización adicional. Por estas razones, la inmutabilidad suele considerarse una práctica recomendable cuando el diseño lo permite.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### No es recomendable incluir métodos `setter` siempre por mera convención. Aunque en muchos ejemplos básicos se presentan junto con los `getter`, su presencia debe responder a una necesidad real de modificar el estado del objeto. Incluir setters de forma automática puede debilitar la encapsulación, ya que permite cambios indiscriminados sobre los atributos internos.

### Si un objeto no necesita cambiar ciertos datos después de su construcción, resulta preferible no proporcionar métodos modificadores. De este modo, se protege mejor el estado interno y se pueden mantener con mayor facilidad las invariantes de clase. Además, eliminar setters innecesarios puede favorecer el diseño de clases inmutables, lo que simplifica el razonamiento sobre el comportamiento del programa.

### Por tanto, la práctica habitual no consiste en añadir setters por defecto, sino en diseñar cuidadosamente la interfaz pública de la clase. Solo deben ofrecerse aquellos métodos que realmente formen parte del comportamiento que se desea exponer, manteniendo el mayor grado posible de control sobre el estado interno.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### La clase String en Java es inmutable. Esto significa que, una vez creado un objeto String, su contenido no puede modificarse. Si aparentemente se cambia su valor, en realidad se está creando un nuevo objeto con el nuevo contenido. Esta característica refuerza la seguridad y la simplicidad en su uso, ya que una cadena no cambiará inesperadamente.

### Cuando se concatenan dos cadenas, por ejemplo con el operador +, no se modifica ninguna de las cadenas originales. En su lugar, se crea un nuevo objeto String que contiene el resultado de la concatenación. Si esta operación se realiza muchas veces dentro de un bucle, se crearán numerosos objetos intermedios, lo que puede afectar al rendimiento y al consumo de memoria.

### Si se necesita construir una cadena muy larga mediante múltiples concatenaciones, se recomienda utilizar la clase StringBuilder. Esta clase es mutable y permite añadir texto de forma eficiente sin crear un nuevo objeto en cada paso. Una vez finalizada la construcción, se puede obtener el String definitivo mediante el método toString().


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### En Programación Orientada a Objetos, los objetos pueden compararse de dos formas distintas: por su identidad o por su contenido. Comparar por identidad significa comprobar si dos referencias apuntan al mismo objeto en memoria. Comparar por contenido implica verificar si los datos internos de los objetos son equivalentes según algún criterio lógico.

### En Java, el operador == compara la identidad de los objetos cuando se aplica a referencias. Para comparar el contenido se utiliza el método equals. Este método está definido en la clase base Object y, por defecto, se comporta como ==, es decir, compara la identidad. Para que compare el contenido, debe sobrescribirse en la clase correspondiente.

### En el caso de las cadenas, la clase String ya redefine el método equals para que compare el contenido carácter a carácter. Por tanto, dos cadenas deben compararse utilizando equals y no ==, ya que este último solo comprobaría si ambas referencias apuntan al mismo objeto.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Las clases wrapper son clases que encapsulan un tipo primitivo dentro de un objeto. En Java, por ejemplo, los tipos primitivos como int o double no son objetos, pero existen clases que los representan como tales. Un ejemplo es Integer, que encapsula un valor int.

### El proceso de convertir un tipo primitivo en su correspondiente objeto wrapper puede hacerse manualmente creando una instancia de la clase correspondiente. Sin embargo, en Java este proceso suele realizarse de forma automática mediante el mecanismo conocido como autoboxing y unboxing. El compilador convierte automáticamente entre el tipo primitivo y su wrapper cuando es necesario.

### Las clases wrapper permiten tratar valores primitivos como objetos, lo que resulta necesario en estructuras que solo admiten objetos, como colecciones genéricas. No todos los lenguajes orientados a objetos tienen tipos primitivos separados de los objetos; algunos, como Python, tratan todos los valores como objetos. Por tanto, la necesidad de wrappers depende del diseño del lenguaje.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### En Programación Orientada a Objetos, un tipo de dato enumerado es un tipo cuyos valores posibles están previamente definidos y son finitos. Se utiliza cuando se desea representar un conjunto cerrado de opciones, como los días de la semana o los estados de un proceso. En lugar de usar números o cadenas sin control, el enumerado garantiza que solo se puedan emplear los valores permitidos.

### En Java, un tipo enumerado (enum) es en realidad una clase especial. Cada valor del enumerado es una instancia única de esa clase, creada automáticamente. Además, un enum puede tener atributos, constructores y métodos, lo que lo convierte en una herramienta mucho más potente que una simple lista de constantes como podría hacerse en C mediante #define o constantes enteras.

### Desde el punto de vista de la encapsulación, los enumerados en Java permiten asociar comportamiento y datos a cada valor, manteniendo ocultos los detalles internos. También impiden la creación de nuevas instancias desde el exterior, ya que sus constructores son implícitamente privados. Esto garantiza que el conjunto de valores posibles esté completamente controlado y cerrado.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

#### public enum Mes {
####
####    ENERO(1, 31),
####    FEBRERO(2, 28),
####    MARZO(3, 31),
####    ABRIL(4, 30),
####    MAYO(5, 31),
####    JUNIO(6, 30),
####    JULIO(7, 31),
####    AGOSTO(8, 31),
####    SEPTIEMBRE(9, 30),
####    OCTUBRE(10, 31),
####    NOVIEMBRE(11, 30),
####    DICIEMBRE(12, 31);
####
####    private final int ordinalAnual;
####    private final int dias;
####
####    private Mes(int ordinalAnual, int dias) {
####        this.ordinalAnual = ordinalAnual;
####        this.dias = dias;
####    }
####
####    public int getDias() {
####        return dias;
####    }
####
####    public int getOrdinalAnual() {
####        return ordinalAnual;
####    }
####
####    public boolean esDePrimavera(boolean esHemisferioNorte) {
####        if (esHemisferioNorte) {
####            return this == MARZO || this == ABRIL || this == MAYO;
####        } else {
####            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
####        }
####    }
####
####    public boolean esDeVerano(boolean esHemisferioNorte) {
####        if (esHemisferioNorte) {
####            return this == JUNIO || this == JULIO || this == AGOSTO;
####        } else {
####            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
####        }
####    }
####
####    public boolean esDeOtoño(boolean esHemisferioNorte) {
####        if (esHemisferioNorte) {
####            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
####        } else {
####            return this == MARZO || this == ABRIL || this == MAYO;
####        }
####    }
####
####    public boolean esDeInvierno(boolean esHemisferioNorte) {
####        if (esHemisferioNorte) {
####            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
####        } else {
####            return this == JUNIO || this == JULIO || this == AGOSTO;
####        }
####    }
#### }

