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

### En C no existen excepciones como en Java, por lo que el control de errores debe diseñarse explícitamente. Cuando una función puede fallar, como en el caso de una función `raiz` que solo acepta números positivos, el error debe comunicarse al código que la invoca. Es importante que la función no imprima directamente el mensaje de error, sino que lo indique de alguna forma para que el llamador decida cómo actuar.

### Una primera opción consiste en **utilizar un valor especial de retorno** para indicar error. Por ejemplo, se puede devolver un valor imposible o poco probable (como `-1.0`) cuando se recibe un número negativo, y que el programa principal compruebe ese resultado antes de usarlo.

####
#### #include <stdio.h>
#### #include <math.h>
####
#### double raiz(double x) {
####    if (x < 0) {
####        return -1.0;  // Valor que indica error
####    }
####    return sqrt(x);
#### }
####
#### int main() {
####    double resultado = raiz(-4.0);
####
####    if (resultado < 0) {
####        printf("Error: no se puede calcular la raiz de un numero negativo\n");
####    } else {
####        printf("Resultado: %f\n", resultado);
####    }
####    return 0;
#### }
####
### Una segunda opción consiste en **separar el resultado del estado de error**, por ejemplo devolviendo un código de error y utilizando un parámetro por referencia (puntero) para almacenar el resultado correcto. Esta técnica es habitual en C cuando se quiere evitar ambigüedad en el valor devuelto.


#### #include <stdio.h>
#### #include <math.h>
####
#### int raiz(double x, double *resultado) {
####    if (x < 0) {
####        return 0;  // 0 indica error
####    }
####    *resultado = sqrt(x);
####    return 1;      // 1 indica éxito
#### }
####
#### int main() {
####    double valor;
####
####    if (!raiz(-4.0, &valor)) {
####        printf("Error: no se puede calcular la raiz de un numero negativo\n");
####    } else {
####        printf("Resultado: %f\n", valor);
####    }
####    return 0;
#### }
####
### En ambos casos, el error se detecta dentro de la función pero se gestiona fuera de ella. Esta forma de trabajar obliga a comprobar manualmente los errores después de cada llamada, lo que puede hacer que el código sea más largo y propenso a olvidos si no se tiene cuidado.

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Una **excepción** es un mecanismo que permite señalar que se ha producido una situación anómala o un error durante la ejecución de un programa. En lugar de devolver un valor especial o un código de error, como se hace habitualmente en C, el flujo normal del programa se interrumpe y se transfiere el control a un bloque específico preparado para gestionar ese problema. De este modo, el tratamiento del error queda separado del código principal.

### El objetivo de utilizar excepciones al implementar funciones es poder detectar condiciones incorrectas (por ejemplo, parámetros inválidos o estados incoherentes) y comunicarlas de forma clara al código que ha realizado la llamada. Así, la función no necesita decidir cómo reaccionar ante el error, sino que simplemente lo notifica. Esto mejora la claridad del diseño y evita mezclar la lógica principal con comprobaciones constantes.

### Desde el punto de vista del programador que llama a la función, las excepciones permiten gestionar los errores de manera estructurada, agrupando el código que puede fallar y el código que maneja el problema. Esto reduce la probabilidad de olvidar comprobar errores y facilita la lectura y mantenimiento del programa.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### En Java, el control de errores puede realizarse mediante **excepciones**, lo que permite separar claramente el código que detecta el problema del código que lo gestiona. En lugar de devolver un valor especial, el método puede lanzar una excepción cuando se detecta una condición inválida, como recibir un número negativo para calcular la raíz cuadrada. De este modo, el error no se mezcla con el valor de retorno.

### El método `raiz` puede declararse dentro de una clase `Calculadora` y lanzar una excepción si el parámetro es negativo. El método `main`, que realiza la llamada, puede capturar esa excepción utilizando un bloque `try-catch`, gestionando así el error desde fuera del método.


#### public class Calculadora {
#### 
#### public static double raiz(double x) {
####        if (x < 0) {
####            throw new IllegalArgumentException(
####                "No se puede calcular la raiz de un numero negativo"
####            );
####        }
####        return Math.sqrt(x);
####    }
####
####    public static void main(String[] args) {
####        try {
####            double resultado = Calculadora.raiz(-4.0);
####            System.out.println("Resultado: " + resultado);
####        } catch (IllegalArgumentException e) {
####            System.out.println("Error: " + e.getMessage());
####        }
####    }
#### }

### En este diseño, el método `raiz` se limita a detectar el problema y lanzar la excepción. El método `main` decide cómo informar al usuario. Esto permite mantener separada la lógica de cálculo de la lógica de tratamiento de errores, haciendo el código más claro y estructurado.

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### **Lanzar** una excepción significa generar explícitamente una situación de error mediante la instrucción `throw`, creando un objeto que representa ese problema. En el ejemplo anterior, el método `raiz` lanza una excepción cuando detecta que el parámetro es negativo. En ese momento, la ejecución normal del método se interrumpe inmediatamente y no se ejecutan las instrucciones posteriores.

### **Controlar** o **capturar** una excepción significa interceptarla mediante un bloque `try-catch`. En el ejemplo, el método `main` envuelve la llamada a `raiz` dentro de un `try`, y en el `catch` se indica qué hacer si se produce el error. De este modo, el código que puede fallar queda separado del código que gestiona el problema, y el programa puede continuar ejecutándose después del bloque `catch`.

### Cuando se dice que una excepción se **propaga**, significa que si un método no la captura, esta asciende automáticamente por la pila de llamadas hasta encontrar un método que sí la capture. Si ningún método la controla, el programa termina mostrando un mensaje de error. A medida que la excepción se propaga, los métodos intermedios van finalizando de forma abrupta: se abandona su ejecución y se eliminan de la pila de llamadas. Estos métodos no se reanudan después; su ejecución queda definitivamente interrumpida. Solo el método que captura la excepción puede decidir cómo continuar el flujo del programa.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Una de las principales ventajas frente a C es que no es necesario comprobar manualmente el resultado de cada función después de cada llamada. En C, cuando se utilizan códigos de error, cada función intermedia debe revisar el valor devuelto y, si hay error, devolverlo nuevamente al nivel superior. Esto obliga a escribir comprobaciones repetitivas y aumenta el riesgo de olvidar alguna, lo que puede provocar comportamientos incorrectos.

### Con la **propagación natural** de las excepciones, si un método no captura la excepción, esta asciende automáticamente por la pila de llamadas hasta encontrar un bloque que la controle. Los métodos intermedios no necesitan escribir código adicional para reenviar el error: simplemente se interrumpe su ejecución y se continúa la búsqueda de un manejador adecuado. Esto simplifica considerablemente el código y evita estructuras anidadas llenas de comprobaciones.

### Además, este mecanismo permite separar claramente la lógica principal del programa del tratamiento de errores. En lugar de mezclar constantemente condiciones de fallo con el flujo normal, se agrupan los posibles errores en bloques específicos. Esto mejora la legibilidad, reduce el acoplamiento entre funciones y hace que el mantenimiento del programa resulte más sencillo y menos propenso a errores humanos.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### En los lenguajes orientados a objetos, las excepciones suelen ser **objetos**. En Java, por ejemplo, una excepción es una instancia de una clase que representa una situación de error. Esto encaja de forma natural con el modelo de POO, donde todo se organiza en clases y objetos, permitiendo que los errores también se modelen como entidades con estado y comportamiento.

### El hecho de que las excepciones sean objetos permite encapsular información relevante sobre el error. Una excepción puede almacenar un mensaje descriptivo, datos adicionales (como el valor incorrecto recibido) e incluso proporcionar métodos propios. De este modo, no solo se indica que ha ocurrido un error, sino que se transporta información estructurada que puede utilizarse al capturarlo. Esto mejora la claridad del diseño y evita depender únicamente de códigos numéricos o mensajes sueltos.

### Sí es posible crear excepciones personalizadas. Basta con definir una clase propia que herede de una clase base de excepciones. Esto permite representar errores específicos del dominio del problema, haciendo el programa más expresivo y facilitando un tratamiento más preciso y organizado de las distintas situaciones anómalas.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### En comparación con el ejemplo en C, donde normalmente solo se dispone de un código numérico o un valor especial para indicar error, en Java un objeto excepción transporta información mucho más rica. Esa información viaja automáticamente junto con la excepción mientras se propaga por la pila de llamadas, hasta que finalmente es capturada por un manejador. Esto elimina la necesidad de pasar manualmente datos adicionales sobre el error.

### La información esencial que lleva cualquier objeto excepción incluye, en primer lugar, un **mensaje descriptivo** que explica qué ha ocurrido. Además, incorpora automáticamente el **tipo concreto de la excepción**, lo que permite distinguir entre distintos tipos de errores mediante diferentes bloques `catch`. También se almacena la **traza de la pila (stack trace)**, que indica en qué método y en qué línea se produjo el problema y cómo se llegó hasta allí.

### Esta información resulta especialmente útil cuando se llega a un manejador, ya que permite diagnosticar con precisión el origen del error. En C, para obtener algo similar sería necesario diseñar manualmente estructuras adicionales y transmitirlas explícitamente. En cambio, en Java, todo ello queda encapsulado dentro del propio objeto excepción, facilitando el análisis, la depuración y el mantenimiento del programa.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### En Java es posible tener **más de un bloque `catch`** asociado a un mismo bloque `try`. Esto permite capturar y tratar de forma diferente distintos tipos de excepciones que puedan producirse dentro del código protegido. Cada bloque `catch` especifica el tipo de excepción que es capaz de manejar, lo que aporta flexibilidad y precisión en el control de errores.

### Sin embargo, cuando se produce una excepción, **solo se ejecuta un bloque `catch`**: el primero cuyo tipo sea compatible con la excepción lanzada. Una vez encontrado ese bloque, los demás `catch` se ignoran y no se ejecutan. Esto implica que el orden de los bloques es importante, ya que los tipos más específicos deben colocarse antes que los más generales.

### Por tanto, pueden declararse varios `catch`, pero en cada ejecución concreta solo uno de ellos será activado. Este diseño permite distinguir claramente entre distintos tipos de errores, evitando tratamientos genéricos cuando se desea una gestión más detallada.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Cuando una excepción interrumpe el flujo normal de ejecución, existe el riesgo de que ciertas operaciones necesarias (como cerrar un fichero o liberar un recurso) no lleguen a ejecutarse. Para garantizar que ese código se ejecute siempre, Java proporciona el bloque `finally`. El bloque `finally` se ejecuta tanto si se produce una excepción como si no, e incluso aunque la excepción se propague hacia niveles superiores.

### El bloque `finally` puede utilizarse junto con `catch`, cuando se desea controlar la excepción y además asegurar la liberación de recursos. En este caso, primero se ejecuta el `try`, luego el `catch` si hay error, y finalmente el `finally`.

#### import java.io.FileReader;
#### import java.io.IOException;
####
#### public class EjemploConCatch {
####
####    public static void main(String[] args) {
####        FileReader lector = null;
####
####        try {
####            lector = new FileReader("datos.txt");
####            int dato = lector.read();
####            System.out.println(dato);
####        } catch (IOException e) {
####            System.out.println("Error al leer el fichero: " + e.getMessage());
####        } finally {
####            try {
####                if (lector != null) {
####                    lector.close();
####                }
####            } catch (IOException e) {
####                System.out.println("Error al cerrar el fichero");
####            }
####        }
####    }
#### }

### También es posible utilizar `finally` sin un bloque `catch`, cuando no se desea gestionar la excepción en ese nivel, sino dejar que se propague. En ese caso, el `finally` se ejecuta antes de que la excepción continúe ascendiendo por la pila de llamadas.

#### import java.io.FileReader;
#### import java.io.IOException;
####
#### public class EjemploSinCatch {
####
####    public static void main(String[] args) throws IOException {
####        FileReader lector = null;
####
####        try {
####            lector = new FileReader("datos.txt");
####            int dato = lector.read();
####            System.out.println(dato);
####        } finally {
####            if (lector != null) {
####                lector.close();
####           }
####        }
####    }
#### }

### En ambos casos, el bloque `finally` asegura que el recurso se libere correctamente, independientemente de si la excepción se captura en ese punto o se sigue propagando.

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Sí, en Java el bloque `finally` puede utilizarse sin un bloque `catch`. La estructura válida es `try-finally`, siempre que exista al menos uno de los dos (`catch` o `finally`). En este caso, si se produce una excepción dentro del `try`, esta no se captura en ese nivel, sino que se propaga hacia arriba, pero antes de propagarse se ejecuta el bloque `finally`.

### El bloque `finally` se ejecuta tanto si ocurre una excepción como si no ocurre ninguna. Es decir, se ejecuta después del `try` si todo ha ido correctamente, o después del `catch` si la excepción ha sido capturada, o justo antes de que la excepción continúe propagándose si no ha sido capturada. Su finalidad es garantizar la ejecución de código que debe realizarse siempre, como liberar recursos.

### Incluso si dentro del bloque `try` aparece un `return`, el bloque `finally` también se ejecuta antes de que el método termine. El valor de retorno se calcula primero, pero la salida efectiva del método se retrasa hasta que finaliza el `finally`. Solo en situaciones extremas, como la finalización abrupta de la máquina virtual, podría no ejecutarse, pero en condiciones normales se ejecuta siempre.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### En Java existen dos grandes categorías: **excepciones controladas (checked)** y **no controladas (unchecked)**. Las controladas son aquellas que el compilador obliga a declarar o capturar; es decir, si un método puede lanzarlas, debe indicarlo con `throws` o tratarlas con `try-catch`. Las no controladas no requieren esa declaración explícita y pueden propagarse sin que el compilador lo exija.

### La clase `RuntimeException` es clave en esta clasificación. Todas las excepciones que heredan de `RuntimeException` se consideran **no controladas**. Suelen representar errores de programación (como accesos indebidos o estados incoherentes) que no deberían recuperarse normalmente en tiempo de ejecución. En cambio, las excepciones que heredan directamente de `Exception` (pero no de `RuntimeException`) son controladas y suelen representar situaciones externas o previsibles.

### Ejemplos típicos de **excepciones controladas** incluyen:

### * `IOException` al trabajar con ficheros.
### * `SQLException` al acceder a bases de datos.
### * Una excepción propia como `DatoInvalidoException` cuando se detecta un error que el llamador puede corregir.
### * Problemas de conexión a un recurso externo.

### Situaciones donde suele preferirse una **excepción controlada**:

### * Lectura o escritura en ficheros.
### * Acceso a red o base de datos.
### * Validación de datos introducidos por el usuario.
### * Procesamiento de recursos externos que pueden fallar.

### Ejemplos típicos de **excepciones no controladas** incluyen:

### * `NullPointerException`.
### * `ArithmeticException` (por ejemplo, división por cero).
### * `IllegalArgumentException` cuando se pasa un parámetro incorrecto.
### * Una excepción propia como `EstadoInvalidoRuntimeException`.

### Situaciones donde suele preferirse una **excepción no controlada**:

### * Errores de programación que indican un fallo lógico.
### * Violaciones de invariantes internas de la clase.
### * Uso incorrecto de una API por parte del programador.
### * Estados que no deberían ocurrir si el programa está bien diseñado.

### En general, las excepciones controladas se emplean cuando el error es recuperable y el llamador puede hacer algo útil para solucionarlo, mientras que las no controladas suelen indicar fallos que deben corregirse en el código más que gestionarse en tiempo de ejecución.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### La palabra clave `throws` se utiliza en la declaración de un método para indicar que dicho método puede lanzar una o varias excepciones. Se coloca después de la lista de parámetros y antes del cuerpo del método. Su función principal es informar al compilador y al programador de que ese método no gestiona internamente ciertas excepciones, sino que las deja propagarse al método que lo invoque.

### En el caso de las **excepciones controladas (checked)**, el compilador obliga a tomar una decisión: o bien capturarlas mediante un bloque `try-catch`, o bien declararlas con `throws`. Por eso `throws` es una alternativa a capturar la excepción. En lugar de gestionarla en ese punto del programa, se decide que sea otro nivel superior quien se encargue de hacerlo.

### Esto permite diseñar mejor la responsabilidad del tratamiento de errores. Un método de bajo nivel puede limitarse a realizar su tarea y declarar que puede fallar, mientras que un método de nivel superior, que tenga más contexto sobre lo que está haciendo el programa, puede decidir cómo reaccionar. De este modo, `throws` facilita la propagación estructurada de excepciones y evita capturas innecesarias en niveles donde no se puede hacer un tratamiento adecuado.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Cuando un método no desea gestionar una excepción controlada, puede declararla en su firma mediante `throws` para que se propague al método llamador. En el caso de apertura de ficheros, la excepción típica es `IOException` (o su subclase `FileNotFoundException`). De este modo, el método indica que puede producirse ese error, pero no lo captura localmente.

### Aun así, puede utilizarse un bloque `finally` para garantizar que el recurso se cierre correctamente si ha llegado a abrirse. El `finally` se ejecutará tanto si ocurre una excepción como si no, antes de que esta se propague hacia arriba en la pila de llamadas.

#### import java.io.FileReader;
#### import java.io.IOException;
####
#### public class LectorFichero {
####
####    public static void procesarFichero(String nombre)
####            throws IOException {
####
####        FileReader lector = null;
####
####        try {
####            lector = new FileReader(nombre);
####            int dato = lector.read();
####            System.out.println("Primer caracter: " + dato);
####        } finally {
####            if (lector != null) {
####                lector.close();
####            }
####        }
####    }
#### }

### En este ejemplo, el método declara `throws IOException`, por lo que no captura la posible excepción si el fichero no existe o si ocurre un error de lectura. Sin embargo, el bloque `finally` asegura que, si el fichero llegó a abrirse, se cerrará correctamente antes de que la excepción continúe propagándose al método que realizó la llamada.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Sí, es posible declarar en `throws` excepciones no controladas, como aquellas que heredan de `RuntimeException`. El compilador no lo exige, pero tampoco lo prohíbe. Desde el punto de vista sintáctico, no hay diferencia entre declarar una excepción controlada o no controlada en la firma del método.

### Sin embargo, en el caso de las excepciones no controladas, el método llamador no está obligado a capturarlas con `try-catch`. Estas excepciones pueden propagarse libremente sin que el compilador genere error. Por eso, en la práctica, rara vez se declaran explícitamente en `throws`, ya que su propia naturaleza indica que representan fallos de programación o situaciones que normalmente no se pretende recuperar.

### El único sentido de declararlas en `throws` sería **documentar explícitamente** que el método puede lanzar ese tipo de excepción, mejorando la claridad de la interfaz. No obstante, no implica ninguna obligación adicional para el llamador. En general, si se espera que el error sea tratado de forma específica, suele preferirse una excepción controlada; si se trata de un error lógico o de uso incorrecto, se deja como no controlada sin necesidad de declararla.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Se recomienda utilizar **excepciones controladas** cuando el error representa una situación externa o previsible ante la que el llamador puede reaccionar razonablemente. Por ejemplo, problemas de entrada/salida como `IOException`, fallos de conexión o datos proporcionados por el usuario que pueden no ser válidos. En estos casos, tiene sentido obligar al programador a decidir cómo tratar la situación, ya que el programa podría recuperarse o tomar una alternativa.

### En cambio, las **excepciones no controladas**, como `IllegalArgumentException`, suelen emplearse cuando el error indica un uso incorrecto de la API o un fallo lógico en el propio código. Representan situaciones que, en principio, no deberían producirse si el programa está bien diseñado. En estos casos, no se obliga al llamador a capturarlas, ya que lo adecuado suele ser corregir el error en el código y no intentar recuperarse en tiempo de ejecución.

### No todos los lenguajes orientados a objetos distinguen entre excepciones controladas y no controladas. Java es uno de los ejemplos más conocidos que sí lo hace. En muchos otros lenguajes (como C#, Python o C++), todas las excepciones funcionan de forma similar a las no controladas de Java: el compilador no obliga a declararlas ni capturarlas. Por tanto, en los lenguajes donde solo existe una opción, la más habitual es el modelo equivalente a las **excepciones no controladas**, donde la captura es opcional y depende del diseño del programador.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Sí, tiene sentido lanzar excepciones dentro de un bloque `catch`. Esto puede hacerse cuando se desea transformar una excepción en otra más adecuada al nivel de abstracción en el que se está trabajando, o cuando se quiere añadir información adicional antes de volver a propagar el error. De este modo, se evita exponer detalles internos de bajo nivel a capas superiores del programa.

### También es posible **relanzar la misma excepción capturada** utilizando simplemente `throw e;`. En este caso, no se crea una nueva excepción, sino que se deja que la misma continúe propagándose. Esto resulta útil cuando en el `catch` solo se desea realizar alguna acción adicional (por ejemplo, registrar el error en un log) pero no se pretende resolverlo en ese punto.

### Ejemplo de transformación de excepción:

#### try {
####    lector.read();
#### } catch (IOException e) {
####    throw new RuntimeException("Error al procesar el fichero", e);
#### }

### En este caso, se captura una `IOException` y se lanza una nueva excepción más general, manteniendo la original como causa.

### Ejemplo de relanzamiento de la misma excepción:

#### try {
####    lector.read();
#### } catch (IOException e) {
####    System.out.println("Se produjo un error, se propagará.");
####    throw e;  // Se relanza la misma excepción
#### }

### Esto último tiene sentido cuando el método no puede solucionar el problema, pero necesita realizar alguna acción previa (como liberar recursos adicionales o registrar información) antes de permitir que la excepción continúe propagándose hacia niveles superiores.

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Que una excepción sea la **“causa”** de otra significa que una excepción de nivel superior encapsula internamente otra excepción que se produjo originalmente. Esto se utiliza cuando se desea traducir un error de bajo nivel (por ejemplo, de entrada/salida) a un error más acorde con el dominio de la aplicación, sin perder la información original. De este modo, se mantiene la abstracción hacia el exterior, pero se conserva el detalle técnico para depuración.

### En Java, muchas clases de excepción permiten recibir otra excepción como causa en su constructor. Así, cuando se captura una excepción de bajo nivel, puede crearse una nueva excepción personalizada y pasar la original como causa. Esto mantiene la cadena completa de errores y facilita entender qué ocurrió realmente.

#### public class ErrorProcesamientoException extends Exception {
####    public ErrorProcesamientoException(String mensaje, Throwable causa) {
####        super(mensaje, causa);
####    }
#### }

####
#### try {
####     FileReader lector = new FileReader("datos.txt");
####    lector.read();
#### } catch (IOException e) {
####    throw new ErrorProcesamientoException(
####        "No se pudo procesar el fichero de datos", e);
#### }

### Cuando una excepción con causa se muestra por pantalla (por ejemplo, mediante `printStackTrace()`), sí se ve la información de la causa. Aparece la traza de la excepción principal y, a continuación, una sección indicando “Caused by”, seguida de la excepción original y su propia traza. Esto permite identificar tanto el error de alto nivel como el problema técnico subyacente.


