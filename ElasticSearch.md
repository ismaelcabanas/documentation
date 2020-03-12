# Elastic Search

Elasticsearch es un motor de búsqueda open source altamente escalable. A pesar de que empezó como un buscador de texto 
ha evolucionado como una herramienta de análisis, que soporta no solo búsquedas simples sino también agregación compleja 
de información. Su naturaleza distribuida permite su escalabilidad a medida que la cantidad de información que 
contiene crece, sin perder rendimiento de forma significativa.

Elasticsearch está basado en Apache Lucene (al igual que Apache Solr) y está implementado en Java. Lucene provee busqueda 
e indexación, Elasticsearch amplía Lucene añadiendo una cómoda API REST basada en JSON para facilitar la comunicación 
con Lucene y además provee las características de sistema distribuido que permite escalar la herramienta de forma casi 
transparente al usuario.

## Cómo funciona una búsqueda

Cualquier aplicación que se precie necesita buscar de alguna forma la información que posee. **Buscar** es encontrar los 
documentos más relevantes que existan que cumplan con los criterios de búsqueda. Esto parece sencillo pero, en un motor de
búsqueda ocurren varias cosas por detrás para llevar a cabo esto.

1.- El motor de búsqueda debe tener conocimiento de la existencia de documentos, encontrarlo e indexarlo.
2.- La indexación de un documento es necesario para que se puedan hacer búsquedas en el futuro.
3.- Por cada palabra de búsqueda que realiza el usuario, tenemos que conocer cómo de relevante es el documento que la contiene. La relevancia es la combinación de los términos de búsqueda más el documento en sí.
4.- La búsqueda en sí misma.

Estos 4 componentes podrían resumir los componentes necesarios para realizar una búsqueda.

Hay varias tecnologías especializadas en cada uno de estos pasos. Por ejemplo, para el punto 1, un rastreador web rastrea todas las páginas con sus links para tener una visión de todos los documentos que existen. En el punto 2, se indexa cada documento encontrado por el rastreador, es decir, se parsean para que los términos individuales se extraigan y almacenen en una estructura de datos llamada el índice invertido. **El índice invertido es un mapeo de un término al documento donde el término es encontrado**. Es invertido porque va desde el término de búsqueda a la página web del documento. Basado en lo que el usuario buscaba, cada documento tendrá asociado una puntuación llamada puntuación relevante (punto 3). Los documentos tienen que estar puntuados antes que se devuelvan los resultados para que aquellos documentos con las puntuaciones más altas se muestren en los más alto del resultado de la búsqueda. La relevancia es calcula en función del término de búsqueda, así como el documento en sí mismo. Por último (punto 4) el proceso de la búsqueda en sí misma, se buscan los documentos en el índice invertido, encuentra los más relevantes y los devuelve en lo más alto del resultado de búsquedas. Aunque los algoritmos de búsqueda son más complejos que esto, se puede decir que estos 4 pasos son el core de cualquier algoritmo de búsqueda.

## El índice invertido

Antes de avanzar, vamos a intentar entender el índice invertido, la estructura de datos del corazón de cualquier algoritmo de búsqueda. 

El primer paso es conocer la existencia de los documentos para ser indexados y posteriormente buscados. El contenido de los documentos es parseado y dividido en palabras individuales normalizadas en minúsculas, para que todas las palabras puedan compararse en minúsculas, y se elimina cualquier signo de puntuación en las palabras.

Cada palabra tiene asignada una frecuencia. La frecuencia determina el número de veces que aparece la palabra en los documentos que tenemos indexados.

Junto con las palabras y su correspondiente frecuencia, el índice invertido contiene el documento donde aparece cada palabra. Cada palabra que contiene el índice invertido puede aparecer en uno o más documentos. Resumiendo, el índice invertido son todas las diferentes palabras junto con la frecuencia de aparición y los documentos en los que aparece.

A las palabras y su frecuencia correspondiente se le llama **diccionario**. 


## Instalación

Cuando arrancamos un ES lo que se crea es un clúster, para facilitar la escalabilidad.

## Tipos de nodo

En ES hay 4 tipos de nodos:

 - Nodos maestros se encargan de configurar y administrar el clustes de ES, como por ejemplo, para añadir nuevos nodos hay que hacerlo a través de los nodos maestros.
 - Nodos de datos se encargan de almacenar documentos. Este tipo de nodos tiene sentido hacer réplicas.
 - Nodos cliente sirven para comunicarnos con el nodo maestro o para recoger información.
 - Nodos de ingesta se encargan de preprocesar los documentos antes de almacenarlos. Son poco frecuentes. Por ejemplo, si la indexación de documentos es un cuello de botella porque en un corto periodo de tiempo se tienen que indexar miles de documentos, entonces se puede crear un nodo de ingesta para que procese esta información.
 

## Documentos

Es la unidad básica de información de ES.
