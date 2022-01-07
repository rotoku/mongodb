# mongodb

## Testando no Docker

### docker-compose.yml
```
version: '3'
services:
  mongo-express:
    image: mongo-express
    ports:
      - 8081:8081
    environment:
      ME_CONFIG_MONGODB_SERVER: mongodb
      ME_CONFIG_BASICAUTH_USERNAME: root
      ME_CONFIG_BASICAUTH_PASSWORD: password
      ME_CONFIG_MONGODB_ADMINUSERNAME: root
      ME_CONFIG_MONGODB_ADMINPASSWORD: password
    depends_on:
      - mongodb
    networks: 
      - mongo-compose-network

  mongodb:
    image: mongo
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: password
      #MONGO_INITDB_DATABASE: alunos
    ports:
      - 27017:27017
    volumes:
      - ${PWD}/data:/data/db
    networks: 
      - mongo-compose-network

networks: 
  mongo-compose-network:
    driver: bridge
```

### Criando
```sh
docker-compose up
```

### Destruindo
```sh
docker-compose down
```

## Comandos básicos
```sh

## mongodb
docker run -p 27017:27017 --name mongodb -d mongo

docker exec -it mongodb /bin/bash

mongo

show collections

db.createCollection("alunos");

db.alunos.insert(
{
	"nome": "Fulano de Tal",
	"data_nascimento": new Date(1994,01,29)
}
);

db.alunos.insert(
{
	"nome": "Fulano de Tal",
	"data_nascimento": new Date(1994,01,29),
	"curso": {
		"nome": "Sistemas de Informação"
	},
	"notas": [10.0, 9.0, 4.5],
	"habilidades": [
		{
			"nome": "inglês",
			"nível": "avançado"
		},
		{
			"nome": "judô",
			"nível": "intermediário"
		}
	]
}
);

db.alunos.find();

db.countLogPage.find(
	{
		"deviceSerial": "153307814",
		"friendlyDeviceSerial": "V4D-19090086",
		"countLogs": {
			"timestamp" : ISODate("2019-08-03T03:00:00.000+0000"),
			"logEntryId" : NumberLong(225484),
			"counts" : [
				{
					"registerId" : NumberLong(0),
					"value" : NumberLong(53),
					"name" : "Customers"
				},
				
				{
					"registerId" : NumberLong(1),
					"value" : NumberLong(0),
					"name" : "Staffs"
				},
				
				{
					"registerId" : NumberLong(2),
					"value" : NumberLong(58),
					"name" : "Deleted 3"
				}
			],
			"configIndex" : NumberLong(0)
		}
	}
);


db.inventory.insertMany( [
   { item: "journal", instock: [ { warehouse: "A", qty: 5 }, { warehouse: "C", qty: 15 } ] },
   { item: "notebook", instock: [ { warehouse: "C", qty: 5 } ] },
   { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 15 } ] },
   { item: "planner", instock: [ { warehouse: "A", qty: 40 }, { warehouse: "B", qty: 5 } ] },
   { item: "postcard", instock: [ { warehouse: "B", qty: 15 }, { warehouse: "C", qty: 35 } ] }
]);

db.inventory.find(
	{
		"instock": {
			"warehouse": "B", "qty": 15
		}
	}
);

use test;

db.alunos.insert(
    {
        "nome": "Isabela Olara Ramos",
        "data_nascimento": new Date(1995, 08, 15),
        "notas": [10, 9, 4.5, 8.5]
    }
);

db.alunos.find();

db.alunos.find(
    {
        "notas": {$gt: 5}
    }
);


db.alunos.insert(
    {
        "nome": "Selma Aparecida De Oliveira",
        "data_nascimento": new Date(1966,7,1),
        "curso": {
            "nome": "Letras"
        },
        "notas": [6,7,8,9]
    }
);

db.alunos.insert(
    {
        "nome": "Mauro Tomio Kumabe",
        "data_nascimento": new Date(1963,11,13),
        "curso": {
            "nome": "Engenharia Elétrica"
        },
        "notas": [9,10,8,9]
    }
);

db.alunos.insert(
    {
        "nome": "Sérgio Júnior",
        "data_nascimento": new Date(1966,7,1),
        "curso": {
            "nome": "Moda"
        },
        "notas": [1,2,3,4]
    }
);

db.alunos.findOne(
    {
        "notas": {$gt: 9}
    }
);


push -> adicionar um elemento no array
addToSet -> adicionar um elemento no array caso ele não exista
each

db.alunos.find({"curso.nome" : "Sistema de informação"})

db.alunos.find({"curso.nome" : "Sistemas de Informação"})

db.alunos.update(
    {"curso.nome" : "Sistema de informação"},
    {
        $set : {
            "curso.nome" : "Sistemas de Informação"
        }
    }    
)

db.alunos.update(
    {"curso.nome" : "Sistemas de informação"},
    {
        $set : 
           {"curso.nome" : "Sistemas de Informação"}  
        }, 
      {
        multi : true 
      }
)


db.alunos.find({"_id" : ObjectId("5d67fc4e632dc308b02c3c2f")}).pretty();

db.alunos.update(
	{"_id": ObjectId("5d67fc4e632dc308b02c3c2f")},
	{$set: { "notas" : [10, 9, 4.5, 8.5] }}
)


db.alunos.update(
    {"_id" : ObjectId("5d67fc4e632dc308b02c3c2f")},
    {
        $push : {
            notas : 8.5
        }    
    }
)

db.alunos.update(
    {"_id" : ObjectId("5d67fc4e632dc308b02c3c2f")},
    {
        $push : {
            "notas" : {$each : [7, 8] }
        }
    }
)


db.alunos.find({"_id" : ObjectId("5d67fc4e632dc308b02c3c2f")}).pretty();

db.alunos.update(
	{"_id": ObjectId("5d67fc4e632dc308b02c3c2f")},
	{$set:	{
				"nome": "Beltano da Silva",
				"data_nascimento": new Date(1995,08,15),
				"notas" : [10, 10, 10, 10]
			}}
)


db.alunos.update(
	{"curso.nome" : "Sistemas de Informação"},
	{$set:{"curso.nome" : "Gastronomia"}},
	{multi : true}
)

db.alunos.insert(
{
	"nome": "Beltano da Silva",
	"data_nascimento": new Date(1995,08,15),
	"curso": {
		"nome": "Medicina"
	},
	"notas": [10.0, 9.0, 8.5, 8],
	"habilidades": [
		{
			"nome": "inglês",
			"nível": "avançado"
		},
		{
			"nome": "ballet",
			"nível": "intermediário"
		},
		{
			"nome": "natação",
			"nível": "intermediário"
		}
	]
}
);

--Ordene os alunos em ordem alfabética crescente
db.alunos.find().sort({"nome": 1});

--Ordene os alunos em ordem alfabética decrescente
db.alunos.find().sort({"nome": -1});

-- limitar um resultado
db.alunos.find().sort({"nome": 1}).limit(1);


-- todos alunos com notas menores que 5
db.alunos.find({
	"notas": {$lt: 5}
}).pretty();

--Busque apenas um aluno do curso de Sistemas de informação
db.alunos.findOne({
	"curso.nome": "Sistemas de Informação"
})
```