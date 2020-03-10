# Atividade Prática – MongoDB
Atividades relacionadas Mongod da pós de DataScience da Furb.


## Exercício 1- Aquecendo com os pets 
Insira os seguintes registros no MongoDB e em seguida responda as questões abaixo: 

```
use petshop 
db.pets.insert({name: "Mike", species: "Hamster"}) 
db.pets.insert({name: "Dolly", species: "Peixe"}) 
db.pets.insert({name: "Kilha", species: "Gato"}) 
db.pets.insert({name: "Mike", species: "Cachorro"}) 
db.pets.insert({name: "Sally", species: "Cachorro"}) 
db.pets.insert({name: "Chuck", species: "Gato"}) 
```

1 Adicione outro Peixe e um Hamster com nome Frodo 
```
db.pets.insert({name: "Nemo", species: "Peixe"}) 
db.pets.insert({name: "Frodo", species: "Hamster"}) 
```
2 Faça uma contagem dos pets na coleção 
```
db.pets.find({}).count()
```
3 Retorne apenas um elemento o método prático possível 
```
db.pets.find({}).limit(1)
```
4 Identifique o ID para o Gato Kilha. 
```
db.pets.find({name:'Kilha'},{'_id':1})
```
5 Faça uma busca pelo ID e traga o Hamster Mike 
```
db.pets.find({})

{ "_id" : ObjectId("5e66e78a1d3fd7c45d949d25"), "name" : "Mike", "species" : "Hamster" }
{ "_id" : ObjectId("5e66e78a1d3fd7c45d949d26"), "name" : "Dolly", "species" : "Peixe" }
{ "_id" : ObjectId("5e66e78a1d3fd7c45d949d27"), "name" : "Kilha", "species" : "Gato" }
{ "_id" : ObjectId("5e66e78a1d3fd7c45d949d28"), "name" : "Mike", "species" : "Cachorro" }
{ "_id" : ObjectId("5e66e78a1d3fd7c45d949d29"), "name" : "Sally", "species" : "Cachorro" }
{ "_id" : ObjectId("5e66e78b1d3fd7c45d949d2a"), "name" : "Chuck", "species" : "Gato" }
{ "_id" : ObjectId("5e66e7e80f2c544b83da3bbc"), "name" : "Nemo", "species" : "Peixe" }
{ "_id" : ObjectId("5e66e7ed0f2c544b83da3bbd"), "name" : "Frodo", "species" : "Hamster" }

db.pets.find({'_id':ObjectId('5e66e78a1d3fd7c45d949d25')})
```

6 Use o find para trazer todos os Hamsters 
```
db.pets.find({'species':'Hamster'})
```
7 Use o find para listar todos os pets com nome Mike 
```
db.pets.find({'name':'Mike'})
```
8 Liste apenas o documento que é um Cachorro chamado Mike 
```
db.pets.find({'name':'Mike','species':'Cachorro'})
```