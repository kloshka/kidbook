# SPARQL-запросы к WikiData

## 1) Базовые финансовые понятия для детей (пример поиска сущностей)
```sparql
SELECT ?item ?itemLabel ?itemDescription WHERE {
  VALUES ?topic {
    wd:Q98918538   # financial literacy
    wd:Q41253      # budget
    wd:Q989438     # income
    wd:Q179550     # expense
    wd:Q11229      # price
    wd:Q18602249   # discount
    wd:Q2690967    # bank card
    wd:Q81894      # cash
    wd:Q47589      # receipt
  }
  BIND(?topic AS ?item)
  SERVICE wikibase:label {
    bd:serviceParam wikibase:language "ru,en".
    ?item rdfs:label ?itemLabel.
    ?item schema:description ?itemDescription.
  }
}
```

## 2) Связанные понятия через свойства (часть подграфа)
```sparql
SELECT ?subject ?subjectLabel ?propLabel ?object ?objectLabel WHERE {
  VALUES ?subject { wd:Q41253 wd:Q179550 wd:Q11229 wd:Q47589 wd:Q81894 }
  ?subject ?p ?object .
  ?prop wikibase:directClaim ?p .

  FILTER(isIRI(?object))

  SERVICE wikibase:label {
    bd:serviceParam wikibase:language "ru,en".
    ?subject rdfs:label ?subjectLabel.
    ?prop rdfs:label ?propLabel.
    ?object rdfs:label ?objectLabel.
  }
}
LIMIT 200
```

## 3) Пример отбора образовательных программ/понятий про финансовую грамотность
```sparql
SELECT ?item ?itemLabel ?countryLabel WHERE {
  ?item wdt:P31/wdt:P279* wd:Q2385804 .   # educational program/initiative
  OPTIONAL { ?item wdt:P17 ?country . }
  SERVICE wikibase:label {
    bd:serviceParam wikibase:language "ru,en".
    ?item rdfs:label ?itemLabel.
    ?country rdfs:label ?countryLabel.
  }
}
LIMIT 150
```

Примечание: идентификаторы сущностей в WikiData могут отличаться по трактовке. Перед финальной выгрузкой полезно проверить карточки вручную и уточнить `wd:Q...`.
