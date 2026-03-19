# SPARQL-запросы к DBpedia

Endpoint: https://dbpedia.org/sparql

## 1) Понятия, связанные с personal finance
```sparql
PREFIX dct: <http://purl.org/dc/terms/>
PREFIX dbo: <http://dbpedia.org/ontology/>

SELECT ?resource ?label ?abstract WHERE {
  ?resource dct:subject <http://dbpedia.org/resource/Category:Personal_finance> ;
            rdfs:label ?label ;
            dbo:abstract ?abstract .
  FILTER (lang(?label) = "en")
  FILTER (lang(?abstract) = "en")
}
LIMIT 50
```

## 2) Иерархия категорий про финансовую грамотность
```sparql
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>

SELECT ?cat ?catLabel ?broader ?broaderLabel WHERE {
  ?cat skos:broader ?broader ;
       rdfs:label ?catLabel .
  ?broader rdfs:label ?broaderLabel .
  FILTER(CONTAINS(LCASE(STR(?catLabel)), "finance"))
  FILTER(lang(?catLabel) = "en" && lang(?broaderLabel) = "en")
}
LIMIT 100
```
