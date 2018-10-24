# cikm-tutorial

## Executing a SPARQL query

DBPedia's SPARQL endpoint is located at https://dbpedia.org/sparql .

The following query finds all movies starring Brad Pitt

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dbpedia-owl: <http://dbpedia.org/ontology/>

SELECT ?movie ?title ?name
WHERE {
  ?movie dbpedia-owl:starring [ rdfs:label "Brad Pitt"@en ];
         rdfs:label ?title;
         dbpedia-owl:director [ rdfs:label ?name ].
  FILTER LANGMATCHES(LANG(?title), "EN")
  FILTER LANGMATCHES(LANG(?name),  "EN")
}
```

The result is a list of bindings for the requested variables.

## Setting up a Triple Pattern Fragments server
Pull from https://github.com/LinkedDataFragments/Server.js and run `npm install` (requires Node.js and npm). Windows users will probably get a bunch of errors from the HDT package but this is not an issue since it's optional.

First we create our dataset. We will create a turtle file with a new Brad Pitt movie:

```turtle
PREFIX ex: <http://example.com/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dbpedia-owl: <http://dbpedia.org/ontology/>

ex:brad rdfs:label "Brad Pitt"@en.
ex:director rdfs:label "The Director"@en.
ex:movie rdfs:starring ex:brad;
         rdfs:label "Our movie"@en;
         dbpedia-owl:director ex:director.
```

Copy `config/config-default.json` to `./config.json`.
We will base ourselves on that file for our config.
Remove the current datasources and add a reference to our newly created turtle file.
```json
"myData": {
  "title": "My awesome dataset",
  "type": "TurtleDatasource",
  "description": "An extensive knowledge base compressed to a single turtle file",
  "settings": { "file": "test.ttl" }
}
```

# Querying our data through the online client
Online client: http://query.linkeddatafragments.org/

Replace the datasources with our source ( http://localhost:5000/myData ) and execute the following query te see our data:
```sparql
SELECT *
WHERE {
  ?s ?p ?o
}
```
