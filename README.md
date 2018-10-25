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

## Using the Web client
Web client at http://query.linkeddatafragments.org/ .

Can easily execute queries there.

Can change the source to DBpedia SPARQL to execute the Brad Pitt query over again.

## Setting up a Triple Pattern Fragments server
Pull from https://github.com/LinkedDataFragments/Server.js and run `npm install` (requires Node.js and npm). Windows users will probably get a bunch of errors from the HDT package but this is not an issue since it's optional.

First we create our dataset. We will create a turtle file with a new Brad Pitt movie:

```turtle
PREFIX ex: <http://example.com/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dbpedia-owl: <http://dbpedia.org/ontology/>

ex:brad rdfs:label "Brad Pitt"@en.
ex:director rdfs:label "The Director"@en.
ex:movie dbpedia-owl:starring ex:brad;
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

## Querying our data through the online client
Online client: http://query.linkeddatafragments.org/

Replace the datasources with our source ( http://localhost:5000/myData ) and execute the following query te see our data:
```sparql
SELECT *
WHERE {
  ?s ?p ?o
}
```

Execute the Brad Pitt query to see it works on this data.

Then add the DBPedia data source to have a federated result.

## Setting up your own online client
Pull from https://github.com/comunica/jQuery-Widget.js/ and `npm install` again.

Update settings.json with our new data
```json
"datasources": [
  {
    "name": "My Data",
    "url": "http://localhost:5000/myData"
  }, ...
```

Start the server with `npm run start` and (eventually) see it on http://localhost:8080 .

Now we can execute all the same queries on our own local web client, including the SPARQL endpoint.
