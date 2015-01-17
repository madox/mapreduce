#noSQL - zadanie 3
![](https://blog.apigee.com/sites/blog/files/nosql-plm.png)

=====
### ANAGRAMY

Plik [word_list.txt](http://wbzyl.inf.ug.edu.pl/nosql/doc/data/word_list.txt) zaimportowałem do bazy MongoDB.

W celu importu skorzystałem z polecenia:
```sh
mongoimport -d slowa -c slowa --type csv --file mcalus/Pobrane/word_list.txt -f "words"
```
Czas importu ok. 1 min.

Zaimportowano:
```sh
> db.slowa.count()
8199
```
#Historyjka NoSQL
![](https://cloudcelebrity.files.wordpress.com/2013/09/sql-nosql.png)

--------------------------------
Anagramy uzyskałem z użyciem mapreduce:
```sh
db.slowa.mapReduce(
  function(){emit(this.slowo.split("").sort().join(""), 1);},
  function(key, values) {return Array.sum(values)},
  {
    query: {},
    out: "mapreduce"
  }
)
db.slowa.mapReduce(
function(){emit(this.slowo.split("").sort().join(""), 1);},,
function(key, values) {return Array.sum(values)}, 
{ out: "mapreduce"});


{
        "result" : "mapreduce",
        "timeMillis" : 433,
        "counts" : {
                "input" : 8199,
                "emit" : 8199,
                "reduce" : 914,
                "output" : 7011
        },
        "ok" : 1
}

```
W ten sposób uzyskałem pary posortowanych słów oraz ciągów znaków.

W kolecji 'anagramy' sa pary - posortowane litery - slowa zawierajace te litery odzielone przecinkami.

```sh
{ "_id" : "aceprs", "value" : 7 }
{ "_id" : "acerst", "value" : 7 }
{ "_id" : "adelst", "value" : 6 }
{ "_id" : "adeprs", "value" : 6 }
{ "_id" : "aderrw", "value" : 5 }
{ "_id" : "aelpst", "value" : 6 }
{ "_id" : "aeprss", "value" : 5 }
{ "_id" : "aerrst", "value" : 5 }
{ "_id" : "aerstt", "value" : 5 }
{ "_id" : "belstu", "value" : 5 }

```


