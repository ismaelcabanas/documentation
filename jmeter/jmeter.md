# JMeter: Getting Started

## Performance testing

Los tests de rendimiento deberían incorporarse en una fase temprana del proyecto y continuar en el ciclo de desarrollo de éste.
Dejar este tipo de tests al final de proyecto no es nada recomendable.

### Qué son tests de rendimiento

Los tests de rendimiento nos sirven para ver cómo un sistema (aplicación, base de datos, ...) se comporta en condiciones de carga para ver cual es su impacto en el sistema. También pueden ser útiles para detectar **cuellos de botella** y **problemas de rendimiento**.

Esto es crítico puesto que puede **afectar** en la **obtención de ingresos** y en la **retención de cliente**.

Los tests de rendimiento se deben ejecutar una vez hayan pasado todos los tests funcionales (de aceptación, unitarios, ...), después que estemos seguros que la aplicación funciona correctamente. 

Pero, estos tests no son solo para medir velocidad. En particular, los tests de rendimiento se miden en términos de:

 1.- **Tiempo de respuesta (response time)**: el tiempo que tarda el sistema en responder a una petición de cliente. Realmente, el **tiempo de respuesta es** la suma de:

  - el *tiempo desde que se realiza la petición hasta que alcanza al sistema* (servidor). A este tiempo hay que sumar cierto tiempo de **latencia de red**.

  - el *tiempo de procesar la petición* y generar una respuesta. Este tiempo depende de la complejidad de la petición y de las características del hardware del servidor.
  
  - el *tiempo que pasa hasta que se envía la respuesta al cliente*. A este tiempo hay que sumar cierto tiempo de **latencia de red**.
  
  **Cuanto menor sea el tiempo de respuesta mejor será nuestra aplicación**.

 2.- Rendimiento (throughput): se puede medir de dos formas:
  
  - El número de transacciones (peticiones con sus respuestas) por unidad de tiempo (millis o segundos)
  
  - El número de kbs o bytes (enviados y recibidos) por unidad de tiempo (millis o segundos)
  
  **Cuanto mayor sea el dato mejor**.
  
 3.- Fiabilidad (Realibility): mide cómo de bien la aplicación detecta y maneja los errores en términos de número de errores por número de peticiones. 
 
 **Cuanto menor sea el dato mejor**.

 4.- Escalabilidad: no es sólo una medida, nos dice cómo de bien el sistema puede expandirse en términos de tiempos de respuesta, rendimiento y porcentaje de errores cuando al sistema se le añaden nuevos recursos, funcionalidades, etc... Por ejemplo, la escalabilidad nos ayuda a saber qué podríamos necesitar en nuestro sistema para tener una carga de 10000 clientes o usuarios.
  
 Podríamos optar por **escalabilidad vertical**, añadiendo más memoria o CPUs a nuestro servidor, o **escalabilidad    horizontal**, añadiendo servidores en un clúster.

### Proceso de tests de rendimiento

#### Requisitos de rendimiento

Antes de crear un sistema es conveniente saber qué requisitos de rendimiento necesitamos en nuestro sistema. Éstos, a veces nos vienen dado por contrato o acuerdos, y otras, somos nosotros los que debemos establecer esos requisitos antes de testear el sistema.

Ejemplos de este tipo de requisitos podría ser: el tiempo medio/máximo de respuesta debería ser 800 ms, el sistema debería ser
capaz de soportar 60 páginas por segundo, o el sistema debería ser capaz de soportar al menos 10000 usuarios por hora.

#### Diseño y construcción de los tests

Una vez tenemos estos requisitos, podemos diseñar y construir unos tests.

Tenemos que tener claros cuales son los patrones de uso de la aplicación, por ejemplo, ¿cuántos comunicados queremos que se envíen por segundo?, o de media, ¿cuántos gestores pueden navegar a la vez en la gestión de candidatos en un momento dado?. Esto ayuda a diseñar nuestros tests y construirlos con JMeter.

### Preparción del entorno de test

Deberíamos tener un entorno lo más similar posible al entorno de producción. 

Luego, para ejecutar los tests, lo primero que tenemos que hacer es validar el script del test y los datos para comprobar que todo funciona como debe. Debemos comprobar los logs para ver si hay errores y luego analizar si éstos son producidos por los tests, la aplicación, el servidor o una combinación de estos.

Después de analizar los resultados, se toman acciones para solucionar esos problemas como: optimizar consultas a la base de datos, incrementar la memoria disponible para la aplicación, o cualquier otra cosa. 

Después de cada cambio realizado, se vuelven a ejecutar estos tests de rendimiento y se comparan los resultados obtenidos con los anteriores para comprobar que dan resultados positivos, de lo contrario, se siguen haciendo cambios y se vuelve a repetir el proceso hasta que los requisitos de rendiemiento se satisfagan.

### Tipos de tests de rendimiento

De forma general, los tests de rendimiento evaluan una aplicacion bajo una cierta cantidad de carga. Hay dos maneras de generar la carga en los tests:

 1.- Incrementar el número de usuarios que acceden a la aplicación al mismo tiempo.

 2.- Incrementar el número de peticiones que la aplicación tiene que gestionar,  independientemente del número de usuarios.
 
Una vez decidamos qué tipo de carga vamos a usar debemos decidir qué tipo de test vamos a ejecutar. 

#### Smoke tests

El primer tipo de test que deberíamos ejecutar es un **Smoke Test**, que es un test con una carga ligera, generalemente un usuario para comprobar que el test funciona bien.

#### Load tests

El segundo tipo de test que deberíamos realizar sería un test de carga, que es un test realizado con un nivel de carga específico. Normalmente, haremos varios de estos tests con diferentes niveles de carga para ver cómo se comporta la aplicación.

#### Stress tests

El tercer tipo de tests que deberíamos realizar serán tests de stress. Este tipo de test se hacen con un nivel de carga que supera los niveles de carga normales para ver hasta qué punto la aplicación permanece estable y respondiendo. 

En un test de estrés, iremos incrementando los usuarios de forma constante en un periodo de tiempo, es decir, primero con 100 usuario y ver cómo se comporta, luego con 500, y así sucesivamente; o podemos incrementar los usuarios en un gran número durante un largo periodo de tiempo para averiguar en qué punto la aplicación deja de funcionar.

#### Spike tests

El cuarto tipo de tests que deberíamos realizar serán los tests de picos, en los que la aplicación es sometida a picos repentinos de carga que superan los límites de su capacidad para ver si después de esos picos de carga la aplicación es lo suficientemente **robusta** como para seguir funcionando durante y después del Spike Test.

#### Endurance tests

El quinto tipo de tests que deberíamos realizar serán los tests de robusted, en los que la aplicación es sometida a cierta carga dentro de sus límites pero durante un largo periodo de tiempo, horas o en algunos casos días, para ver si la aplicación tiene memory leaks o no cierra bien las conexiones a base de datos o entrada y salida. 

## JMeter

JMeter es una herramienta Open Source de tests de carga que forma parte de Apache. JMeter no es un navegador web, no renderiza respuestas HMTL, no ejecuta Javascript. 

JMeter trabaja a nivel de protocolo simulando peticiones a través de la red. Con JMeter simulamos miles de usuarios, cuyo límite está en la capacidad de la máquina en la que se ejecuta JMeter. 

JMeter está escrito en Java y usa un thread para simular un usuario. Por cada usuario crea una petición, guarda la respuesta y recolecta todos los datos que necesita para presentar informes y gráficos en tiempo real y al final del test.

### Instalación

#### Requisitos

Unos requisitos aceptables para ejecutar JMeter podrían ser una máquina de 4 o más núcleos con una memoria RAM de unos 16GB. Con estas condiciones podremos simular miles de usuarios.

También, un escenario ideal es en el que JMeter no se esté ejecutando en la misma máquina que la aplicación que se quiere testear, ya que ambas compitirán por los mismos recursos.

La interface de red debería ser de un Gigabit, para que la red no sea un cuello de botella.

Puesto que JMeter es un programa escrito en Java necesita tener instalada la JVM para poder ejecutarse. 

#### Instalación

Para instalar JMeter en Mac es sencillo:

```
brew install jmeter
```

Después de terminar el proceso de instalación, deberíamos tener instalado JMeter en **/usr/local/Cellar/jmeter**.

#### Ejecutando JMeter

JMeter lo podemos ejecutar desde un modo gráfico o línea de comandos.

##### Modo gráfico

El modo gráfico se debería usar para crear los script de test y para hacer alguna prueba básica. Para arrancar este modo, nos vamos al directorio **/bin** del directorio de instalación de JMeter y ejecutamos el comando **jmeter**.

##### Modo línea de comandos

Este modo debería usarse cuando se van a hacer los tests de rendimiento, en modo no gráfico para que consuma el menor número de recursos. Un ejemplo de ejecución sería

```
jmeter -n -t testFile.jmx
```

**-n** indica que JMeter se va a ejecutar en modo no gráfico
**-t** indica el fichero que contiene el script del test

## Configurando un Test Plan

En JMeter los componentes de un test están organizados en un árbol, donde cada componente tiene un ámbito que especifica a qué otros componentes puede ser aplicado o a qué tiene acceso.

En la siguiente imagen se puede ver el árbol de componentes

### Test Plan

Test Plan es el componente más alto de la jerarquía de componentes. Es el elemento raíz de un test, donde se especifican la configuración general y los demás componentes que lo contienen. 

 - Podemos modificar el nombre del Test Plan.

 - In *User defined variables* podemos definir variables para el test

 - La opción *Run Thread Groups consecutively* indica que a JMeter que si existe más de un thread group, JMeter los ejecutará uno después de otro en lugar de hacerlo de forma paralela.

 - *Run teardown Thread Groups after shutdown of main threads* indica que los thread groups serán ejecutados graceful después de que los threads principals hayan terminado, pero no se ejecutarán si el test se ve forzado a parar.

 - *Functional Test Mode* indica que JMeter guardará las respuestas en unos ficheros de resultados. Estos ficheros pueden crecer mucho y afectar en el rendimiento de JMeter. Así que, sólo activar esta opción cuando se ejecuten tests pequeños.

### Thread Groups

Un Thread Group es el punto de entrada de un test. Controla el número de threads o usuarios que JMeter usará para ejecutar el test.

Es un hijo del componente Test Plan. 

Podemos crear un **setup thread group** y **teardown thread group** que son cosas que queremos hacer antes o después de cada **thread group** normal.

Cuando creamos un **thread group** podemos: 

 - establecer qué acción se toma después que se produzca un error en el test.

 - *Number of Threads(users)*, indica el número de usuarios para el test

 - *Ramp-Up Period*, el tiempo que JMeter se toma para crear los usuarios

 - *Loop Count*, el número de veces que queremos que JMeter ejecute el test, o si queremos que lo ejecute por siempre.

 - *Delay Thread creation*, indica a JMeter que no cree los usuarios hasta que sea necesario. 

 - *Scheduler* te permite planificar la duración de la ejecución del **thread group** y después de cuántos segundos comenzará

### Elementos de configuración

 Hay varios elementos de configuración. Se suelen usar para establecer una configuración por defecto y variables que se usarán más tarde en otros elementos o componentes.

 Se pueden configurar desde cabeceras HTTP, conexiones JDBC, configuración de Login, variables aleatorias o definidas por el usuario, etc... 

### Controladores y muestras

Los controladores son hijos del componentes Thread Group. Existen dos tipos de controladores: 

 - Controladores lógicos

 - Muestras

#### Controladores lógicos

Este tipo de controladores nos permite personalizar la lógica de decidir cuando se envían peticiones. 

#### Muestras o samplers

Realiza una petición de un cierto tipo generando uno o más resultados. 

JMeter viene ya integrado con varios controladores lógicos y también con varios samplers. 

### Temporizadores

JMeter ejecuta las peticiones secuencialmente, sin pausa. Los temporizadores nos permiten introducir una espera entre peticiones. Por ejemplo, se puede simular el tiempo que se toma un usuario en hacer una acción en la aplicación, y así, crear un test más acorde con la realidad.

Este tipo de componente se puede poner en cualquier nivel de jerarquía del árbol de componentes, pero se suelen poner debajo de los samplers o controladores a los que va a afectar. 

### Aserciones

Una vez el servidor responde, lo normal es que queramos verificar algo de esa respuesta, por ejemplo que las respuesta tenga un JSON específico, que la petición haya tomado un tiempo concreto, etc...

Las aserciones se pueden añadir a nivel de **Thread Group** o a nivel de **Sampler**. 

Las aserciones de tipo HTML, XML o XPath no son recomendadas ya que consumen muchos recursos.

### Listeners

JMeter recoge información de las peticiones que realiza. Los listeners se adjuntan a las respuestas y agregan métricas. 

Los listeners se pueden añadir a cualquier nivel, pero solo recogen información de los niveles inferiores de donde se añade. 

Los listeners más usados son:

 - View Results Tree, que muestra un árbol de las respuestas recibidas durante el test. 

 - Summary Report, que crea una tabla para cada petición el test y muestra varias estadísticas.

 - Aggregate Report, similar al Summary Report pero que usa más memoria.

Los listeners pueden escribir la información en un fichero y en distintos formatos.

Hay que tener en cuentas dos cosas con los listeners:

 1.- Todos los listeners usan la misma información del test, pero la presentan de forma distinta

 2.- Consumen mucha memoria, sobre todo si las respuestas son grandes.

### Orden de ejecución y reglas

Los elementos o componentes de un test pueden ser de dos tipos:

 - Ordenados, como los controladores y samplers, que se ejecutan en el orden en que aparecen en el test

 - Jerárquicos, los otros tipos de elementos, que se ejecutan acorde a su ámbito.

Por ejemplo, un ejemplo de un Test Plan ordenado

()[jmeter_test_plan_ordered.png]

primero se ejecuta el sampler *Home*, luego el sampler *Catalog*, luego el sampler *Detail* y por último el sampler *Login*.

Este ejemplo es de un Test Plan que contiene elementos jerárquicos

()[jmeter_test_plan_hierachical.png]

Aquí, el assertion *Response Assertion 1* aplica al ámbito del *Home* sampler, y el assertion *Response Assertion 2* aplica al ámbito del *Transaction Controller* *Catalog* y *Book Detail*. Así que el orden de ejecución será: Home -> Response Assertion 1 -> Catalog -> Response Assertion 2 -> Book Detail -> Response Assertion 2.

En la siguiente imagen se muestra el orden de ejecución de los elementos de un Test Plan

!()[jmeter_execution_order]

### Reglas

Para los elementos de configuración debemos tener en cuentas dos reglas:

 - Las variables definidas por el usuario se procesan al inicio del test, no importa dónde se coloca este elemento

 - Un elemento de configuración dentro de una rama del árbol más interna tiene prevalencia frente a un elemento de configuración del mismo tipo en una rama más externa.

# Recogiendo y analizando los resultados de los tests

## Utilizando listeners



## Caso práctico: Envío masivo de comunicados

Un gestor puede enviar de manera masiva un comunicado a una gran cantidad de candidatos. Como requisitos podríamos establecer que:
 
 - El gestor debe poder enviar más de 10000 comunicados a candidatos en una petición
 - El tiempo de respuesta de que el gestor realice un envío masivo de comunicados a candidatos debería ser menor que 1 segundo.
 - El sistema debería poder soportar usar esta funcionalidad más de 20000 usuarios a la vez.

### Tests de carga

#### API sin código

Test de carga de 100 usuarios que realizan una petición con un segundo entre cada usuario:
 - 116 petis/min
 - Avg 6 ms

#### API con command handler pero sin ejecutar caso de uso

Test de carga de 100 usuarios que realizan una petición con un segundo entre cada usuario:
 - 112 petis/min
 - Avg 13 ms

#### API con command handler ejecutando caso de uso sin publicar eventos de domimio ni enviar comunicados

Test de carga de 100 usuarios que realizan una petición con un segundo entre cada usuario:
 - 113 petis/min con desviación de 6
 - Avg 23 ms 

#### API publicando solo los eventos de dominio

Test de carga de 100 usuarios que realizan una petición con un segundo entre cada usuario:
 - 109 petis/min con desviación de 691
 - Avg 3500 ms  

#### API publicando los eventos de dominio solo transformando el evento a JSON y validando el esquema

Test de carga de 100 usuarios que realizan una petición con un segundo entre cada usuario:
 - 113 petis/min con desviación de 337
 - Avg 2000 ms  

#### API publicando solo los eventos de dominio con la configuración por defecto del batch del producer

Test de carga de 100 usuarios que realizan una petición con un segundo entre cada usuario:
 - 110 petis/min con desviación de 655
 - Avg 4385 ms   

#### API publicando solo los eventos de dominio con la configuración custom del batch del producer (batch.size: 664000)

Test de carga de 100 usuarios que realizan una petición con un segundo entre cada usuario:
 - 110 petis/min con desviación de 442
 - Avg 3397 ms    