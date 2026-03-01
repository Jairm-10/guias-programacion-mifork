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

## Escudo que le ponemos a nuestras clases, para proteger los atributos

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### ENCAPSULACIÓN, tiene que ver con "Protección":
###                                                 - Evito estados NO válidos de mis objetos
###                                                 - Evito dependencias desde fuera, que no quiero
### Encapsulación: He juntado estado y comportamiento en un artefacto (la clase), y ahora puedo ocultar ciertas partes del exterior.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Interfaz pública -> Los miembros que se ven desde fuera, es decir, los que no están ocultos. 

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### La interfaz pública si se cambia, tiene más consecuencias que cualquier cambio en la parte oculta.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Invariantes de clase: Condiciones que los objetos de esa clase deben cumplir para ser válidos y durante toda la vida del objeto.
### Ejemplos:
### - CuentaBancaria debe tener siempre saldo positivo.
### - Persona debe tener edad >= 0
### - Rectángulo debe tener ancho y alto > 0


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### El punto una vez que se crea no va cambiar el valor, gracias al private
### Desde fuera, no va cambiar el punto, estoy ocultando sus atributos
### Lo que está con private no se ve desde fuera, está oculto
###
### class Punto {
###    private int x;
###    private int y;
###
###    public Punto(int x, int y) {
###        this.x = x;
###        this.y = y;
###    }
###
###   public double distanciaAOrigen() {
###        return Math.sqrt(this.x * this.x + this.y * this.y);
###    }
###
###    public double distanciaAOtroPunto(Punto otro) {
###        double dx = this.x - otro.x;
###        double dy = this.y - otro.y;
###        return Math.sqrt(dx * dx + dy * dy);
###    }
###
###    public double getX() {
###     return this.x;
###    }
###
###    public void setX(double x){
###     this.x = x;
###     }
###
###    public String getNombre
###
### }


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### En Java:
### Public: clases, atributos y métodos
### Private: clases internas (no las estamos viendo), atributos y métodos.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Protected, solo se ve desde "subclases" (las veremos en el tema de herencia)
### "package-private" o sin modificador, solo se ve desde el paquete

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### La (a), estña oculta para código de otras clases.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### "getter" y "setter" permiten dar acceso a atributos privados para obtener su valor o cambiarlo.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### No, esto no es ciberseguridad, es facilitar una programación con menos buggs.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Miembro de clase -> No asociad a ninguna instancia; es compartido por todas las instancias.
### En métodos, no hay this.
### Miembro de instancia -> Está asociado a cada instancia, no son compartidos


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### 

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### static


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 


### round es un método de clase(está con mayúscula = clase)
### En la clase del ejercicio 5
###
### public static Punto puntoRedondeado(double x, double y){
###    return new Punto(Math.round(x), Math.round(y));
### }
###
### En otra clase
### class EjercicioEncapsulacion {
###    public static void main(string[] args) {
###        Punto p = Punto.puntoRedondeado(4.5,6.7)

###    }
### }
###
### Si queremos ocultar el constructor sin new Punto
###
### private Punto(double x, double y){
###    this.x = x;
###    this.y = y;
### }


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### public class EjercicioEncapsulacion {
###    public static void main(String[] args) {
###        Punto p = new Punto(4, 5);
###        System.out.println(p.distanciaAOrigen());
###    }
### }


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Si los hago públicos:
### - Para poder garantizar la invariante de clase
### - Para poder cambiar la representación interna
### - Convención es: atributos siempre privados y emplear métodos de acceso


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Inmutable -> su estado no cambia
### Modificador -> cualquier metodo que cambia al estado interno, por ejemplo: un setter.
### Las clases inmutables tienen ventajas -> no hacer clases mutables como primera opción.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### No


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### String es inmutable


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
