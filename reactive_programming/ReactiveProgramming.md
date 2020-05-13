# Programación reactiva

## Por qué usar programación reactiva

Antes de ver qué es la programción reactiva tenemos que entender el por qué usar PR y qué problema resuelve. 

De 10-15 años a la actualidad las cosas han cambiado bastante. El software ha crecido mucho, y con ello las instalaciones del software. Por ejemplo, hace 10-15 años las aplicaciones las instalábamos en pocos nodos, en la actualidad, se siguen instalando en poco nodos, pero también en decenas, cientos e incluso miles de nodos. 

También, el manejo de la información ha cambiado, antes, la información cambiaba poco, quizá en procesos batch que se dedicaban a actualizar esa información por la noche, ahora, la información cambia permanentemente y debe estar actualizada. Antes manejábamos gigas de información y ahora se llegan hasta petabytes de información, y esa información la vamos a tener que consumir.

Antes veíamos que las aplicaciones daban mensajes de mantenimiento de servicio, bien por que se actualizaba el software o se lanzaban scripts de base de datos. Esto ahora no es aceptable. Seguimos teniendo que actualizar software y actualizar bases de datos pero sin tener que afectar al usuario final.

Pero al final esto son detalles técnicos, pero lo que se intenta solucionar es un cambio en las esperanzas de los usuarios modernos. Así que el cambio actual es que los usuarios modernos interactúen con el software para hacer su trabajo y su día a día mejor. El software que no es responsive al usuario al final acaba frutándole y provocando que deje de usar el software, perder tiempo en su trabajo, etc... De hecho, un usuario que utiliza un producto que no es responsive, si encuentra otro producto que ofrezca la mismas features pero que son responsive, lo normal es que se quede con él.

Por lo tanto, **el primer objetivo de la programación reactiva es proporcionar al usuario una experiencia responsive bajo cualquier circunstancia**.

Pero, ¿cómo podemos llevar a cabo este objetivo?. Deberíamos ser capaces de construir software que sea capaz de:

 - Escalar de 10 a 10 millones de usuarios. Se trata de crear un software que, desde su inicio no tenga por qué soportar 10 millones de usuarios sino que, a medida que vaya creciendo en número de usuarios la arquitectura esté diseñada para ser capaz de soportarlos en un futuro.

 - Consumir los recursos necesarios para soportar la carga actual. No deberíamos estar en el escenario de tener una infraestructura que soporte 10 millones de usuarios cuando no tenemos esa necesidad. A nivel de costes sería poco eficiente.

 - Los errores no afecten al usuario, que no tengan gran impacto sobre ellos. Errores van a haber siempre, pero hay que intentar que éstos no les suponga un dolor.

 - Ser distribuído. Para que los tres puntos anteriores se cumplan, nuestro sistema debería poder distribuirse entre decenas, centenas o miles de máquinas. Esto no es una solución a los tres puntos anteriores pero sí nos da muchas ventajas:
    - En cuanto a la escalabilidad, ya que podríamos distribuir nuestro software en muchas máquinas y soportaríamos más usuarios.
    - En cuanto al manejo de errores, tampoco solucionaría completamente la distribución, pero algunas máquinas pueden estar caídas mientras que otras pueden suplirlas y no perderíamos disponibilidad.

 - Mantener un nivel consistente de calidad y sensibilidad al usuario (responsive). Es decir, no podemos ser capaces de soportar miles de usuario a costa de que el usuario tenga un alto tiempo de respuesta cada vez que usa nuestra aplicación.

Si conseguimos en gran medida este tipo de cosas, podríamos decir que hemos construído un software reactivo

## Principios reactivos

Lo primero de todo es entender [el manifiesto reactivo](https://www.reactivemanifesto.org/). Hay 4 principios básicos que un sistema reactivo debe cumplir:

 - **Responsive**, es el más importante. Un sistema reactivo responde siempre de forma consistente y oportuna.

 - **Resiliente**, esto quiere decir que un sistema reactivo debe ser responsive incluso cuando el sistema falla. Si nuestro sistema es responsive excepto cuando ocurre un fallo, es que no es totalmente responsive, y más cuando ese fallo se puede producir frecuentemente.

 - **Elástico**, un sistema reactivo debe ser responsibe a pesar de la carga del sistema. El que el sistema sea elástico no es lo mismo que sea escalable, porque cuando nos referimos a escalabilidad solemos hacerlo siempre en el sentido de soportar más carga. En cambio, con la elasticidad nos queremos referir a que podemos escalar hacia arriba si la carga es alta, pero también escalar hacia abajo si la carga deja de ser alta.

 - **Dirigido por mensajes**, un sistema reactivo debería ser dirigido por mensajes (o eventos, pero con el término mensajes se quiere tener un ámbito mayor que el término evento) asíncronos no bloqueantes.

 ### Responsiveness

 Responsiveness es la piedra angular de usabilidad. Esto significa que el sistema de responder de una forma rápida y consistente, siempre que sea posible. Hay situaciones en las que es imposible ésto, por ejemplo, si todo el hardware se viene abajo. Pero el objetivo es tener un sistema que sea lo más responsive posible siempre que podamos y tan a menudo como sea posible.

 Una de las razones de construir un sistema responsive es la confianza. Si nuestro sistema no es responsive, falla a menudo, no se le da información consistente, los usuarios terminarán perdiendo confianza y dejarán de usar nuestro software.

 ### Resilencia

 La resilencia hace que nuestro sistema sea responsive frente a los fallos que haya en el sistema. La resilencia puede conseguirse de varias maneras: 
  - **replicación**, teniendo varias copias del software
  - **aislamiento**, el software es autónomo para funcionar, no necesita dependencias externas
  - **contención**, es una consecuencia del aislamiento, se refiere a que si un servicio tiene un fallo, éste no se propaga a otros servicios, ya que el servicio está aislado.
  - **delegación**, se refiere a que la recuperación se gestiona por un componente externo.

La clave de la resilencia es que si ocurre un fallo en un servicio, éste no se propaga hacia otros servicios haciendo que todo el sistema se venga abajo. Este es un problema que suele ocurrir en sistemas monolíticos.

### Elasticidad

La elasticidad hace que nuestro sistema sea responsive frente al incremento o decremento de la carga de nuestro sistema. La elasticidad implica que haya contención **cero** y no haya cuellos de botella. Podemos tener métricas que nos predigan cuando vamos a tener un pico de carga y cuando deberíamos reducir recursos para ahorrar en costes, y esto nos lo va a permitir la elasticidad de nuestro sistema.

### Message Driven

Una arquitectura dirigida por mensajes ayuda a tener un sistema responsive, resilente y elástico. Los mensajes deben ser asíncronos y no bloqueantes. Esto nos da bajo acoplamiento, aislamiento y localización transparente. La idea de trabajar con mensajes asíncronos no-bloqueantes es evitar el tiempo de respuesta que genera el consumir un mensaje y estar esperando la respuesta. Evidentemente, en muchas situaciones querremos tener una respuesta, pero ésta vendrá de forma asíncronamente.

## Qué es una aplicación reactiva

Un ejemplo de software reactivo es Netflix. Netflix tiene algunas features como la lista de películas que has quieres ver o la lista de películas que estás viendo que, cuando estas features fallan no hacen que falle toda la aplicación de Netflix sino que, directamente las dejamos de ver en la aplicación, pero aún así podemos ir a buscar la película que estabamos viendo y continuar su reproducción.

De esta manera, aunque esas features estén dando algún tipo de error, ya sea porque fallen, porque estén en un proceso de migración, etc..., el usuario no ve ningún tipo de error, sólo que deja de ver temporalmente esa feature.

Otro ejemplo de software reactivo es Git. Git usa **mensajes asíncronos** no bloqueantes. Por ejemplo, yo puedo hacer una Pull Request y esperar a que alguien la valide, pero mientras yo puedo seguir realizando mi trabajo. Mientras Git envía mensajes a otros usuarios para que la revisen y cuando está completa nos avisa.

Por otro lado, Git es **resilente** ya que cada usuario tiene una copia local del repositorio remoto, y si por cualquier razón el repositorio remoto de Git se cae, nosotros podemos seguir realizando nuestro trabajo. Incluso podríamos seguir trabajando con otros usuarios enviando nuestros cambios a ellos. Nuestra máquina está aislada. 

También es muy **elástica** ya que, se pueden crear varias copias del repositorio en varias máquinas. Sobre el mismo repositorio pueden trabajar cientos, miles de usuarios sobre el mismo conjunto de datos.

Con estas características de mensajería asíncrona, resilencia, elasticidad, hacen que Git sea **responsive**, la experiencia que tenemos con Git es que siempre funciona, no nos limita a la hora de realizar nuestro trabajo

## Programción reactiva

En el mundo de reactivo se hablan de varios términos como programación reactiva, sistemas reactivos y arquitectura reactiva que a menudo son malentendidos. 

Por ejemplo, podemos tener un sistema que esté compuestos de microservicios que estén diseñados para ser reactivos, éstos serían **sistemas reactivos**. Dentro de estos sistemas reactivos podemos tener técnicas de **programación reactiva** como Futures, RxJava, Reactive Streams. 

Generalmente, para construir sistemas reactivos se usan estas técnicas pero, usar técnicas de programación reactiva no quiere decir que el sistema resultante sea reactivo. Usar programación reactiva no implica que se apliquen los principios de **arquitectura reactiva**.

### Sistemas reactivos

Cuando se construyen sistemas reactivos, éstos son construídos siguiendo los principios del manifiesto de arquitectura reactivas. 

Esto significa que los principales componentes (microservicios) del sistema interactúan de una forma reactiva, deben seguir el principio de ser **dirigidos por mensajes**. También hay que estar seguros que sean **elásticos**, **resilentes** y **responsive**.

Para hacer esto, los sistemas reactivos están separados por contextos asíncronos, por lo tanto, los microservicios deben comunicarse de forma asíncrona mediante mensajes asíncronos. Microservicios reactivos son un ejemplo de construir un sistemas reactivos.

### Programación reactiva

Por otro lado, la programación reactiva ayuda a la construcción de sistemas reactivos. Pero, ¿cómo es posible que usando programación reactiva no construyamos un sistema reactivo?, fácilmente, desplegando el microservicio en un único nodo no conseguimos que sea resilente, ya que, si se cae el nodo la aplicación deja de estar disponible. También, el sistema no sería reactivo si el microservicio no es elástico. En resumen, podemos construir un microservicio que use técnicas de programación reactiva pero que el sistema resultante no sea reactivo.

## El Modelo Actor

El **Modelo Actor** es una herramienta más de programación reactiva, es un paradigma de programación que ayuda a construir sistemas reactivos. Usar este paradigma, otra vez, no implica que logremos construir un sistema reactivo. 

Siempre hay que tener en mente los principios de arquitectura reactiva para lograr construir un sistema reactivo.

Pero, el **modelo actor** proporciona facilidades para fácilmente soportar todos los principios de arquitectura reactiva. 

Por su naturaleza, el modelo actor es dirigido por mensajes. Cuando se construye un sistema basado en el modelo actor, toda la comunicación entre actores se hace a través de mensajes asíncronos.

También proporciona abstracciones para construir sistemas elásticos y resilentes. Del hecho que el modelo actor es dirigido por mensajes, hace que sea muy fácil hacerlo elástico y resilente, y ésto ayuda a construir software responsive.

En la JVM, Akka implementa el modelo actor. El modelo actor no es algo nuevo sino que el concepto fue creado en los años 70, y la idea fundamental que había detrás de ello era que toda la lógica computacional estaba dentro del Actor.

### Fundamentos del modelo actor

1. Toda la computación ocurre dentro del actor.

2. Cada actor tiene una dirección única.

3. La comunicación entre actores se hace a través de mensajes asíncronos, es la única forma en la que se pueden comunicar los actores.

Al ser el modelo actor dirigido por mensajes por naturaleza, proporciona algo conocido como **localización transparente**. Todos los actores se comunican utilizando la misma técnica a pesar de dónde se encuentren los actores. 

Como hemos dicho en el punto 2, cada actor tiene una dirección única, esto quiere decir que pueden estar en la misma JVM o en distintas JVMs. Para comunicarse, los actores mandan mensajes sin saber a dónde los mandan, solo saben enviar mensajes y si al actor que va dirigido es local o remoto, el actor no lo sabe, no hay una técnica específica para mandar mensajes a actores locales y remotos, se usa la misma técnica. Esto es la esencia de la **localización transparente**.

Con esta aproximación, los actores pueden desplegarse en distintos servidores con lo que conseguimos la resilencia, ya que si uno de ellos falla no hace fallar a todo el sistema a la vez. 

También conseguimos elasticidad, pues si tenemos un alto nivel de carga podemos desplegar más instancias de los actores o si por el contrario, nuestras necesidades de carga son menores, podemos eliminar instancias de los actores, con lo que conseguimos la elasticidad.

Una cosa a tener en cuenta es que hay muchas herramientas de programación reactiva pero que no siempre nos van a facilitar construir software reactivo. De hecho, la mayoría solo soportan algunos de los principios reactivos. Para construir un sistema reactivo, muchas veces tenemos que recurrir a combinar varias herramientas para llegar a construir un sistema reactivo, por ejemplo, incluir balanceadores de carga para conseguir la elasticidad. El modelo actor proporciona ayudas para llegar a construir sistemas reactivos:

 - Es dirigido por mensajes

 - La localización transparente soporta elasticidad y resilencia a través de la distribución

 - La elasticidad y resilencia proporciona responsiveness.

Pero aún así, hay que tener en cuenta que, podemos usar el modelo actor y aún así no construir un sistema reactivo.

### Sistemas reactivos sin el modelo actor

Se pueden construir sistemas reactivos sin el modelo actor pero, veamos qué implicaciones tiene eso. 

Por lo pronto, para conseguir la localización transparente necesitaríamos un balanceador de carga y un servicio de registro para así no saber cuántas instancias de los servicios tenemos desplegados (balanceador) ni dónde están esos servicios (servicio de registro).

La comunicación asíncrona la podemos conseguir a través de un bus de mensajes. 

Con la combinación del balanceador de carga y el bus de mensajes conseguimos la resilencia y elasticidad. Podemos desplegar varias instancias de los servicios, pudiendo escalarlos hacia arriba y hacia abajo, y para la resilencia podemos recuperarnos si algún evento falla.

En definitiva, podemos tener un sistema reactivo. Pero el precio que se paga es a costa de crear mucha infraestructura: el balanceador de carga, el servicio de registro, un bus de mensajes. Estas cosas las necesitamos para construir un sistema reactivo. Pero esto es reactivo a escala de microservicios pero no es necesariamente reactivo en los microservicios, es una de las principales diferencias con el modelo actor: el modelo actor puede ser reactivo a nivel de actores y los actores están dentro de un microservicio. Podemos tener varios actores dentro de un único microservicio.

El resumen, es que con el modelo actor se pueden crear sistemas reactivos más fácilmente que sin usarlo.

