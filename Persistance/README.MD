# Exercices sur la persistance à distance avec MongoDB

Ce dossier contient des exercices sur les notions de persistance à distance et la base de données orientée documents `MongoDB`.

Chaque dossier contient un fichier `README` avec l'objectif de l'exercice ainsi qu'une solution possible.

Vous êtes fortement encouragés d'essayer de compléter l'exercice par vous-mêmes avant de regarder la solution, mais vous pouvez à tout temps consulter la solution finale pour mieux comprendre le fonctionnement des différentes notions.

La documentation du [pilote de NodeJS](https://www.mongodb.com/docs/drivers/node/current/) et [MongoDB](https://www.mongodb.com/docs/manual/reference/method/) vous seront également utiles pour les exercices. Notez que la documentation de MongoDB est celle des méthodes natives, mais une grande partie des exemples sont en JavaScript.

## Table des matières

Voici les liens vers les différents exercices et leurs énoncés. Vous êtes conseillés de comencer par l'exercice d'introduction :

- [`Class`](./Class/README.MD) : exercice d'évolution d'un système existant pour migrer vers une persistance à distance.

# NO SQL

## BD Cle Valeur
Every item stands on it's own, this means there are key-value stores: ***Key : Value***
You can split the workloads across servers and each server will be responsible for a part for ur database ***PARTITIONS***

If your database is split across partitions to find it NoSQL uses hashfunctions that will turn ur key into a hash number with a hashfunction and this will let you know where to put an item or to locate an item, if you have 2 servers then your range (keyspace) is split in half. Here you have doubled your storage capacity.

NoSQL is also Squemaless (each value can be different)

## BD Orientee Column
If you have 10 columns and lets say you want the name and tier of that column, reading it traditionally is not efficient, so we search by column name and column tier to extract our data
1) Read dat from top to bottom 
2) Only read he column we tell him to

## BD Orientee document
Having many attribuse for different Elements is when you should use this, example
[
    {"id" : 1, "name": "Iphone 15", "os": "iOS"},
    {"id" : 2, "name": "Sony a7R", "optics": "Mirrorless"},
]

It is in JSON documents.
The database is: Database> Collections> Documents

## BD Oriente Graphes

plein de graphes connecter (une chose les leis en commun?)