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

### DEVOLVER VALOR ESPECIAL
#### float raiz(float num){
####    if (num < 0){
####        return -1.0;
####    }
####    return sqrt(num);
#### }
#### main () {
####    float num = leerDeTeclado();
####    float resultado = raiz(num);
####    if (resultado == -1.0){
####        printf("Error...");
####        } else {
####            printf(+"%d",resultado);
####        }
####    }

### PARAMETRO ADICIONAL PARA ALAMACENAR UN CODIGO DE ERROR
#### float raiz (float num, int x error){
####    if (num < 0){
####        * error = 1;
####        return = 0;
####    } else {
####        * error = 0;
####        return sqrt(num);
####    }
####    main () {
####        int error = 0;
####        float num = leerDeTeclado();
####        float resultado = raiz(num, &error);
####        if (error != 0){
####            printf ("Hay un error);
####        } else {
####            printf ("Todo bien");
####        }
####    }
#### }

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Excepción -> Surge en situaciones atípicas.
### Cuando implementamos -> No; permite indicar más claramente el error
### Cuando llamams -> Me facilita separar la lógica normal de la de reacción o manejo de la situación errónea.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Java con excepciones
### class Calculadora {
####    public static double raiz(double num){
####        if(num < 0.0){
#### //LANZAR UNA EXCEPCIÓN   throw new IlegalArgumentException("num negativo"); //Es una excepción // throw = excepción
####        } else {
####            return Math.sqrt(num);
####        }
####    }
#### }
#### class App{
####    main(...) {
####        double num=leerTeclado();
####        try{ double resultado=Calculadora.raiz(num);
####        sout(resultado);
#### } catch(IlegalArgumentException e){
####    sout ("El numero es negativo, no te preocupes nadie es perfecto")
#### }
####    }
#### }

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### a. Un mensaje (getMessage())
### b. La traza de la pila: 
### (getStackTrace
### printStackTrace)
### c. Opcionalmente, la "Causa", es otra excepción que es la verdadera causa


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Si, se puede tener más de uno:
### -Solo se ejecuta uno.
### -Se va comprobando por orden hasta el primero que encaje,
#### try{
####
#### } catch (TipoExcepción e){ //AccessDeniedException
#### } catch (TipoExcepción2 e){    //IOException
#### }
### Con este orden obtenemos lo que queremos decir
### AccessDeniedException es una IOException, por eso hay que usar este orden
### Porque si sucede que ocurre una AccessDeniedException y hay primero un catch IOException,
### la excepción AccessDeniedException entrará por ese catch, en vez de por el suyo, cosa que no queremos
### Se deben poner del más específico al más general, porque si no, los catch para excepciones específicas no se ejecutarán.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### El finally se EJECUTA SIEMPRE que se entre en el bloque try.
#### try {
#### return 7;
#### } catch (...) {
#### } catch (...) {
#### } finally {
#### }

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Si, puede ir sin catch,
### - Se ejecuta, puesto que es finally
### - Si hubo excepción, como no tomamos catch, se propaga.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Exception: (CONTROLADA)
### (SUBRAMAS)
### ReturnException: (Subtipos): IlegalArgumentException, NullPointerException(NPE), ArrayIndexOutOfBoundsException (NO CONTROLADAS)
### IOException: AccessDeniedException (CONTROLADA)
###
### Controlada: Si obliga a trycatch/throws
### No controlada: No obliga a trycatch/throws

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### public String leerFichero(Path p) throws IOException{ //NO me interesa manejar la excepción
### }
### try{
### = Files.readAllBytes(p);
### } catch(IOException e){
### } 


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### public String leerFichero(Path p) throws IOException{ //NO me interesa manejar la excepción
### try{
### = Files.readAllBytes(p);
### } finally {
### } 
### }


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### - Por poder podemos, pero el compilador no va a obligar al bloque try.catch.
### - No suele ser habitual
### A veces se ponen por documentación.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### La mayoria de lenguajes prefiere las No Controladas. 
### Los lenguajes compilados también hay una corriente en la que si hay excepciones pero no ando aburriendo al programador.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Si, tiene sentido en algunos casos.
### Si, puedo relanzar la misma excepción:
### (Una forma)
#### try{    
#### } catch(NumberFormatException e){
#### throw e; //La excepción que me llegó arriba la capturo, y la relanzo   
#### }
### (Otra forma)
### Envolver en otra excepción nueva:
#### try{
####     
#### } catch(IOException e){
####    throw new RuntimeException(
####        "Excepción de E/S", e);
#### }
### (Otra forma)
### Lanzar otra excepción totalmente nueva:
#### try{
####    
#### } catch(IOException e){
#### throw new AplicacionException("error");
#### }

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Consiste en que yo capturo la excepción y meto la original dentro como causa primigenia.
#### try {
####
#### } catch (IOException e){
#### throw new NetfluxException *Excepción personalizada* ("error E/S", e);
#### }
### Causa de excepción:
### - Se ve cuando la excepción se muestra por pantalla
### "Excepcion externa (NetfluxException)"
### Caused by excepcioninterna (IOException)

### HASTA AQUÍ EXAMEN TEÓRICO
### Tipo test 4 respuestas
### 3 mal resta una bien

