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
  
  Podríamos optar por **escalabilidad vertical**, añadiendo más memoria o CPUs a nuestro servidor, o **escalabilidad horizontal**, 
  añadiendo servidores en un clúster.
  
### Requisitos de rendimiento

Antes de crear un sistema es conveniente saber qué requisitos de rendimiento necesitamos en nuestro sistema. Éstos, a veces 
nos vienen dado por contrato o acuerdos, y otras, somos nosotros los que debemos establecer esos requisitos antes de testear 
el sistema.


