# mongo-inventaire-ordi
Utilisation et manipulation d'inventaire d'ordianteurs utilisant MongoDB

## Importation
Importer le fichier ordinateur.json avec la commande mongoimport
```
mongoimport --db entrepot --collection ordinateurs --jsonArray --file "C:\Users\mtrem\Desktop\BDD MASSIVES\ordinateur.json
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/79412cf3-de22-4052-82a2-656fda726e79)

## Renomer
Renommez la collection "ordinateur" en y ajoutant votre prénom en tant que suffixe. 
```
db.ordinateurs.renameCollection("ordinateuralex")
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/9343c300-7a99-4eb3-967b-f38aa3a02311)

## Afficher
Afficher la liste des ordinateurs (marque, modele, processeur) (2 points)
```
db.ordinateuralex.find({}, {"marque":1, "modele":1, "processeur":1, "_id":0})
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/a5eb04da-c03e-4ae8-87bf-8a41f49749e3)

## Afficher avec condition
Afficher la liste des ordinateurs (marque, modele, processeur) avec la ram supérieure à 16 (2 points)
```
db.ordinateuralex.find({"ram":{$gt: 16}}, {"marque":1, "modele":1, "processeur":1, "_id":0})
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/decfe818-68fd-4101-9582-6c73d332325a)

Afficher la liste des ordinateurs avec le stockage supérieur à 512 et prix inférieur à 1999
```
db.ordinateuralex.find({"stockage":{$gt:512}, "prix":{$lt:1999}})
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/a570db69-2a94-4ed2-8c31-1619e1f04198)

Afficher la liste des ordinateurs avec os macOS Big Sur
```
db.ordinateuralex.aggregate([{$match:{"os":"macOS Big Sur"}}])
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/45e8368d-338e-449c-b33f-a9d717bba7c7)

Afficher la liste des ordinateurs avec la note d’avis égale à 4 (2 points)
```
db.ordinateuralex.find({
"avis.note": 4
})
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/7e31771e-884b-40c7-9b32-ca22e781aa68)


## Update
Rajouter plus 150$ au prix des ordinateurs
```
db.ordinateuralex.updateMany({},{$inc:{"prix": 150}})
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/8609cfb0-78c7-48d1-9739-5b0e3ad2ef76)

Rajouter le stockage 512 aux ordinateurs avec une espace de 512 go
```
db.ordinateuralex.updateMany(
{'stockage': 512},
{$inc:{stockage: 512}}
)
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/64054a88-76da-4cd6-8eac-c907e0e47a82)

## Calcule
Calculer le nombre des ordinateurs de la marque Apple
```
db.ordinateuralex.aggregate([{$match:{marque:"Apple"}},{$count: "Ordinateurs de la marque Apple"}])
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/04f8875c-61dc-4c8b-92fa-bc016cc3c765)

Calculer la somme des stockages pour les ordinateurs non de la marque Apple
```
db.ordinateuralex.aggregate([
{ $match: { marque: { $ne: "Apple" } } },
{ $group: { _id: 0, somme: { $sum: "$stockage" } } }
])
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/31f05f05-1dfc-40ef-92d8-55a06b0d0292)

Calculer la somme des prix des ordinateurs d’os Windows 10 Home
```
db.ordinateuralex.aggregate([
{ $match: { os: { $eq: "Windows 10 Home" } } },
{ $group: { _id: 0, somme: { $sum: "$prix" } } }
])
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/d7ccb8ce-9694-4a4a-93c2-d201535d82f6)

## Insert
Rajouter trois documents dans la collection « ordinateur » selon la structure du fichier json
```
db.ordinateuralex.insertMany([
{"marque" : "michaelsoft",
"modele" : "pisquasher",
"processeur" : "amd taskripper",
"ram" : NumberInt(32),
"stockage" : NumberInt(1024),
"ecran" : NumberInt(16),
"os" : "michaelsoft binbows",
"prix" : NumberInt(6969),
"avis" : [
{
"note" : NumberInt(5),
"commentaire" : "pouris"
},
{
"note" : NumberInt(4),
"commentaire" : "pouris++"
}
]},
{"marque" : "big pc limited",
"modele" : "system 69",
"processeur" : "leg",
"ram" : NumberInt(32),
"stockage" : NumberInt(1024),
"ecran" : NumberInt(16),
"os" : "linoox",
"prix" : NumberInt(8008),
"avis" : [
{
"note" : NumberInt(5),
"commentaire" : "Excellent"
},
{
"note" : NumberInt(4),
"commentaire" : "wow"
}
]},
{"marque" : "Divine Computing",
"modele" : "Greybox",
"processeur" : "potato",
"ram" : NumberInt(32),
"stockage" : NumberInt(1024),
"ecran" : NumberInt(16),
"os" : "templeOS",
"prix" : NumberInt(9999),
"avis" : [
{
"note" : NumberInt(5),
"commentaire" : "performance pouritte"
},
{
"note" : NumberInt(4),
"commentaire" : "prix est wow"
}
]}
])
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/97d13e77-1844-40b2-9fb1-018ba0bd4915)

## Delete
Supprimer les ordinateurs avec la note d’avis égale à 3 
```
db.ordinateuralex.deleteMany({"avis.note":3})
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/85fdc41a-7e6f-4703-a6b4-23179d0a008f)

Supprimer les ordinateurs des écrans  (13,3 et 13,4 et 15)
```
db.ordinateuralex.deleteMany({"ecran": {$in: [13.3, 13.4, 15]}})
```
![image](https://github.com/4ltrem/mongo-inventaire-ordi/assets/51710412/871a97c0-9f03-4d6c-92d6-80c13cca90f3)
