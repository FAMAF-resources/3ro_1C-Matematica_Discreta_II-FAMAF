Algunos errores de los grupos en la parte 2. (no son todos).

1) Muchos estudiantes no cumplieron con la regla suprema de trabajo grupal:

SIEMPRE DESCONFIAR DE LOS OTROS INTEGRANTES DEL GRUPO.

Es penoso ver como pej GulDukat esta programado con un codigo correcto, eficiente y que da gusto leer mientras que ElimGarak tiene numerosos errores y codigo espantoso.

Cada fragmento de codigo deberia haber sido hecho por una persona pero revisado por otra si trabajaron en grupo.

2) NO COMPILA.
Muchos grupos.

En orden de gravedad, de menor a mayor:

2a) no compila porque se olvidaron de agregar a la parte 2 algún include o define que tenian en la parte 1.

2b) No compila porque en vez de incluir el .h que se pedia, hacen un include de OTRO .h (que tiene las definiciones que debia tener el .h especificado). Muchos grupos en vez de usar API2024Parte2.h usaron APIG24Parte2.h
Esto es un error mas serio. Las especificaciones estan para algo, y si no se respetan los nombres de los .h que los equipos deben usar en comun...no duraran mucho en una empresa.

Y tambien me llama la atencion que muchos grupos usaron ese nombre particular, lo cual  sugiere que compartieron alguna bebida alcoholica juntos o se copiaron el codigo entre si o bien copiaron codigo del 2023, cambiando el 3 en APIG23Parte2.h  por un 4.

2c) no compila por otras razones mas esotericas.

2d) no compila porque solo puede compilar con funciones o estructuras definidas en su parte 1. Desaprobación automática.


3) Muchos grupos tienen diversos segfaults, ya sea en Greedy, GulDukat, ElimGarak o las tres.
A veces el segfault es simplemente un "off by one" error, que reservaron un lugar menos de memoria de lo que tenian que reservar, en otros casos la razon del segfault es mas complicada.
Pero en todos los casos, si hubieran testeado con la flag que les habiamos dicho para testear segfaults, hubieran sabido que tenian el error, lo cual sugiere que no compilaron con esa flag o no testearon adecuadamente.


4) Un ejemplo frecuente del error 3) es que Greedy funciona en gral bien pero crashea con algunos grafos especiales.
En varios casos crashean con grafos completos o ciclos impares porque asumen que el grafo se va a poder colorear con Delta colores cuando en esos casos se requiere Delta+1 y solo reservaron Delta memoria. Pero hay otros errores mas esotericos.

5) Greedy es muy lento y no cumple con la condicion de que su complejidad debia ser O(m).La parte clave esta en cual es la complejidad de colorear un vertice.Dado que hay que recorrer todos los vecinos, esta complejidad no puede ser menor a O(d(v)) si v es el vertice,pero obtener esa complejidad esta bien porque O(suma de grados)=O(m).Pero muchos grupos no logran esa complejidad de O(d(v)) por vertice y la complejidad global es mayor a O(m).

 Los dos errores mas comunes de este tipo (hay otros) son:
 
5a) Para calcular el color con el cual hay que colorear un vertice, empiezan probando con color 1, recorriendo todos los vecinos a ver si alguno tiene el color 1, si no hay ninguno, colorean al vertice con 1, pero si hay algun vecino con el color 1, entonces recorren OTRA VEZ todos los vecinos a ver si alguno tiene color 2, etc.Esto en vez de tener complejidad O(d(v)) tiene complejidad O(d(v).(color con el que se colorea a v))  y la complejidad total sera O(mX(G)) en vez de O(m), y si X(G) esta en el orden de miles, esto se nota, demorando varias horas para poder hacer 1000 Greedys.

5b) Para evitar el problema a) la tecnica mas usada es recorrer los vecinos UNA SOLA VEZ, guardando en algun array auxiliar cuales son todos los colores de los vecinos, luego recorrer el array auxiliar hasta encontrar el menor color no usado por los vecinos.

El problema aca es como resetear las entradas que fueron modificadas del array auxiliar eficientemente.
 Y muchos grupos no lo hacen, porque o bien resetean TODO el array o bien liberan y callocan el array para cada vertice y usan un array con Delta+1 entradas.

Esto da complejidad O(Delta) por vertice y O(nDelta) total=O(n al cuadrado) si Delta es del orden de n.

En este caso, la velocidad es aun peor que el caso anterior, demorando algunos grupos dias en poder hacer 1000 Greedys.



6) Confunden una POScondición con una PREcondición. Esto tiene 2 puntos de descuentos automáticos.
El error mas comun dentro de esta categoria es que la PREcondicion para el array Orden[] en ambos ordenes es:

"Tambien asumen que Orden apunta a una region de memoria con n lugares, donde n es el número de vertices de G."

NADA MAS.
Que Orden[] sea un orden de los vertices es una POScondicion, que debe cumplirse luego de correr los ordenes, no necesariamente antes.

Pej, se puede hacer un calloc para allocar memoria a Orden[] y luego llamar a GulDukat o ElimGarak y en este caso un 24% de los grupos no ordenan los vertices o segfaultean o algo.


7)Tanto para GulDukat como para Garak es esencial calcular cuántos colores tiene el coloreo dado a G.

Esto es trivial de hacer, pero muchos grupos no lo calculan, y una cierta cantidad simplemente asumen que el coloreo viene dado por Greedy lo cual NO estaba en las especificaciones.
Aca a diferencia de 3)  no es que confundan una precondicion con una poscondicion, sino que asumen una precondicion que no existe.

La precondicion respecto de los colores era:

"Funciones para crear ordenes
Estas funciones asumen que el grafo G tiene un coloreo propio con colores {1, 2, .., r} para algún r."

Nada mas. No se especifica que el coloreo venga de Greedy. Los vertices podrian ser coloreados con colores todos distintos y luego llamar a los ordenes.

En particular varios grupos allocan memoria EN LOS ORDENES para Delta+1 colores. Esto esta bien EN GREEDY, porque sabemos que Greedy no va  a usar mas de esa cantidad de colores, pero no lo podemos suponer en los ordenes.

Esto esta mal de 2 formas:

a) puede haber sustancialmente menos colores que Delta +1, desperdiciando memoria.

b) si el coloreo no viene de Greedy, puede haber más y producir segfault.

8) Algunos (pocos) grupos hacen que el orden dado por GulDukat o Garak no ordene por bloque de colores, el VIT no aplica, y los colores aumentan luego de hacer Greedy despues de ellos. Y entregaron un codigo que claramente no chequearon ya que dijimos que esas eran condiciones basicas para chequear.

9) Bastantes grupos a pesar de ordenar por bloque de colores, ordenan los colores incorrectamente, no de acuerdo con lo que se pedía en las especificaciones. Algunos tipos de este error:

9a) No hay ningún tipo de estructura en el orden.
 A veces GulDukat ordena correctamente los múltiplos de 4 primero, luego el resto de los pares, luego los impares, pero el orden entre ellos es cualquier cosa. O bien Garak ordena sin tener en cuenta la cardinalidad de los vertices de ese color.
 
9b) A veces ordenan al revés de lo que se pide.

9c) A veces Garak está casi todo bien pero termina 1,2 en vez de 2,1

9d) A veces GulDukat tiene los múltiplos de 4 bien y antes del resto de los pares y estos antes de los impares, pero los pares no multiplos de 4 estan mal ordenados entre si y los impares estan mal ordenados entre si.

Este error particular viene de calcular mal m(x) pero bien M(x). (el error simétrico de calcular bien m(x) pero mal M(x) no lo he visto o al menos no lo recuerdo).

10) En un caso Garak lo primero que hace es borrar todos los colores del grafo.

11) GulDukat o Garak o ambos son muy lentos.
A veces el problema está en la funcion de ordenación que usan, pero más típicamente el problema es que calculan m,M o la cardinalidad usada en Garak de forma MUY ineficiente.


12) Excesivo uso de memoria para grafos grandes.
Un par de grupos tienen un main extravagante que requiere varios Gigabytes de memoria.
Un grupo codeo uno de los ordenes en forma tal que requiere TERAbytes de memoria.

13) Un error relativamente menor pero muy comun es  que los órdenes retornan 48 en vez de 0 si todo anda bien.

13a) Varios grupos fueron mas alla y en vez de retornar 48 retornaron 79 que es mucho peor.


14) Una gran cantidad de grupos no incluyeron dentro de Greedy ningun tipo de chequeo  de que Orden[] sea una biyeccion.
Dos puntos de descuento automatico por no incluir algo que se pedia.
(Observacion: esto no se pedia en otros años, con lo cual queda ademas la sospecha que simplemente copiaron el codigo de Greedy de otros años, simplemente cambiando empezar en 1 en vez de 0. Si tuviera prueba que hicieron esto el descuento seria mucho mayor).

15) Otros al menos incluyeron el codigo de chequeo de biyeccion, pero mal.

Para chequear que Orden[] es una biyeccion del conjunto {0,1,..,n-1}, dado que es una precondicion que Orden[] tiene n lugares de memoria y que son u32 (por lo tanto Orden[i]>=0 automaticamente) habia que chequear solamente 3 cosas:

a) Orden[i]<n para todo i

b)Orden induce una funcion inyectiva en {0,1,...,n-1}

c) Orden induce una funcion suryectiva sobre {0,1,..,n-1}

Pero como {0,1,..,n-1} es finito, en realidad basta chequear a) y alguna de b) o c), la otra sera automatica.

Muchos grupos hicieron esto correctamente (no hice la estadistica de cuantos probaron a) y b) y cuantos probaron a) y c)).
Pero otros grupos chequearon solamente una condicion de las tres.

Varios grupos chequearon solamente a) lo cual es medio ridiculo pero al menos no produce segfaults.

Otros grupos mas sofisticadamente chequearon solamente b) o solamente c), pero al no chequear a) producian segfault al querer escribir en memoria prohibida si Orden[i]>=n para algun i.

Algunos grupos codearon el chequeo tanto para a) como para alguna de b) o c) pero codearon mal EL ORDEN de chequeo:
pusieron ambos chequeos dentro de un solo if, y se olvidaron que los ifs se evaluan de izquierda a derecha.

16) Greedy no es greedy.
Colorean a veces con la misma cantidad de colores que colorearia Greedy real, pero no le dan a los vertices el mismo color que les daria Greedy.
En otros casos ni siquiera colorean con la misma cantidad de colores de Greedy.

Pej, como Greedy mira solamente los colores de los vertices anteriores al que se esta coloreando, el vertice i-esimo en ser coloreado (es decir, en nuestro caso, el vertice en la posicion Orden[i-1]) no puede nunca ser coloreado con un color mayor a i, porque a lo sumo se habran eliminado los colores 1,2,..,i-1.(y en particular, el vertice en Orden[0] debe ser coloreado con el color 1).

Varios grupos ni siquiera cumplian con esta propiedad elemental.

Algunos grupos (muy pocos) ni siquiera obtuvieron coloreos propios.

Otros errores cometidos aca fue que Greedy directamente hacia cosas que no producian efecto  y dejaba el coloreo que ya tenia el grafo.

O bien coloreaba todos los vertices con colores distintos, siempre.
