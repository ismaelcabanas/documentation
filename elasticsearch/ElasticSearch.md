# Elastic Search

Elasticsearch es un motor de búsqueda open source altamente escalable. A pesar de que empezó como un buscador de texto 
ha evolucionado como una herramienta de análisis, que soporta no solo búsquedas simples sino también agregación compleja 
de información. Su naturaleza distribuida permite su escalabilidad a medida que la cantidad de información que 
contiene crece, sin perder rendimiento de forma significativa.

Elasticsearch está basado en Apache Lucene (al igual que Apache Solr) y está implementado en Java. Lucene provee busqueda 
e indexación, Elasticsearch amplía Lucene añadiendo una cómoda API REST basada en JSON para facilitar la comunicación 
con Lucene y además provee las características de sistema distribuido que permite escalar la herramienta de forma casi 
transparente al usuario.

## Empezando

### Cómo funciona una búsqueda

Cualquier aplicación que se precie necesita buscar de alguna forma la información que posee. **Buscar** es encontrar los 
documentos más relevantes que existan que cumplan con los criterios de búsqueda. Esto parece sencillo pero, en un motor de
búsqueda ocurren varias cosas por detrás para llevar a cabo esto.

1.- El motor de búsqueda debe tener conocimiento de la existencia de documentos, encontrarlo e indexarlo.

2.- La indexación de un documento es necesario para que se puedan hacer búsquedas en el futuro.

3.- Por cada palabra de búsqueda que realiza el usuario, tenemos que conocer cómo de relevante es el documento que la contiene. La relevancia es la combinación de los términos de búsqueda más el documento en sí.

4.- La búsqueda en sí misma.

Estos 4 componentes podrían resumir los componentes necesarios para realizar una búsqueda.

Hay varias tecnologías especializadas en cada uno de estos pasos. Por ejemplo, para el punto 1, un rastreador web rastrea todas las páginas con sus links para tener una visión de todos los documentos que existen. En el punto 2, se indexa cada documento encontrado por el rastreador, es decir, se parsean para que los términos individuales se extraigan y almacenen en una estructura de datos llamada el índice invertido. **El índice invertido es un mapeo de un término al documento donde el término es encontrado**. Es invertido porque va desde el término de búsqueda a la página web del documento. Basado en lo que el usuario buscaba, cada documento tendrá asociado una puntuación llamada puntuación relevante (punto 3). Los documentos tienen que estar puntuados antes que se devuelvan los resultados para que aquellos documentos con las puntuaciones más altas se muestren en los más alto del resultado de la búsqueda. La relevancia es calcula en función del término de búsqueda, así como el documento en sí mismo. Por último (punto 4) el proceso de la búsqueda en sí misma, se buscan los documentos en el índice invertido, encuentra los más relevantes y los devuelve en lo más alto del resultado de búsquedas. Aunque los algoritmos de búsqueda son más complejos que esto, se puede decir que estos 4 pasos son el core de cualquier algoritmo de búsqueda.

### El índice invertido

Antes de avanzar, vamos a intentar entender el índice invertido, la estructura de datos del corazón de cualquier algoritmo de búsqueda. 

El primer paso es conocer la existencia de los documentos para ser indexados y posteriormente buscados. El contenido de los documentos es parseado y dividido en palabras individuales normalizadas en minúsculas, para que todas las palabras puedan compararse en minúsculas, y se elimina cualquier signo de puntuación en las palabras.

Cada palabra tiene asignada una frecuencia. La frecuencia determina el número de veces que aparece la palabra en los documentos que tenemos indexados.

Junto con las palabras y su correspondiente frecuencia, el índice invertido contiene el documento donde aparece cada palabra. Cada palabra que contiene el índice invertido puede aparecer en uno o más documentos. Resumiendo, el índice invertido son todas las diferentes palabras junto con la frecuencia de aparición y los documentos en los que aparece.

A las palabras y su frecuencia correspondiente se le llama **diccionario**. Normalmente, el diccionario está ordenado para que la búsqueda de una determinado palabra sea muy efectiva. Cuando introducimos un término para realizar una búsqueda, éste se busca en el diccionario ordenado y los correspondientes documentos donde ha aparecido (llamados **postings**), que en terminología de índice invertido se llama la **posting list**.

Por ejemplo, dado que tenemos el siguiente ejemplo de índice invertido, si buscamos por el término *winter*, éste lo encontraríamos en el índice invertido, y veríamos que el origen dónde aparece el elemento es *Stark*. Y por lo tanto, el posicionamiento sería (posting list) el documento Stark. Este resultado es el que devolveríamos en la búsqueda. Mientras que, si por ejemplo, buscásemos el término *is*, como éste aparece en dos documentos, el resultado de la búsqueda sería dos regisros, *Stark* y *Baratheon*. 

Podemos hacer búsqueda más complejas combinando términos de búsqueda con AND o OR. O por ejemplo, búsquedas que terminen en **ong**, el índice invertido escanea todas las palabras que contiene y les da la vuelta mapeándola a los mismos documentos origen. Igualmente, para hacer búsquedas de substrings, el índice invertido utiliza otra técnica usando el índice invertido. O para hacer búsquedas geográficas, utiliza otro tipo de técnicas usando el índice invertido.

En definitiva, el índice invertido es el corazón de cualquier motor de búsqueda.

### Lucene: el motor de búsqueda

Apache Lucene es un motor de búsqueda Open Source. Lucene es una librería de indexación y búsqueda de alto rendimiento que ofrece además búsqueda full text. Lucene puede ser utilizado como motor de búsqueda para cualquier plataforma y aplicación. Proporciona capacidades de escrapeo y parseado de documentos. Lucene proporciona también algoritmos de búsqueda muy eficientes escritos originalmente en Java, pero puede ser portado a otros lenguajes.

Lucene es el núcleo de otras tecnologías que están construidas alrededor de Lucene, como por ejemplo Solr que añade indexación distribuída, balanceo de carga, replicación, recuperación automática, etc... Elasticsearch es otro ejemplo de tecnología que por debajo tiene a Lucene como motor de búsqueda permitiendo un motor de búsquedas distribuídas y un motor de análisis de datos.

### Elasticsearch

Elasticsearch es una tecnología Open Source. Es un motor de búsqueda y análisis escrito en Java y que usa Apache Lucene como la librería core para realizar las búsquedas. Es decir, no solo se usa para buscar sobre documentos sino también para analizar y hacer agregaciones complejas sobre los resultados de búsqueda.

Pero, cuales son los puntos fuertes de Elasticsearch?
1.- **Distribución**: permite escalar a miles de nodos en un sólo clúster lo que implica un alto rendimiento en las operaciones de búsqueda.
2.- **Alta disponibilidad**: es tolerante a fallos ya que tiene varias copias de la información en el clúster. Cada índice es replicado.
3.- **API REST**: existe un API REST sencillo para realizar operaciones CRUD sobre índices y documentos.
4.- **DSL potente**: posee un DSL para poder expresar consultas complejas con un lenguaje de dominio sencillo usando JSON.
5.- **Schemaless**: todos los documentos e índices que se almacenan en Elasticsearch no necesitan de un esquema ni tipado de datos.

Elasticsearch está desarrollado por la compañía [Elastic](http://elastic.co), desde el 2015. Elastic tiene además otros productos relacionados con Elasticsearch, como por ejemplo:
 - Kibana: es una herramienta de visualización para navegar por los documentos almacenados en Elasticsearch.
 - Beats: es un agente que envía datos desde otras máquinas para que lleguen a Elasticsearch.
 - Logstash: es un procesador de datos que puede analizar datos que vienen de varios orígenes.
 - X-Pack: es una herramienta de monitorización y reporte.
 - Cloud: que permite tener a Elasticsearch y Kibana en AWS.
 
### Instalación

Para instalar Elasticsearch descargamos del sitio oficial la versión que deseemos descargar. En este caso he bajado la versión 7.6.1.

Una vez descargado, descomprimimos el fichero y desde un nivel superior a la carpeta donde hemos descomprimido el fichero ejecutamos 

```./elasticsearch-7.6.1/bin/elasticsearch```

Con esto arrancamos un nodo de Elasticsearch corriendo en local en un clúster que por defecto se llama **elasticsearch**. Y podemos ver en las trazas que ha creado un nodo con un identificador autogenerado.

```[AFE0811358.home] initialized```

Podemos arrancar otros nodos al mismo clúster con la misma instrucción indicando el nombre del clúster.

Si queremos dar un nombre específico al clúster y al nodo, podemos hacerlo de la siguiente manera

```./elasticsearch-7.6.1/bin/elasticsearch -Ecluster.name=isma_es -Enode.name=my_first_node```

### Conceptos básicos de Elasticsearch

Elasticsearch permite hacer búsquedas prácticamente en tiempo real, con una latencia de aproximadamente 1 segundo entre el tiempo que un documento es indexado y el tiempo en el que el documento está disponible para ser buscado.

#### Nodos
Elasticsearch es distribuido por naturaleza. Se ejecuta en varias máquinas dentro un **clúster**. Una de esas máquinas dentro del clúster es un **nodo**.

Cada nodo dentro del clúster realiza las operaciones de indexación para poder indexar todos los documentos que se añaden a Elasticsearch. Todos los nodos también participan en las operaciones de búsqueda y análisis. Cualquier búsqueda se ejecutará en varios nodos en paralelo.

Cada nodo dentro del clúster tiene un identificador y nombre único. Estos datos son para referirnos a un nodo en las tareas de administración del clúster.

##### Tipos de nodo

En Elasticsearch hay 4 tipos de nodos:

 - Nodos maestros se encargan de configurar y administrar el clustes de ES, como por ejemplo, para añadir nuevos nodos hay que hacerlo a través de los nodos maestros.
 - Nodos de datos se encargan de almacenar documentos. Este tipo de nodos tiene sentido hacer réplicas.
 - Nodos cliente sirven para comunicarnos con el nodo maestro o para recoger información.
 - Nodos de ingesta se encargan de preprocesar los documentos antes de almacenarlos. Son poco frecuentes. Por ejemplo, si la indexación de documentos es un cuello de botella porque en un corto periodo de tiempo se tienen que indexar miles de documentos, entonces se puede crear un nodo de ingesta para que procese esta información.

#### Clúster
Un **cluster** es una colección de nodos que operan juntos para llevar a cabo un mismo objetivo. Un clúster de Elasticsearch puede escalar a cientos e incluso miles de nodos, o incluso un único nodo en el clúster. Cualquier índice que se cree para que los documentos sean susceptibles de ser buscados se guardan en el clúster. Todo clúster tiene un nombre único, que si no se especifica, por defecto es **elasticsearch**. 

La forma que tenemos de escalar es añadir nuevos nodos al clúster especificando el nombre de éste a la hora de levantarlo. Los nodos dentro del clúster se encontrán a sí mismos dentro de Elasticsearch enviándose mensajes. Las máquinas que representan los nodos dentro del clúster deben estar en la misma red.

#### Tipos de documentos (deprecado a partir de la versión 6)

Los **documentos** son la información que vamos a querer almacenar en Elasticsearch. Éstos pueden tener cualquier información dependiendo de nuestro modelo de negocio. Estos documentos los podemos indexar formando una agrupación lógica, por ejemplo, dentro de un mismo índice de productos, podríamos tener documentos de móviles, de portátiles, etc... y estos podríamos indexarlos de forma lógica como distintos tipos de documentos. 

Cada uno de estos tipos de documentos forman parte de un **índice**, que es dónde se va a buscar. Por ejemplo, en un Site de Blogs, los artículos serían un tipo de documento, los comentarios sobre los artículos podrían ser otro tipo de documento. No es condición necesaria para Elasticsearch que un índice esté formado por un único tipo de documento, un índice puede estar formado por documentos de distintos tipos.

#### Índice

El índice en Elasticsearch se refiere a un conjunto de documentos similares. No necesitan ser exactamente del mismo tipo. Por dar una analogía con el mundo de base de datos relaciones, un índice
sería como una base de datos que puede contener distingos tipos de documentos (tablas en el mundo base de datos). Por lo tanto, podríamos tener un índice de catálogo de productos, otro de catálogo de usuarios, etc..., luego, Elasticsearch nos permitirá realizar búsquedas incluso entre varios índices.

Un índice se identifica únicamente en un clúster por su nombre, y se pueden tener cualquier número de índices dentro de un clúster. Lo normal es que los índices estén formados por documentos del mismo tipo porque luego se querrán hacer búsquedas sobre ellos. Los documentos con los mismos campos suelen pertenecer a un tipo. 

Un **tipo de documento** es una colección de documentos con las mismas características dentro de un índice.

#### Documentos

Un **documento** es la unidad de información que necesita ser indexada. Solamente es un contenedor de texto que necesita ser buscado. Los documentos en Elasticsearch son expresados en JSON. Todos los documentos se encuentran en un índice. Un documento es asignado a un tipo y un índice. Con el tipo y el índice logramos identificar un conjunto de documentos. Un índice puede estar compuesto por una gran cantidad de documentos, y puede pasar que almacenar todos estos documentos ocupen una gran cantidad de espacio y no quepan físicamente en un disco de un solo nodo. 

#### Shards o fragmentos

Si el número de documentos a almacenar es muy grande o el tamaño de estos documentos es enorme entonces el índice puede llegar a ser demasiado grande para que quepa en un solo nodo. Esto hará que nuestras búsquedas sean demasiado lentas. Para solucionar este problema se aconseja que el índice se separe entre varias máquinas o nodos en el clúster. Este proceso se llama **fragmentación (sharding)** de datos. Por eso, la solución es separar el índice a través de varios nodos en el clúster, donde cada nodo contenga una fragmentación (shard) del índice, es decir, que cada nodo tendrá sólo un subconjunto de datos del índice. El índice completo será la combinación de poner todos los nodos juntos. Y el concepto de **shards** o fragmentos, es lo que nos permite realizar búsquedas en paralelo en diferentes nodos. Lo que se hace en definitiva es, cuando buscamos un término, buscar de forma paralela en subconjunto de datos que son más pequeños y por tanto más fácil de encontrar lo que buscamos.

#### Réplicas

Para tener alta disponibilidad de la información y tolerantes a fallos necesitamos replicar la información. Esto se hace configurando **réplicas** de nuestro índice. Cada **shard** tendrá una réplica. Así, si uno de nuestros nodos falla, su información estará replicada en un shard de otro nodo.

Así que, fragmentando nuestro índice y replicando cada shard es como conseguimos que las operaciones de búsqueda sean eficientes y rápidas, y que nuestro clúster sea tolerante a fallos y altamente disponible.

Con los shards conseguimos también escalar en volumen y rendimiento buscando entre todas las réplicas a la vez.

#### Conclusión

Un índice se puede separar en varias máquinas, formando así subconjuntos de índice, y a estos subconjuntos de índice se le llaman **shards**.

Un **shard** puede ser **replicado** cero o más veces. Podemos tener tantas réplicas como pensemos que vayamos a necesitar.

Por defecto, Elasticsearch tiene 5 shards y 1 réplica. Así que, cada shard tiene un backup.

Visión general de un clúster de Elasticsearch

![Elasticsearch clúster](images/cluster_es.png)

Visión general de cómo se gestionan los índices en los nodos

![Elasticsearch índice](images/indice.png)

### Monitorizando el clúster

A continuación una serie de endpoints para comprobar el estado general de nuestro Elasticsearch

#### Información genérica

Una vez arrancado el clúster de Elasticsearch, podemos consultar su estado haciendo una petición a 
```http://localhost:9200```

y nos saldrá una información parecida a esta:

```
{
  "name": "my_first_node",
  "cluster_name": "isma_es",
  "cluster_uuid": "5F0T_vqoRvWtyGztDMjtGA",
  "version": {
    "number": "7.6.1",
    "build_flavor": "default",
    "build_type": "tar",
    "build_hash": "aa751e09be0a5072e8570670309b1f12348f023b",
    "build_date": "2020-02-29T00:15:25.529771Z",
    "build_snapshot": false,
    "lucene_version": "8.4.0",
    "minimum_wire_compatibility_version": "6.8.0",
    "minimum_index_compatibility_version": "6.0.0-beta1"
  },
  "tagline": "You Know, for Search"
}
```

Nos presenta información básica como el nombre del nodo (name), el nombre del clúster (cluster_name), el identificador del clúster (cluster_uuid) y la versión de Elasticsearch (version.number).

#### Información del estado del clúster, shards y réplicas

```
http://localhost:9200/_cat/health?v&pretty
```

La respuesta de este endpoint nos da la información del número de nodos disponibles (node.total), el número de nodos de datos (node.data), el estado del clúster (status).

Si el **status** del clúster es **green** indica que todos los shards y réplicas del clúster están disponibles para hacerles peticiones. El clúster es totalmente funcional.

Si el **status** del clúster es **yellow** indica que el clúster sigue siendo capaz de procesar peticiones pero puede que ciertas réplicas no estén disponibles.

Si el **status** del clúster es **red** indica que algunos shards de algunos índices no están disponibles, ni tampoco sus réplicas. El clúster no es totalmente funcional y necesita ser arreglado para evitar que pierda información.

#### Información de nodos

```
http://localhost:9200/_cat/nodes?v&pretty
```

Este endpoint nos indica los nodos que tenemos disponibles dentro del clúster. 

## Usando el API de Elasticsearch

Elasticsearch tiene una potente RESTful API para hacer todo tipo de operaciones sobre Elasticsearch.

### Crear índices

Para crear un índice, por ejemplo *candidates*, para tener un catálogo de los candidatos de nuestros clientes

```
PUT http://localhost:9200/candidates?&pretty
```

El nombre **candidates** será el nombre del índice que queremos crear.

### Listar los índices

Para ver los índices de nuestro Elasticsearch

```
GET http://localhost:9200/_cat/indices?v&pretty
```

Un ejemplo de la respuesta sería

```
health status index      uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   candidates f2pcFbuuQe-6QFO60RcsSw   1   1          0            0       283b           283b
```

Cuando creamos un índice, Elasticsearch asigna automáticamente un UUID, indicado por el campo **uuid**, al índice. Por defecto, en la versión 7.6.1, cuando se crea un índice se asigna 1 shard, indicado por el campo **pri**, y 1 réplica, indicado por el campo **rep**. En versiones anteriores, por defecto, se asignaba a 5 shards y 1 réplica.

El número de documentos del índice viene indicado por el campo **docs.count**.

El tamaño que ocupa el índice viene indicado por el campo **store.size**.

Cuando se crea el índice, se puede ver que el campo **health** está a **yellow**, esto es porque es un clúster de un solo nodo, y al no haber otro nodo donde hacer la réplica del nodo, el nodo mismo guarda la réplica, pero sería un problema si este nodo lo perdiésemos por algún motivo

### Añadir documentos al índice

Para añadir un documento a un índice hacemos una petición PUT con un request body con el contenido del documento

```
PUT http://localhost:9200/cvs/candidates/c388034b-98e7-4e30-af59-be3c7fde36af?pretty

{
	"name": "Ismael",
	"surname": "Cabañas Garcia"
}
```

La respuesta que recibimos es algo como esto

```
{
  "_index": "cvs",
  "_type": "candidates",
  "_id": "c388034b-98e7-4e30-af59-be3c7fde36af",
  "_version": 1,
  "result": "created",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 0,
  "_primary_term": 1
}
```

La respuesta nos indica que se ha creado un documento en el índice **cvs** del tipo **candidates** con el identificador **c388034b-98e7-4e30-af59-be3c7fde36af**. El campo **version** indica el número de veces que el documento se ha actualizado (cuando es creado por primera vez el valor es 1). El campo **result** indica la operación que se ha llevado a cabo, en este caso **created**. 

### Recuperar documentos por id

```
GET http://localhost:9200/cvs/candidates/c388034b-98e7-4e30-af59-be3c7fde36af?pretty
``` 

La respuesta será parecida a esta

```
{
  "_index": "cvs",
  "_type": "candidates",
  "_id": "c388034b-98e7-4e30-af59-be3c7fde36af",
  "_version": 1,
  "_seq_no": 0,
  "_primary_term": 1,
  "found": true,
  "_source": {
    "name": "Ismael",
    "surname": "Cabañas Garcia"
  }
}
```

Lo destacable de esta respuesta es que el contenido del documento se encuentra en el campo **_source**. Si el contenido del documento es muy grande, éste campo será a su vez muy grande y afectará al rendimiento de la respuesta pues es información que se tiene que enviar al cliente.

El campo **found** nos indica si encontró el documento en el índice, en este caso es *true* pero si no lo encontrase devolvería *false* y el campo *_source* no vendría en la respuesta.

Si no estuviésemos interesados en el contenido del documento, podemos añadir el parámetro **_source=false** a la petición.

```
GET http://localhost:9200/cvs/candidates/c388034b-98e7-4e30-af59-be3c7fde36af?pretty&_source=false
``` 

Si sólo nos interesan ciertos campos del contenido, podemos añadir el parámetro **_source=field1, ..., fieldN**

```
GET http://localhost:9200/cvs/candidates/c388034b-98e7-4e30-af59-be3c7fde36af?pretty&_source=name
``` 

### Información del índice

Una vez tenemos documentos en nuestro índice, una información interesante es consultar el endpoint

```
http://localhost:9200/candidates
```

La respuesta nos da mucha meta-información del índice, como por ejemplo los tipos que ha asignado Elasticsearch a cada uno de los campos de los documentos.


### Actualización de documentos

Cuando queremos actualizar el contenido de un documento, éste necesita ser reindexado o actualizado. Se puede actualizar todo el contenido del documento o hacer una actualización parcial. 

El endpoint para realizar una **actualización total** de un documento es similar al endpoint para crear un documento, solo que cambiamos el contenido

```
PUT http://localhost:9200/cvs/candidates/c388034b-98e7-4e30-af59-be3c7fde36af?pretty

{
	"name": "Ismael",
	"surname": "Bernardino Garcia"
}
```

La respuesta también es similar a la respuesta obtenida cuando creabamos un documento

```
{
  "_index": "candidates",
  "_type": "cvs",
  "_id": "f26bea19-120c-4f50-be95-c6e3acd15cb7",
  "_version": 2,
  "result": "updated",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 4,
  "_primary_term": 1
}
```

Pero la diferencia viene en los campos **version** y **result**. El campo **version** vemos que aumenta en 1, indicando que hay una nueva versión del documento, el campo **result** indica que es una actualización.

Para realizar una **actualización parcial** de un documento debemos realizar una petición más concreta, debe ser con el método POST, el path **_update** y en el body de la petición un campo **doc**. Veamos un ejemplo en el que queremos añadir un nuevo campo a un documento, la petición para realizar esto sería algo como esto

```
POST http://localhost:9200/candidates/cvs/f26bea19-120c-4f50-be95-c6e3acd15cb7/_update?pretty

{
	"doc": {
		"age": 44 
	}
}
```

Y la respuesta obtenida

```
{
  "_index": "candidates",
  "_type": "cvs",
  "_id": "f26bea19-120c-4f50-be95-c6e3acd15cb7",
  "_version": 3,
  "result": "updated",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 5,
  "_primary_term": 1
}
```

Y vemos que se ha incrementado el campo **version** y el campo **result** indica que es una actualización. Y si consultamos el documento, veremos que aparece el nuevo campo **age** en el documento, pero los otros documentos del mismo tipo no tendrán tal campo.

También podemos usar este mismo endpoint para actualizar campos existentes

```
POST http://localhost:9200/candidates/cvs/f26bea19-120c-4f50-be95-c6e3acd15cb7/_update?pretty

{
	"doc": {
		"name": "Ismael"
	}
}
```

Si realizamos una actualización parcial sin se produzca ningún cambio en el documento, la respuesta que recibiremos será parecida a 

```
{
  "_index": "candidates",
  "_type": "cvs",
  "_id": "f26bea19-120c-4f50-be95-c6e3acd15cb7",
  "_version": 4,
  "result": "noop",
  "_shards": {
    "total": 0,
    "successful": 0,
    "failed": 0
  },
  "_seq_no": 6,
  "_primary_term": 1
}
```

El campo **result** indicará el valor **noop**, y el valor del campo **version** no cambiará.

El API Rest de Elasticsearch nos permite hacer actualizaciones parciales en base a un **script**

```
POST http://localhost:9200/candidates/cvs/f26bea19-120c-4f50-be95-c6e3acd15cb7/_update?pretty

{
	"script": "ctx._source.age += 2"
}
```

### Borrado de documentos

#### Borrado de un documento en el índice

Después de haber creado un documento en un índice, si quisiéramos eliminarlo deberíamos hacer una petición DELETE del documento

```
DELETE http://localhost:9200/candidates/cvs/724f918a-b1d3-4e5e-b024-f424a3cbee5d?pretty
```

La respuesta obtenida sería parecida a 

```
{
  "_index": "candidates",
  "_type": "cvs",
  "_id": "724f918a-b1d3-4e5e-b024-f424a3cbee5d",
  "_version": 2,
  "result": "deleted",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 9,
  "_primary_term": 1
}
```

Indicando en el campo **result** que la operación realizada fue borrado de un documento.

Si intentamos eliminar un documento que no existe dentro del índice, obtendríamos una respuesta parecida a 

```
{
  "_index": "candidates",
  "_type": "cvs",
  "_id": "724f918a-b1d3-4e5e-b024-f424a3cbee5d",
  "_version": 2,
  "result": "deleted",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 9,
  "_primary_term": 1
}
```

Indicando en el campo **result** el valor **not_found**.

#### Existencia de un documento

Una alternativa para comprobar si un documento existe o no dentro de un índice sería realizar una peteción HEAD del documento, por ejemplo

```
HEAD http://localhost:9200/candidates/cvs/724f918a-b1d3-4e5e-b024-f424a3cbee5d?pretty
```

Si el documento no existe el código de estado de la respuesta será un **404**, en cambio, si existe el código de estado de la respuesta será un **200**.

Hay que tener en cuenta que cuando borramos un documento de Elasticsearch éste **no se elimina del índice inmediatamente** sino que se marca como borrado lógico, y más tarde, cuando se producen los procesos de sincronización de Elasticsearch para consolidar espacio en disco, es cuando el borrado tiene lugar. 

#### Borrado de un índice

Podemos borrar todos los documentos de un índice borrando el índice completo.

Si intentamos eliminar un índice que no existe

```
DELETE http://localhost:9200/cvss?pretty
```

Obtendremos una respuesta de error indicando que el índice no se encontró: 

```
{
  "error": {
    "root_cause": [
      {
        "type": "index_not_found_exception",
        "reason": "no such index [cvss]",
        "resource.type": "index_or_alias",
        "resource.id": "cvss",
        "index_uuid": "_na_",
        "index": "cvss"
      }
    ],
    "type": "index_not_found_exception",
    "reason": "no such index [cvss]",
    "resource.type": "index_or_alias",
    "resource.id": "cvss",
    "index_uuid": "_na_",
    "index": "cvss"
  },
  "status": 404
}
```

En cambio, si borramos un índice existente recibiremos una respuesta

```
{
  "acknowledged": true
}
```

Y si listásemos los índices del clúster, veríamos que el eliminado ya no se encontraría entre ellos.

### Operaciones Bulk

Elasticsearch no nos permite hacer operaciónes bulk de edición de documentos, pero sí nos permite obtener documentos y ejecutar varias acciones en una petición.

#### _mget

Podemos obtener varios documentos con el path **_mget**, por ejemplo

```
GET http://localhost:9200/_mget?pretty

{
	"docs": [
		{
			"_index": "candidates",
			"_type": "cvs",
			"_id": "5592c48f-08c4-44eb-aac6-cbbeeedf97d9"
		},
		{
			"_index": "candidates",
			"_type": "cvs",
			"_id": "2ee7b73d-0605-4441-8ece-aae108ee2cc6"
		}
	]
}
```

En el cuerpo de la petición, podemos ver que indicamos el índice, el tipo de documento, y el identifcador del documento que queremos recuperar.

Y la respuesta que obtendríamos sería

```
{
  "docs": [
    {
      "_index": "candidates",
      "_type": "cvs",
      "_id": "5592c48f-08c4-44eb-aac6-cbbeeedf97d9",
      "_version": 2,
      "_seq_no": 3,
      "_primary_term": 1,
      "found": true,
      "_source": {
        "name": "Beatríz",
        "surname": "Bernardino Nuño"
      }
    },
    {
      "_index": "candidates",
      "_type": "cvs",
      "_id": "2ee7b73d-0605-4441-8ece-aae108ee2cc6",
      "_version": 1,
      "_seq_no": 0,
      "_primary_term": 1,
      "found": true,
      "_source": {
        "name": "Saúl",
        "surname": "Cabañas Bernardino"
      }
    }
  ]
}
```

Un campo **docs** con un array de los documentos encontrados.

La misma respuesta obtendríamos si en lugar de especificar el índice en el cuerpo de la petición lo hiciésemos en el path de la petición, por ejemplo

```
GET http://localhost:9200/candidates/_mget?pretty
```

La misma respuesta obtendríamos si en lugar de especificar el tipo de documento en el cuerpo de la petición lo hiciésemos en el path de la petición, por ejemplo

```
GET http://localhost:9200/candidates/cvs/_mget?pretty
```

#### _bulk

Podemos ejecutar varias acciones en un petición con el path **_bulk**. Mejor ver la documentación de Elasticsearch si necesitamos un caso de uso de este estilo. 

Básicamente, podemos hacer operaciones de crear, borrar, o actualizar documentos en una sola petición a Elasticsearch, y el API nos respondería individualmente por cada acción.

### Operaciones bulk sobre el índice

Podemos hacer operaciones bulk sobre un índice, como por ejemplo indexar documentos a partir de un fichero JSON. Éste JSON debe tener un formato específico, un ejemplo sería:

```
{"index": {"_index": "candidates", "_type": "cvs", "_id": "uuid"}}
{"name": "Ismael", "surname": "Cabañas García"}
...
{"index": {"_index": "candidates", "_type": "cvs", "_id": "uuid"}}
{"name": "Ismael", "surname": "Cabañas García"}
```

Mejor ver también la documentación oficial de Elasticsearch si tuviésemos que realizar un caso de uso similar.


