# Performance testing

Los tests de rendimiento deberían incorporarse en una fase temprana del proyecto y continuar en el ciclo de desarrollo de éste.
Dejar este tipo de tests al final de proyecto no es nada recomendable.

## Qué son tests de rendimiento

Los tests de rendimiento nos sirven para ver cómo un sistema (aplicación, base de datos, ...) se comporta en condiciones de carga para 
ver cual es su impacto en el sistema. También pueden ser útiles para detectar **cuellos de botella** y **problemas de rendimiento**.

Esto es crítico puesto que puede **afectar** en la **obtención de ingresos** y en la **retención de cliente**.

Los tests de rendimiento se deben ejecutar una vez hayan pasado todos los tests funcionales (de aceptación, 
unitarios, ... bajo nuestro criterio), después que estemos seguros que la aplicación funciona correctamente. Pero, estos tests
no son solo para medir velocidad. En particular, podemos medir:

1.- Tiempo de respuesta (response time): el tiempo que tarda el sistema en responder a una petición de cliente. Realmente, el **tiempo de 
  respuesta es** la suma de:
  - el *tiempo desde que se realiza la petición hasta que alcanza al sistema* (servidor). A este tiempo hay que sumar cierto
    tiempo de **latencia de red**.
  - el *tiempo de procesar la petición* y generar una respuesta. Este tiempo depende de la complejidad de la petición y de 
    las características del hardware del servidor.
  - el *tiempo que pasa hasta que se envía la respuesta al cliente*. A este tiempo hay que sumar cierto
    tiempo de **latencia de red**.
  
  **Cuanto menor sea el tiempo de respuesta mejor será nuestra aplicación**.

2.- Rendimiento (throughput): se puede medir de dos formas:
  - El número de transacciones (peticiones con sus respuestas) por unidad de tiempo (millis o segundos)
  - El número de kbs o bytes (enviados y recibidos) por unidad de tiempo (millis o segundos)
  **Cuanto mayor sea el dato mejor**.
  
3.- Realibility: mide cómo de bien la aplicación detecta y maneja los errores en términos de número de errores 
  por número de peticiones. **Cuanto menor sea el dato mejor**.

4.- Escalabilidad: no es sólo una medida, nos dice cómo de bien el sistema puede expandirse en términos de tiempos de respuesta,
  rendimiento y porcentaje de errores cuando al sistema se le añaden nuevos recursos, funcionalidades, etc... Por ejemplo, 
  la escalabilidad nos ayuda a saber qué podríamos necesitar en nuestro sistema para tener una carga de 10000 clientes o usuarios.
  
  Podríamos optar por **escalabilidad vertical**, añadiendo más memoria o CPUs a nuestro servidor, o **escalabilidad    horizontal**, 
  añadiendo servidores en un clúster.

## Proceso de tests de rendimiento

### Requisitos de rendimiento

Antes de crear un sistema es conveniente saber qué requisitos de rendimiento necesitamos en nuestro sistema. Éstos, a veces 
nos vienen dado por contrato o acuerdos, y otras, somos nosotros los que debemos establecer esos requisitos antes de testear 
el sistema.

Ejemplos de este tipo de requisitos podría ser: el tiempo medio/máximo de respuesta debería ser 800 ms, el sistema debería ser
capaz de soportar 60 páginas por segundo, o el sistema debería ser capaz de soportar al menos 10000 usuarios por hora.

### Diseño y construcción de los tests

Una vez tenemos estos requisitos, podemos diseñar y construir unos tests.

Tenemos que tener claros cuales son los patrones de uso de la aplicación, por ejemplo, cuántos comunicados queremos que se 
envíen por minuto?, o cuántos gestores pueden navegar a la vez en la gestión de candidatos en un momento dado?. Esto ayuda a
diseñar nuestros tests y construirlos con JMeter.

### Preparción del entorno de test

Deberíamos tener un entorno lo más similar posible al entorno de producción. 

Luego, para ejecutar los tests, lo primero que tenemos que hacer es validar el script del test y los datos para comprobar que
todo funciona como debe. Debemos comprobar los logs para ver si hay errores y luego analizar si éstos son producidos por 
los tests, la aplicación, el servidor o una combinación de estos.

Después de analizar los resultados, se toman acciones para solucionar esos problemas como: optimizar consultas a la base 
de datos, incrementar la memoria disponible para la aplicación, o cualquier otra cosa. 

Después de cada cambio realizado, se vuelven a ejecutar estos tests de rendimiento y se comparan los resultados obtenidos 
con los anteriores para comprobar que dan resultados positivos, de lo contrario, se siguen haciendo cambios y se vuelve a 
repetir el proceso hasta que los requisitos de rendiemiento se satisfagan.

## Tipos de tests de rendimiento

Existen dos maneras de generar tests de carga:

 1.- Incrementar el número de usuarios que acceden a la aplicación al mismo tiempo.
 2.- Incrementar el número de peticiones que la aplicación tiene que gestionar, independientemente del número de usuarios.
 
Una vez decidamos qué tipo de carga que vamos a usar debemos decidir qué tipo de test vamos a ejecutar. 

El primer tipo de test que deberíamos ejecutar es un **Smoke Test**, que es un test con una carga ligera, generalemente un usuario para comprobar que el test se funciona bien.

Después, realizamos un **Load Test** (test de carga), que es un test realizado con un nivel de carga específico. Normalmente, 
haremos varios de estos tests con diferentes niveles de carga para ver cómo se comporta la aplicación.

Haremos un **Stress Test** con un nivel de carga que supera los niveles de carga normales para ver hasta qué punto
la aplicación permanece estable y respondiendo. En un test de estrés, iremos incrementando los usuarios en pasos o de forma
constante en un periodo de tiempo, o podemos incrementar los usuarios en un gran número durante un largo periodo de tiempo 
para averiguar en qué punto la aplicación deja de funcionar.

Tenemos también los **Spike Test**, en los que la aplicación es sometida a picos repentinos de carga que superan los límites de 
su capacidad para ver si después de esos picos de carga la aplicación es lo suficientemente **robusta** como para seguir 
funcionando durante y después del Spike Test.

Los **Endurance Test** son tests en los que la aplicación es sometida a un carga dentro de los límites que ésta puede 
soportar pero durante un largo periodo de tiempo, horas o en algunos casos días, para ver si la aplicación tiene memory leaks
o no cierra bien las conexiones a base de datos o entrada y salida.

### Spike Test en JMeter

