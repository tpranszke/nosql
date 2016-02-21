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


