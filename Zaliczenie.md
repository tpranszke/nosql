# Zadanie - EDA

## Import bazy danych
Do tego zadania wykorzystałem bazę komentarzy z serwisu Reddit. Baza spakowana miała rozmiar 5.07 GB, a po rozpakowaniu rozmiar wynosił 29.4 GB.

### Import do MongoDB

#### Fragmet kodu importujący bazę:

```js
mongoimport --db redd --collection RColl  <  D:\RC_2015-01.json
```

Zużycie zasobów komputera podczas importu bazy:

![](http://i.imgur.com/E5yDLbF.png)

![](http://i.imgur.com/Goct2EJ.png)

Import trwał 1 godzinę i 2 minuty.

#### Zliczenie rekordów w bazie

```js
db.RColl.count()
```

![](http://i.imgur.com/CzFdydA.png)

Zostały zaimportowane 53 851 542 rekordy.

### Import do bazy PostreSQL

#### Fragment kodu importujący bazę:

```js
pgfutter_windows_amd64.exe --pw "lordryba1" json "D:\RC_2015-01.json"
```

Zużycie zasobów komputera podczas importu bazy:

![](http://i.imgur.com/nA7Ak2N.png)

![](http://i.imgur.com/rPgfuCw.png)

#### Zliczenie wierszy w bazie

```js
select count(*) from import.rc_2015_01;
```

Import do bazy danych PostgreSQL trwał 77 minut i 43 sekundy.

Zaimportowano 53 851 542 rekordy, co zostało obliczone pecjalną funkcją.

Zliczanie wierszy trwało 15 minut i 24 sekund.

# Zadanie - GeoJson

Do utworzenia mapy GeoJSON wykorzystałem [geojson.io](http://geojson.io/). Wykonałem mapkę z zaznaczynymi hotelami 5-gwiazdkowymi na terenie Polski.

*Mapa hoteli*

![](http://i.imgur.com/cvBkAeo.png)

Bazę danych zaimportowałem do MongoDB poleceniem:
```js
    mongoimport -c hotel2 < D:\hotel.json
```

Przykładowy rekord sprawdziłem poleceniem:

```js
db.hotel2.findOne()
```

Rekord: 
```javascript
{
  "_id" : ObjectId("2476c210d1a0f10511c704dc"),
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "name": "Hotel SPA Dr Irena Eris",
      "properties": {},
      "geometry": {
        "type": "Point",
        "coordinates": [
          16.4990633,
          50.4136944
        ]
      }
    }
```

### Point

Załóżmy, że do Poznania przylatuje osoba na wakacje i chce wynająć pokój w 5-gwiazdkowym hotelu. Przy użyciu polecenia: 
```js
var Poznan = {
               "type": "Point",
               "coordinates":[ 16.8264271,52.4199541 ]
               }

db.hotel2.find ( {loc : { $geoWithin : { $centerSphere : [Poznan, 50 / 3963.2 ] } } } )
```
możemy sprawdzić, który 5-gwiazdkowy hotel jest najbliżej Portu lotniczego Poznań-Ławica w promieniu 50 mil.

### LineString

Nasza osoba na wakacjach, chciałaby zwiedzić Polske od północy do południa. Zapytanie dotyczy czy na danej trasie od Sopotu do Zakopanego znajdują się 5-gwiazdkowe hotele.

![](http://i.imgur.com/D1XBflU.png)

Po wykonaniu zapytania:
```js
db.hotel2.find({loc: {$geoIntersects: {$geometry: {type: "LineString", 
coordinates: [18.5669743,54.4466851], [18.2046578,53.1257358], ... )
```
Nie wystąpił żaden błąd. Nie zostały również wyświetlone żadne rekordy.


