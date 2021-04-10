Ejercicio 1
	No se implementa.
Ejercicio 2
	No se implementa.
Ejercicio 3
	No se implementa.
Ejercicio 4
Error n°1 : recorre de 1 a 10 cuando los arrays van de 0 a elem-1 (0 a 9 en este caso)

Error n°2: cada case debe tener su break, caso contrario se da lo que se llama fall-through, una vez que entra en un switch ejecuta todos los case que estén debajo hasta que se encuentre con el final, o con un break.

Error n°3 : los caracteres se representan con comillas simples. En una variable de tipo char  se almacena un valor de 0 a 256 que representa el código de ese carácter.  Si nosotros asignamos "D" pasan varias cosas. Primero se aloca memoria automáticamente  para el String (o char array mejor dicho) "D". Al asignar "D" al char, no se esta asignando la D, sino el puntero que se genero (el cual apunta a D). Por ende le estamos asignando a cadena[i] un valor incoherente (internamente se convertirá a char ese valor, pero SEGURO no será el valor correspondiente a la D).

Error n°4: Usar operaciones de strings sobre streams. Es importante recalcar la diferencia entre ambos, la cual radica que si bien los dos son una secuencia de bytes, los strings terminan en el caracter nulo, representado por el ‘\0’. Las funciones que esperan strings (printf, strcpy, strcmp) operan desde la dirección del puntero que se pase por parámetro hasta el primer ‘\0’ que encuentre. En este caso la variable cadena no tiene ‘\0’ por ende si usamos una función de este tipo, seguirá recorriendo bloques de memoria hasta que encuentre un ´\0´ corriendo el riesgo de corromper la memoria. También hay que tener en cuenta el otro caso, que es que si un String tiene un ‘\0’ en la mitad, el uso de alguna de estas funciones no resultará correcto ya que analizara solo hasta el primer ‘\0’.

Error n°5 : El error acá radica en  que la función strcmp, esta devuelve 0 si 2 strings son iguales, y el if pregunta siempre por 1 en la expresión-condición. Si esta evalúa a algo distinto de 1, entonces iría por el false.

Error n°6: Esto es un error muy común de los que programan en lenguajes de alto nivel. No se puede comparar cadenas con el ==. Esto sucede primero porque no existe la estructura de string, solo existen punteros, y justamente las 2 variables que tenemos aquí son 2 char*. Al compararlas con == lo que comparamos son los valores de las variables (que son direcciones, no cadenas) y obviamente ambas son diferentes.

Error n°7 : El strcpy no aloca memoria, el char* de destino debe tener memoria alocada y lo suficientemente grande para almacenar la de origen.

Error n°8: Este error radica en que no se hizo un malloc lo suficientemente grande porque el strlen(..) devuelve la cantidad de caracteres SIN EL CARÁCTER NULO. Cuando copiamos con strcpy(..) copiamos también el ‘\0’, por eso debe agregarsele un +1 al size del malloc así entra el ‘\0’.


Error n°9 : Este es un error dado por un concepto, el pasaje por valor o por referencia. En C siempre se hace pasaje por copia del valor, no existe el pasaje por referencia. 
Cuando hacemos 
int a=2;
sqrt(a);
lo que se hace, es que se agarra el valor que se paso por parámetro y se copia al stack de la función llamada. Si yo adentro de sqrt quiero hacerle modificaciones al value (parámetro), podre, pero ninguna de esas modificaciones afectará a la variable a. 
Si yo tengo 
char* cadena=”Hola Mundo”;
ModificarCadena(cadena);    
y dentro de ModificarCadena(char* value) hacemos value=”Otra cadena mejor”

Ahí estamos bajo el mismo caso, ya que cadena tiene como valor una dirección, la dirección de comienzo del String. value, cuando comienza la ejecución de ModificarCadena tiene el mismo valor que cadena. al momento de asignar value no estamos modificando la variable cadena.
Para poder hacer esto habría que hacer
char* cadena=”Hola Mundo”;
ModificarCadena(&cadena);    
y dentro de ModificarCadena(char** value) hacemos *value=”Otra cadena mejor”

Error n°10 Y 11: scanf pide en su segundo argumento punteros. En el primer caso esta mal, porque es no es un puntero, para pasar el puntero deberíamos hacer &x. En el segundo caso también esta mal, porque *st es el contenido del puntero (la cadena) y no el puntero, en este caso con pasar st alcanza.

Error 12: El error aquí, es con los prototipos de las funciones. El compilador asume que todas las funciones que no tengan encabezados devuelven un int, y reciben una cantidad ilimitada de argumentos de cualquier tipo. En el caso de la función sqrt, que devuelve un double, esto genera errores, ya que el retorno de la función no sera 1.4...... sino 1.

Error 13: Es un error parecido al 9, para modificar la variable raíz (y poder elevarla al cuadrado) se debe pasar el puntero a la variable, no su valor.

Error 14: Es un error común, el operador de comparación es == , no =. Lo peor de esto es que solo genera un warning. ¿Por qué? porque es válido hacer una asignación en una condición. Es importante recordar que el resultado de una expresión asignación, es el valor asignado, es decir que si hago a=2; esta expresión evalúa a 2.
Dicho esto, en el if una asignación funciona igual   
 if(a=2)->if(2)->ELSE porque es distinto de 1.

Error 15: no es un error sintáctico. Acá por un error de indentación del código se imprime el mensaje equivocado. Recordar que un else siempre aplica al if más cercano.
Ejercicio 5

El archivo contiene los siguientes errores:
- Archivo Leaks.c
* Cuando se aloca espacio para la variable absolute_path falta agregarle un byte por el ‘\0’ que lleva toda palabra.
- Archivo Cliente.c
* Cuando se destruye el cliente falta liberar la memoria que tiene el valor del nombre y del apellido.
* Se utiliza una variable automática para que el cliente apunte a dicho valor, el problema es que una vez que termina la ejecución de la función la posición de memoria de dicha variable puede ser utilizada para cualquier otra cosa y esto puede corromper el valor del nombre y del apellido.
- Archivo Factura.c
* Falta destruir los elementos que están referenciados por el TAD Lista.

Ejercicio 6

La solución de Leo no es correcta, ya que no esta correctamente sincronizada. Como pueden haber visto, el orden de ejecución varía en cada una de las ejecuciones, esto se debe a que el orden en el que se ejecuten los hilos depende de la planificación del SO, nosotros (los programadores) no podemos asumir nada sobre el orden de ejecución de los hilos. 	
Más allá del orden en el que se ejecuten los hilos, el problema es que no se está garantizando la mutua exclusión al acceder a la variable saldo la cual es utilizada tanto para consulta como para escritura en la función hacer_compras(..).
Para garantizar esto tenemos 2 opciones, utilizar un Mutex o un semáforo binario. 
El mejor mecanismo de sincronización en este caso es un Mutex ya que no importa el orden de ejecución solo importa que haya Mutua Exclusión.

Ejercicio 7

Esta solución de Leo tiene 2 problemas: 
* No se garantiza la Mutua Exclusión en el acceso a la cola, tanto para agregar como para quitar elementos.
Para resolver este inconveniente lo que hacemos es agregar un Mutex que asegurará la Mutua Exclusión al agregar o quitar elementos de la cola.

* No se cumple con la definición funcional que pide que la impresora debe suspenderse si no hay más trabajos disponibles para procesar, y que si luego aparecen debe accionarse sola. 
Podemos ver en el código que si no hay trabajos sale del while y el hilo finaliza su ejecución. Reemplazar ese while por un while(true) tampoco es una opción, ya que si siempre se trata de hacer un pop de la cola y esta está vacía, generará una espera activa consumiendo el uso del CPU con procesamiento innecesario.
Para resolver este inconveniente lo que hacemos es agregar dos semáforos contadores, uno se utiliza para que solo se active cuando hay trabajos pendientes en la cola de impresión y el otro se utiliza para bloquear al hilo que produce cuando se llego al limite de trabajos en espera.


Ejercicio 8

El problema de la solución propuesta es que Leo pensó que el orden de creación de los hilos eran el orden en el que se iban a ejecutar, pero así como venimos viendo con ejercicios anteriores, sabemos que esto no es así. Los hilos se planifican de acuerdo a la disponibilidad y algoritmo del SO y por ende no se puede asumir que el orden de creación de los hilos definirá el orden de la ejecución. Para sincronizar correctamente acá hay una única opción, un semáforo binario por cada ingrediente. 
Un mutex no sería útil, ya que solo asegura la mutua exclusión, se inicializa SIEMPRE en estado desbloqueado, a diferencia del binario el cual se le puede indicar al crearlo cuantas instancias iniciales de ese recurso hay. Y un semáforo contador no tiene lugar ya que solo va a tener solo una instancia de cada ingrediente y es importante el orden de los mismos.
Es importante que noten que los semáforos SIEMPRE están cruzados, uno hace post  del paso siguiente, el cual establoqueadoo en un wait esperando a que sea habilitado por otro hilo.



Ejercicio 9

El error en la solución original radica en la forma de serializar la estructura. Este error existe SOLO porque la estructura t_spock tiene campos dinámico (punteros) como campos.

Cuando uno hace   fwrite(file, size, 1, data)  el fwrite agarra el comienzo del bloque apuntado por data y escribe los siguientes n bytes. 
Si la estructura solo tuviera campos estáticos (como es el caso de la estructura t_villano) puede ser enviada directamente ya que los campos estáticos de las estructuras se acomodan todos correlativos, y por ende pueden ser escritos correctamente por el fwrite.
En el caso de la estructura t_spock la cual tiene campos dinámico, el fwrite(..) se comporta de la misma manera, pero el problema que los valores de los campos son los punteros a los datos (por ej: en el campo spock->nombre no se encuentra la cadena “Roberto Spock”, sino que se encuentra un puntero a la cadena). Si nosotros hacemos un fwrite directamente como hace la solución lo que enviamos es la dirección de memoria del puntero y no el contenido de la estructura asociada. Las direcciones tienen sentido en el contexto de ejecución del proceso, por lo que si le envío a otro proceso una dirección, este no podrá hacer nada con ella, ya que no pertenece al espacio de direcciones original.


Es importante importante ver como en la solución propuesta se usa la función list_iterate para no romper el encapsulamiento de la lista.
