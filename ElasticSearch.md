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
documentos más relevantes que existan que cumplan con los criterios de búsqueda.
