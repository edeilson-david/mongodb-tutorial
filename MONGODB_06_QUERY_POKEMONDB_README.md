# Sample Database

**Pre-requisites**: 
- The MongoDB container running.
- Files project

## Setup Environment

```shell
# Starting the MongoDB container.
docker start mongodb-shell

# Copy data file to container
docker cp ./data/pokemon.json mongodb-shell:/

# Verify if the file exists
docker exec -it mongodb-shell ls

# Import data
docker exec -it mongodb-shell mongoimport -d pokemondb -c pokemons --type json --jsonArray --file pokemon.json 

# Accessing MongoDB shell
docker exec -it mongodb-shell mongosh

# Listing databases 
show dbs

# Using previously created database 
use pokemondb
db.getCollectionNames()
```

## Simple queries
```shell
# First document
db.pokemons.findOne({ })

# Count
db.pokemons.count({ legendary: true })
db.pokemons.count({ 'legendary': false })
db.pokemons.countDocuments({ 'legendary': false })

# Filter by name field
db.pokemons.find({ name: 'Pikachu' })

# Using regex
db.pokemons.find({ name: /^(P)/ })
db.pokemons.find({ name: /^(Pi)/ })
db.pokemons.find({ name: /achu$/ })

# Only name that start with 'Pi' (case sensitive)
db.pokemons.find({ name: /^Pi/ }, { name: 1 })

# (ignore case sensitive)
db.pokemons.find({ name: /^PI/i }, { name: 1 })
```

# Comparison queries operator
```shell
# Find documents where attack greater than
db.pokemons.find({ attack: { $gte: 185 } })

# Other operator:
# gt: greater than
# gte: greater than or equal
# eq: equals
# ne: not equals
# lt: less than
# lte: less than or equal
# in: in an array
# nin: not in an array

db.pokemons.find({ attack: { $lte: 5 } })
db.pokemons.find({ types: { $in: ['Fire', 'Normal'] } }, { name: 1, types: 1 })
db.pokemons.find({ name: { $in: ['Pichu', 'Pikachu'] } }, { name: 1, types: 1 })

# Note: The operator 'in' work as 'or' sql operator

# Find all where (3 < defense AND defense <= 5)
db.pokemons.find({ defense: { $gt: 3, $lte: 10 } }, { name: 1, defense: 1 })
```

# Logical operators
```shell
# Find all where (3 < defense AND defense <= 5)
db.pokemons.find( {$or: [ {defense:{$gt: 3, $lte: 10}}, {defense: 100} ]}, {name: 1, defense: 1}).sort({ name:1, defense: 1})

# Scenario: 1
db.pokemons.find({
  $or: [
    { $and: [ { hp: { $gt: 80 } }, { defense: { $gt: 80 } } ] },
    { $and: [ { attack: { $gt: 80 } }, { speed: { $gt: 80 } } ] }
  ]
},
{
  name: 1, hp: 1, defense: 1, attack: 1, speed: 1
})

db.pokemons.find({
  $or: [
    { $and: [ { hp: { $gt: 80 } }, { defense: { $gt: 80 } } ] },
    { $and: [ { attack: { $gt: 80 } }, { speed: { $gt: 80 } } ] }
  ]
},
{
  name: 1, hp: 1, defense: 1, attack: 1, speed: 1
})
```

# Size and All operator
```shell
# Find documents where types field has 1 size (element)
db.pokemons.find({ types: { $size: 1 } }, { name: 1, types: 1})

# Find documents where types field has 3 size (element)
db.pokemons.find({ types: { $size: 2 } }, { name: 1, types: 1})

# Find documents where types is "Rock" AND "Flying"
db.pokemons.find({ types: { $all: [ "Rock", "Flying" ] } }, { name: 1, types: 1})
```

# Pagination
```shell
# Find documents where types is "Rock"
db.pokemons.find({ types: "Rock"}, { name: 1, types: 1})

# 1st page with 2 documents
db.pokemons.find({ types: "Rock"}, { name: 1, types: 1, attack: 1}).sort({ attack: -1 }).skip(0).limit(2)

# 2nd page with 2 documents
db.pokemons.find({ types: "Rock"}, { name: 1, types: 1, attack: 1}).sort({ attack: -1 }).skip(2).limit(2)

# 50th page with 2 documents
db.pokemons.find({ types: "Rock"}, { name: 1, types: 1, attack: 1}).sort({ attack: -1 }).skip(50).limit(2)
```

```shell
# Stopping the MongoDB image
docker stop mongodb-shell
```