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
8
```
3 Retorne apenas um elemento o método prático possível 
```
db.pets.find({}).limit(1)

{ "_id" : ObjectId("5e66e78a1d3fd7c45d949d25"), "name" : "Mike", "species" : "Hamster" }
```
4 Identifique o ID para o Gato Kilha. 
```
db.pets.find({name:'Kilha'},{'_id':1})

{ "_id" : ObjectId("5e66e78a1d3fd7c45d949d27") }
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

{ "_id" : ObjectId("5e66e78a1d3fd7c45d949d25"), "name" : "Mike", "species" : "Hamster" }
{ "_id" : ObjectId("5e66e7ed0f2c544b83da3bbd"), "name" : "Frodo", "species" : "Hamster" }
```
7 Use o find para listar todos os pets com nome Mike 
```
db.pets.find({'name':'Mike'})

{ "_id" : ObjectId("5e66e78a1d3fd7c45d949d25"), "name" : "Mike", "species" : "Hamster" }
{ "_id" : ObjectId("5e66e78a1d3fd7c45d949d28"), "name" : "Mike", "species" : "Cachorro" }
```
8 Liste apenas o documento que é um Cachorro chamado Mike 
```
db.pets.find({'name':'Mike','species':'Cachorro'})

{ "_id" : ObjectId("5e66e78a1d3fd7c45d949d28"), "name" : "Mike", "species" : "Cachorro" }
```

## Exercício 2 – Mama mia!
Importe o arquivo dos italian-people.js do seguinte endereço: Downloads NoSQL FURB. Em seguida, importe o mesmo com o seguinte comando: 

Analise um pouco a estrutura dos dados e em seguida responda: 

1 Liste/Conte todas as pessoas que tem exatamente 99 anos. Você pode usar um count para indicar a quantidade. 
```
db.italians.find({'age':99}).count();

0
```

2 Identifique quantas pessoas são elegíveis atendimento prioritário (pessoas com mais de 65 anos) 
```
db.italians.find({'age':{$gt:65}}).count()
1768
```
3 Identifique todos os jovens (pessoas entre 12 a 18 anos). 
```
db.italians.find({$and:[{'age':{$gt:12}},{'age':{$lt:18}}]}).count()
597
```
4 Identifique quantas pessoas tem gatos, quantas tem cachorro e quantas não tem nenhum dos dois 
```
db.italians.find({$and:[{'age':{$gt:12}},{'age':{$lt:18}}]}).count()
597
```
5 Liste/Conte todas as pessoas acima de 60 anos que tenham gato 
```
db.italians.find({'cat':{$exists:true}}).count()
6012
db.italians.find({'dog':{$exists:true}}).count()
4007
db.italians.find({'dog':{$exists:false},'cat':{$exists:false}}).count()
2416
```
6 Liste/Conte todos os jovens com cachorro 
```
db.italians.find({'dog':{$exists:true},'age':{$lt:18}}).count()
874
```
7 Utilizando o $where, liste todas as pessoas que tem gato e cachorro 
```
db.italians.find(
 { 
    $where: function() {
        return (this.cat && this.dog)
    } 
} 
);
2435 (não listado para nao colar 2435 linhas aqui)
```
8 Liste todas as pessoas mais novas que seus respectivos gatos. 
```
db.italians.find({$expr: { $lt: [ "$age" , "$cat.age" ] }}).count()
643
```
9 Liste as pessoas que tem o mesmo nome que seu bichano (gatou ou cachorro) 
```
db.italians.find(
{ 
    $where: function() {
           return ( 
            (  this.cat &&this.cat.name === this.firstname ) || 
            (  this.dog &&this.dog.name === this.firstname ))
       } 
}
).count()
89
```
10 Projete apenas o nome e sobrenome das pessoas com tipo de sangue de fator RH negativo 
```
db.italians.find(
{ $or : [
        { 'bloodType' :  {$eq :'A-'}},
        { 'bloodType' :  {$eq :'B-'}},
        { 'bloodType' :  {$eq :'AB-'}},
        { 'bloodType' :  {$eq :'O-'}},
    ]
}
).count()

5040
```
11 Projete apenas os animais dos italianos. Devem ser listados os animais com nome e idade. Não mostre o identificado do mongo (ObjectId) 
```
 db.italians.find({},{'cat.name':1,'cat.age':1,'dog.name':1,'dog.name':1})
```
12 Quais são as 5 pessoas mais velhas com sobrenome Rossi? 
```
 db.italians.find({'surname':'Rossi'}).sort({'age':-1}).limit(5)

{ "_id" : ObjectId("5e6815c8d507ae55614f2d87"), "firstname" : "Fabio", "surname" : "Rossi", "username" : "user9283", "age" : 79, "email" : "Fabio.Rossi@uol.com.br", "bloodType" : "AB+", "id_num" : "405401884484", "registerDate" : ISODate("2017-09-17T09:21:07.109Z")
, "ticketNumber" : 9371, "jobs" : [ "Aquicultura" ], "favFruits" : [ "Banana", "Banana", "Mamão" ], "movies" : [ { "title" : "Pulp Fiction: Tempo de Violência (1994)", "rating" : 4.48 }, { "title" : "Vingadores: Ultimato (2019)", "rating" : 2.85 }, { "title" : "A F
elicidade Não se Compra (1946)", "rating" : 4.92 }, { "title" : "A Viagem de Chihiro (2001)", "rating" : 2.28 } ], "cat" : { "name" : "Giovanna", "age" : 4 } }
{ "_id" : ObjectId("5e6815b1d507ae55614f0b64"), "firstname" : "Daniele", "surname" : "Rossi", "username" : "user544", "age" : 77, "email" : "Daniele.Rossi@outlook.com", "bloodType" : "A-", "id_num" : "043313531080", "registerDate" : ISODate("2012-02-27T20:51:59.050
Z"), "ticketNumber" : 8016, "jobs" : [ "Engenharia Biomédica" ], "favFruits" : [ "Melancia" ], "movies" : [ { "title" : "Harakiri (1962)", "rating" : 0.62 }, { "title" : "Interestelar (2014)", "rating" : 4.8 }, { "title" : "À Espera de um Milagre (1999)", "rating"
: 4.68 } ] }
{ "_id" : ObjectId("5e6815b2d507ae55614f0c33"), "firstname" : "Andrea", "surname" : "Rossi", "username" : "user751", "age" : 77, "email" : "Andrea.Rossi@uol.com.br", "bloodType" : "O-", "id_num" : "430441555454", "registerDate" : ISODate("2012-07-21T02:15:16.645Z")
, "ticketNumber" : 8714, "jobs" : [ "Produção Cultural", "Gastronomia" ], "favFruits" : [ "Banana", "Laranja", "Banana" ], "movies" : [ { "title" : "A Lista de Schindler (1993)", "rating" : 1.48 }, { "title" : "Os Sete Samurais (1954)", "rating" : 0.9 }, { "title"
: "Um Estranho no Ninho (1975)", "rating" : 4.06 }, { "title" : "Star Wars, Episódio V: O Império Contra-Ataca (1980)", "rating" : 2.55 }, { "title" : "Batman: O Cavaleiro das Trevas (2008)", "rating" : 4.86 } ], "mother" : { "firstname" : "Sara", "surname" : "Ross
i", "age" : 108 }, "father" : { "firstname" : "Paola", "surname" : "Rossi", "age" : 107 }, "cat" : { "name" : "Angela", "age" : 1 }, "dog" : { "name" : "Cristian", "age" : 16 } }
{ "_id" : ObjectId("5e6815c4d507ae55614f28a3"), "firstname" : "Pasquale", "surname" : "Rossi", "username" : "user8031", "age" : 77, "email" : "Pasquale.Rossi@hotmail.com", "bloodType" : "O-", "id_num" : "260866780146", "registerDate" : ISODate("2014-09-12T01:26:27.
050Z"), "ticketNumber" : 6609, "jobs" : [ "Arquivologia" ], "favFruits" : [ "Tangerina" ], "movies" : [ { "title" : "A Origem (2010)", "rating" : 2.53 }, { "title" : "Seven: Os Sete Crimes Capitais (1995)", "rating" : 2.98 } ] }
{ "_id" : ObjectId("5e6815b4d507ae55614f0fa3"), "firstname" : "Mattia", "surname" : "Rossi", "username" : "user1631", "age" : 75, "email" : "Mattia.Rossi@uol.com.br", "bloodType" : "B-", "id_num" : "764460551417", "registerDate" : ISODate("2012-09-14T21:41:04.105Z"
), "ticketNumber" : 3559, "jobs" : [ "Fisioterapia" ], "favFruits" : [ "Mamão", "Melancia" ], "movies" : [ { "title" : "O Silêncio dos Inocentes (1991)", "rating" : 0.81 }, { "title" : "Cidade de Deus (2002)", "rating" : 0.18 }, { "title" : "Clube da Luta (1999)",
"rating" : 0.02 } ] }

```
13 Crie um italiano que tenha um leão como animal de estimação. Associe um nome e idade ao bichano 
```
db.italians.insert([
{
    "firstname" : "Recigio", 
    "surname" : "Poffo", 
    "username" : "user751", 
    "age" : 37, 
    "email" : "Recogio@uol.com.br", 
    "bloodType" : "O-",
    "id_num" : "430441555454", 
    "registerDate" : ISODate("2012-07-21T02:15:16.645Z"), 
    "ticketNumber" : 8714, 
    "jobs" : [ "Produção Cultural", "Gastronomia" ], 
    "favFruits" : [ "Banana", "Laranja", "Banana" ], 
    "movies" : [ { "title" : "A Lista de Schindler (1993)", "rating" : 1.48 }],
    "father" : { "firstname" : "Paola", "surname" : "Rossi", "age" : 107 }, 
    "lion" : { "name" : "Mozart", "age" : 12 }, 
}
])
```
14 Infelizmente o Leão comeu o italiano. Remova essa pessoa usando o Id. 
```
db.italians.find({'surname':'Poffo'})

{ "_id" : ObjectId("5e6826ca41c2a9b192ad5ac3"), "firstname" : "Recigio", "surname" : "Poffo", "username" : "user751", "age" : 37, "email" : "Recogio@uol.com.br", "bloodType" : "O-", "id_num" : "430441555454", "registerDate" : ISODate("2012-07-21T02:15:16.645Z"), "t
icketNumber" : 8714, "jobs" : [ "Produção Cultural", "Gastronomia" ], "favFruits" : [ "Banana", "Laranja", "Banana" ], "movies" : [ { "title" : "A Lista de Schindler (1993)", "rating" : 1.48 } ], "father" : { "firstname" : "Paola", "surname" : "Rossi", "age" : 107
}, "lion" : { "name" : "Mozart", "age" : 12 } }

db.italians.remove({'_id':ObjectId("5e6826ca41c2a9b192ad5ac3")})

```
15 Passou um ano. Atualize a idade de todos os italianos e dos bichanos em 1. 
```
db.italians.update({},{ $inc: { 'age':1,  'cat.age':1 ,'dog.age':1 } }, { multi:true })

WriteResult({ "nMatched" : 10000, "nUpserted" : 0, "nModified" : 10000 })

```
16 O Corona Vírus chegou na Itália e misteriosamente atingiu pessoas somente com gatos e de 66 anos. Remova esses italianos. 
```
 db.italians.remove({'age':66, 'cat':{$exists:true}})
WriteResult({ "nRemoved" : 104 })
```
17 Utilizando o framework agregate, liste apenas as pessoas com nomes iguais a sua respectiva mãe e que tenha gato ou cachorro. 
```
db.italians.aggregate([
   {
      $match : { 
         $or: [
            {"cat":{"$exists":true}},
            {"dog":{"$exists":true}}
            ]
      }
    },
    {"$addFields": {
        "nameEqMotherName": {"$eq":["$firstname","$mother.name"]}
      }
    }
])

```
18 Utilizando aggregate framework, faça uma lista de nomes única de nomes. Faça isso usando apenas o primeiro nome 
```
db.italians.distinct( "firstname" );

//nao adicionado para nao ficar grande
```
19 Agora faça a mesma lista do item acima, considerando nome completo. 
```
db.italians.aggregate( 
            [
                {"$group": { "nome_completo": { firstname: "$firstname", surname: "$surname" } } }
            ]
        );
//nao adicionado para nao ficar grande
```
20 Procure pessoas que gosta de Banana ou Maçã, tenham cachorro ou gato, mais de 20 e  menos de 60 anos
```
db.italians.find(
    {   
         $and: [  
             { 
                $or: [
                         { 
                            $or: [ {favFruits: {$in:["Maçã"]}},{favFruits: {$in:["Banana"]}}]
                         },
                         { 
                            $or: [ {cat: {$exists:true}},{dog: {$exists:true}}]
                         }
                 ],
             },
            {
                'age' : { $gt:20 }
            },
            {
                'age' : { $lt:60 }
            }
        ]    
    }
)

4144
```

## Exercício 3 – Stockbrokers 
Importe o arquivo stocks.json do repositório Downloads NoSQL FURB. 
Esses dados são dados reais da bolsa americana de 2015. 

A importação do arquivo JSON é um pouco diferente da execução de um script:
```
mongoimport --db stocks --collection stocks --file stocks.json 
```

Analise um pouco a estrutura dos dados novamente e em seguida, responda as seguintes perguntas: 

1 Liste as ações com profit acima de 0.5 (limite a 10 o resultado)
```
show collections

db.stocks.find({})

db.stocks.find({'Profit Margin':{$gt:0.5}}).limit(10)

```
2 Liste as ações com perdas (limite a 10 novamente) 
```
db.stocks.find({'Profit Margin':{$lt:0}}).limit(10)
```
3 Liste as 10 ações mais rentáveis  
```
db.stocks.find({}).sort({'Profit Margin':-1}).limit(10)
```
4 Qual foi o setor mais rentável? 
```
db.stocks.aggregate(
   [
       
     {
       $group:
         {
           _id: { sector: "$Sector" },
           totalAmount: { $sum: "$Profit Margin" }
         }
     },
     {
        $sort:{ totalAmount:-1 }
      },
   ]
)

{ "_id" : { "sector" : "Financial" }, "totalAmount" : 162.5356 }
```
5 Ordene as ações pelo profit e usando um cursor, liste as ações. 
```
const cursor = db.stocks.find({}).sort({'Profit Margin':-1});

while (cursor.hasNext()) {
    const atual = cursor.next();
   console.log(atual);
}

```
6 Renomeie o campo “Profit Margin” para apenas “profit”. 
```
db.stocks.updateMany( {}, { $rename: { "Profit Margin": "profit" } } )

```
7 Agora liste apenas a empresa e seu respectivo resultado 
```
db.stocks.find({},{'Company':1,'profit':1})

```
8 Analise as ações. É uma bola de cristal na sua mão... Quais as três ações você investiria? 
```
Escolheria as que nos ultimos 50 dias tiveram o melhor moving average. Um periodo nem tão curto, nem tão longo.

db.stocks.find({}).sort({'50-Day Simple Moving Average':-1}).limit(3)

```
9 Liste as ações agrupadas por setor
```
db.stocks.aggregate([
   { $group : { _id : "$Sector", empresas: { $push: "$Company" } } }
 ])
```

## Exercício 3 (seria 4?) – Fraude na Enron! 

Um dos casos mais emblemáticos de fraude no mundo é o caso da Enron. A comunicade do MongoDB utiliza muito esse dataset pois o mesmo se tornou público, então vamos importar esse material também: 
 
(OBS: alterada collection para manter a stocks no banco)
```
mongoimport --db stocks --collection eron --file enron.json 
```

1. Liste as pessoas que enviaram e-mails (de forma distinta, ou seja, sem repetir). Quantas pessoas são? 
```
db.eron.distinct("sender")
db.eron.distinct("sender").length
2200
```
2. Contabilize quantos e-mails tem a palavra “fraud” 
```
db.eron.find({ text: { $regex: /fraud/i}}).count()
25
```