# Sample Database

**Pre-requisites**: 
- The MongoDB container running.
- Pokemon database up.

## Setup Environment

```shell
# Starting the MongoDB container.
docker start mongodb-shell

# Accessing MongoDB shell
docker exec -it mongodb-shell mongosh

# Using previously created database 
use pokemondb
```

## Indexes
```shell
# Create Index
db.pokemons.createIndex({ name: 1 })
db.pokemons.getIndexes()

db.pokemons.find({ 'name': 'Umbreon' }).explain()
db.pokemons.find({ 'name': 'Umbreon' }).explain('executionStats')
```

## Compound Indexes
```shell
# Create Compound Index

# The index position is important
db.pokemons.createIndex({ name: 1, attack: 1 })

# Inverse
db.pokemons.createIndex({ attack: 1, name: 1 })
db.pokemons.getIndexes()

db.pokemons.find({ 'name': /^R/, 'attack': { $gte: 100 } }).explain('executionStats')
```

## Force the index to be used
```shell
db.pokemons.find({ 'name': /^R/, 'attack': { $gte: 100 } }).hint({ attack: 1, name: 1 }).explain('executionStats')
```

## Remove indexes
```shell
db.pokemons.dropIndex('attack_1_name_1')
db.pokemons.dropIndex('name_1_attack_1')
```

## Compound indexes
```shell
db.pokemons.createIndex({ attack: 1, defense: -1 }, { name: 'more_powerful_by_less_defender'})
db.pokemons.find({ attack: { $gte: 85 }, defense: { $lte: 50 } }).explain('executionStats')
```

## Visit the site to learn more about index types and properties
https://www.mongodb.com/docs/manual/indexes/

```shell
# Stopping the MongoDB image
docker stop mongodb-shell
```