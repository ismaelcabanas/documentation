# Arquitectura reactiva: Domain Driven Design

Las reglas usadas en DDD son altamente compatibles con las usadas en arquitecturas reactivas. Uno de los principales objetivos de DDD es lidiar con dominios complejos para descomponerlos en subdominios más pequeños y así reducir la complejidad inicial. Por este motivo, DDD propone establecer subdominios acotados, que tienen sus propias reglas de negocio, independientes de otros subdominios acotados, y la conjunción de estos subdominios construyen la solución a un gran dominio.

Los microservicios reactivos tienen un objetivo parecido: necesitan estar claramente acotados. En este caso, las acotaciones de los microservicios reactivos deben ser asíncronos. Los microservicios deben tener un API bien definida y tener ciertas responsabilidades. Lo complicado aquí es determinar cuales son esas acotaciones.

Entonces, en este punto es donde entra DDD, que nos ayuda con sus técnicas a delimitar esas acotaciones que necesitan los microservicios reactivos, a romper un dominio grande y complejo en otros dominios (subdominios) más pequeños y fáciles de resolver.

## Descomponiendo el dominio

Lo que propone DDD es que si tenemos un dominio grande y complejo lo descompongamos en subdominios más pequeños y manejables. Estos subdominios se crean agrupando ideas, reglas de negocio, acciones. 

Podemos partir de una serie de conceptos y a partir de ahí intentar establecer los subdominios. Puede ocurrir que, y de hecho es bastante frecuente, ciertos conceptos estén en varios subdominios, pero que pueden tener o no distinto significado en cada subdominio. 

Es importante evitar el concepto de abstracción. Por ejemplo, que si en varios subdominios existe el concepto de Customer, hay que evitar realizar una abstracción de Customer para que se utilizada por todos los subdominios. Esto es un error, porque los detalles que pueda tener Customer en un subdominio pueden ser distintos en otro subdominio.

Al final, cada subdominio termina formando su propio lenguaje ubícuo y modelo. El lenguaje ubícuo y el modelo es lo que se llama **bounded context** (contexto acotado). Suele haber una relación uno a uno entre subdominio y bounded context.

Cada bounded context o subdominio es un buen punto de partida para construir un microservicio reactivo. Incluso, ese bounded context puede ser implementado por varios microservicios, y hasta conclusión podemos llegar más adelante, habiendo construido un microservicio que implemente el bounded context, eso ya depende de nuestro diseño.

Cuando hablamos de bounded contexts, el significado de una palabra en un BC puede tener un significado completamente distinto en otro BC. 

Pero, cómo podemos establecer o delimitar un BC?, no existe una regla universal que nos diga cómo delimitar nuestros BCs. Existen algunas pistas: 
 - Si diferentes actores realizan tareas distintas puede ser una opción para empezar a delimitar un BC.
 - Buscar cambios en el lenguaje ubícuo, si el mismo concepto tiene distintos significados según se hable 
 - Si estamos implementando un flujo de trabajo que resulta complicado, enrebesado, etc... es una pista de que o bien no hemos entendido bien el dominio o hemos descompuesto incorrectamente el BC
 - Si un BC tiene demasiadas dependencias es otro indicativo de que no has descompuesto bien el BC

## Event first DDD

DDD se basa mucho en los objetos que modelan nuestro negocio, pero últimamente, estas técnicas han evolucionado un poco y ahora se está considerando la aproximación de modelar primero los eventos que ocurren en nuestro dominio: antes que ver cuáles van a ser estos objetos, primero vemos qué ocurre en estos objetos o qué están haciendo.

Examinando estos eventos primero, lo que hacemos es agruparlos de forma lógica y a partir de aquí podemos establecer sistemas acotados. Esto se conoce como la técnia de **Event Storming**. 

## Actividades del dominio

Dentro de un dominio hay una serie de actividades con las que podemos tratar: Commands, Events y Queries.

### Commands

Este tipo de actividad indica que algo ocurre o va a ocurrir en el dominio. Representa una petición para realizar una acción, pero no una acción que deba realizarse ya, sino que se realizará. 

Los comandos van dirigidos a un receptor específico, que tiene la opción de rechazarlo.

Los comandos suelen cambiar el estado del sistema una vez hayan sido ejecutados.

Cuando se ejecuta una comando, por lo general, no se espera una respuesta, como mucho puedes esperar un mensaje diciendo que "ok, procesaré tu petición".

Ejemplos de comandos son: crear un proceso, eliminar un seguimiento, ... Con esto no se quiere decir que creamos un proceso o eliminamos un seguimiento, sino que alguien realizará la acción de crear un proceso y eliminar un seguimiento.

### Events

Los eventos son un tipo de actividad que indican que algo ocurrió en nuestro dominio, algo que pasó en el pasado. Mientras un comando es algo que va a suceder en el futuro, un evento es algo que ocurrió en el pasado.

A diferencia de los comandos, los eventos no pueden ser rechazados ya que se ejecutaron en algún momento. Lo que se puede decidir no hacer nada cuando llega la hora de tratarlo.

Los eventos, normalmente, se dirigen a varios receptores.

Los eventos registran un cambio en el dominio, en el estado del sistema, de tal forma, que son una consecuencia de la ejecución de un comando.

Ejemplos de eventos son: Proceso creado, proceso desactivado, ...

### Queries

Las consultas son un tipo de actividad que representan una petición de información sobre el dominio. Siempre se espera una respuesta cuando se realiza una consulta.

La respuesta de una consulta se entrega, normalmente, a un receptor. 

Las consultas no deben cambiar el estado del dominio.

Algunos ejemplos de consultas son: comprueba si el proceso está activo, dame todos los procesos de un cliente, ...


En un sistema reactivo, los comandos, las consultas y los eventos son los mensajes de los que hablamos cuando hablamos que una arquitectura reactiva debe ser dirigida por mensajes.

## Objetos del dominio

### Value Objects

Un Value Object viene definido por sus atributos. Por lo tanto, dos Value Objects son iguales si todos sus atributos son idénticos.

Un Value Object es inmutable. Si cambios alguno de los atributos del Value Object, éste se convierte en un nuevo Value Object. 

Además de contener un estado, los Value Objects pueden contener lógica de negocio (es bastante deseable).

Un ejemplo de Value Object sería una dirección.

En un sistema reactivo, los mensajes se implementan como Value Objects.

### Entities

Una entidad está definida por una clave o identificador. 

Una entidad puede cambiar sus atributos pero no su identidad. Por lo tanto, una entidad es mutable. 

Una entidad puede contener lógica de negocio.

Un ejemplo de entidad sería una persona identificada por su nombre, por ejemplo.

En un sistema reactivo que use Akka, por ejemplo, los Actors representan el concepto de entidad.

### Aggregates

Un agregado es un tipo especial de entidad. Es una colección de objetos de dominio que están acotados a una entidad raíz, Aggregate Root.

Un ejemplo de agregado raíz podría ser persona que estaría formada, por ejemplo, por value objects como dirección, nombre, número de teléfono,...

El acceso a los objetos dentro de un agregado raíz debe hacerse a través de éste.

Las transacciones no deberían incluir más de un agregado raíz, si nos encontramos en esta situación es un indicativo que nuestro agregado raíz no está bien diseñado.

En sistemas reactivos, los agregados son buenos candidatos para distribuirlos.

Pero, lo complicado en DDD es encontrar estos aggregados raíz en un bounded context. Lo normal es que haya un único agregado raíz por bounded context, pero no tiene que ser siempre así. Una de las preguntas que nos deberíamos hacer es si la entidad, que es candidata a ser agregado raíz,:
 - está implicada en la mayoría de las operaciones que se realizan en el bounded context?
 - si eliminamos la entidad esto supone que eliminemos otras entidades?
 - una transación engloba varias entidades?, si la respuesta es sí, entonces podemos estar seguros que hemos realizado una mala elección del agregado raíz. 

### Services

Cuando hay cierta lógica de negocio que no se ajusta o no encaja en una entidad o value object, ésta lógica puede encapsularse en un servicio de dominio.
