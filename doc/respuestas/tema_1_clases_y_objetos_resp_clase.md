<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Abstracción -> (Encapsulación, herencia y polimorfismo ayudan a en la Abstracción) Olvidarse de detalles para: manejar mejor temas complejos (tener una visión o conocimiento global), facilitar la modificación (el mantenimiento).
### Encapsulación -> Unir información y funciones sobre esa información en un mismo artefacto + Ocultar partes al exterior
### Herencia -> Crear jerarquías 
### Polimorfismo -> Misma función, distintas implementaciones en función del tipo

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### JavaScript, Python, PHP
###
### Compilados, un proceso que el programador es consciente y detecta errores a la vez que traduce el código a una vesión de bajo nivel ejecutable:
### Java, C##
### C++


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Ensamblador (secuencia de instrucciones y saltos arbitrarios)
### Estructuada (secuencia, bifuración(if,switch), iteración(while,for)): Quita el SALTO ARBITRARIO
### Modular (libería, paquete, interfaz): Para encapsular y reutilizar

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Identidad -> Todo objeto tiene identidad única (piénsalo como su dirección en memoria)
### Estado (Atributos: campos) -> El valor de sus atributos en un momento concreto
### Comportamiento (Métodos) -> Funciones que pueden hacer

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Clase: Molde que define el estado y el comportamiento                                           ej: Coche (marca, año)
### Objetos o instancias: Realizaciones concretas en ejecución de una clase                         ej: Mercedes,2009
### Todos estos objetos son INSTACIAS de "perro"(de la clase que le corresponda).                   ej: Mazda,2020

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Nos la saltamos


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Método: Funciones que defino dentro de una clase, es decir, las funciones que un objeto puede hacer.
### Sobrecarga de método: La posibilidad de crear métodos dentro de una clase con el mismo nombre, pero cambiando el tipo y/o número de sus parámetros.

class Calculadora {
    // sin estado
    int sumar(int a, int b){
        return a + b;
    }
    
    double sumar(double a, double b) {
        return a + b;
    }
}

main() {
    Calculadora miCalculadora = new Calculadora();

    int suma1 = miCalculadora.sumar(4, 6);
    double suma2 = miCalculadora.sumar(4.6, 6.7);
}


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### // Esto está en C
### struct Punto{
###    int x;
###    int y;
### }
###
### double calculaDistanciaAOrigen(struct Punto p) {
###    // distancia de p a (0,0)
###    return sqrt(p.x * p.x + p.y * p.y)
### }
###
### main() {
###    Punto miPunto;
###    miPunto.x = 5;
###    miPunto.y = 3;
###     double resultado = caculaDistanciaAOrigen(miPunto);
### }
### ------------
### //JAVA
### class Punto {
###    // Estado(atributos)
###    int x;
###    int y;
###
###    //Comportamiento(métodos)
###    double calculaDistaciaAOrigen() {
###        // distancia de p a (0,0)
###        return sqrt (x * x + y * y);
###    }
### }
###
### class Ejercicio 1 {
###    public static void main(String[] args){
###
###    Punto miPunto = new Punto();
###
###    miPunto.x = 5;
###    miPunto.y = 3;
###
###    double resultado = miPunto.calculaDistaciaAOrigen();
###    }
### }


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
