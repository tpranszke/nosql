#Zadanie - Aggregation Pipeline

Do zadania egzaminacyjnego wykorzystałem bazę reddit. Baza zawiera ponad 50 milionów rekordów.
Jako drivera do Mongo użyłem PyMongo. Zapytania wyglądają tak samo.

###Aby sprawdzić przykładowy rekord użyłem zapytania:
```js
db.RColl.findOne()
```

###Przykładowy rekord wygląda następująco: 

![](http://i.imgur.com/qkcJSaH.png)


###Zapytanie o ilość komentarzy mężczyzn: 
```js
db.RColl.aggregate([ {$match: { author_flair_text: "Male" } }, 
{ $group: { _id: null, count: {$sum: 1} } } ])
```

Wynik zapytania: 
```js
{ "_id" : null, "count" : 26193 }
```
Zapytanie w PyMongo:
```js
import pymongo
client = pymongo.MongoClient("localhost", 27017)
db = client.RColl
db.RColl.aggregate([ {$match: { author_flair_text: "Male" } }, 
{ $group: { _id: null, count: {$sum: 1} } } ])
```


###Zapytanie o ilość komentarzy kobiet:
```js
db.RColl.aggregate([ {$match: { author_flair_text: "Female" } }, 
{ $group: { _id: null, count: {$sum: 1} } } ])
```


Wynik zapytania:
```js
{ "_id" : null, "count" : 5025 }
```


###Zapytanie o ilość komentarzy osób, które nie podały płci:
```js
db.RColl.aggregate( [ {$match: {author_flair_text: null} }, 
{ $group: { _id: null, count: {$sum: 1}}}])
```

Wynik zapytania: 
```js
{ "_id" : null, "count" : 29062486 }
```

###Zapytanie o 3 najlepszych autorów postów:
```js
db.redditColl.aggregate([ {$group: { _id: "$author", score: { $sum: "$score"} } },
 { $sort: {score: -1}}, {$limit: 3}], {allowDiskUse: true})
```

Wynik zapytania:
```js
{ "_id" : "[deleted]", "score" : 7486663 }
{ "_id" : "AutoModerator", "score" : 185250 }
{ "_id" : "PainMatrix", "score" : 164494 }
```

###Zapytanie o 3 najgorzyszych autorów postów: 
```js
db.redditColl.aggregate([ { $group: { _id: "$author", score: {$sum: "$score" } } },
{ $sort: {score: 1} }, { $limit: 3} ], {allowDiskUse: true})
```

Wynik zapytania:
```js
{ "_id" : "wutshappening", "score" : -8066 }
{ "_id" : "dwimback", "score" : -5241 }
{ "_id" : "salmonhelmet", "score" : -3499 }
```

###Zapytanie o ilość postów, które nie byly edytowane.

```js
db.RCOLL.aggregate ( [ {$match: {edited: false} }, 
                       {$group: { _id: null, count: 
                       {$sum: 1}}} ]
                   )
```

Wynik zapytania o ilość postów, które nie byly edytowane.

```js
{ "_id" : null, "count" : 52233268 }
```

