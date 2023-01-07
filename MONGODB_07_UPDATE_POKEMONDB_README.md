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

## Add and Remove fields
```shell
# Add a field with 'set' operator 
db.pokemons.updateOne({ '_id': 561 }, { $set: { gender: ['Male', 'Female'] } })
db.pokemons.find({ gender: 'Male' })

# Find and Count documents that gender field exists
db.pokemons.find({ gender: { $exists: true } }).count()

# Find and Count documents that gender field not exists
db.pokemons.find({ gender: { $exists: false } }).count()

# Update another document adding two fields
db.pokemons.updateOne({ '_id': 562 }, { $set: { gender: ['Male', 'Female'], height: 0.5, weight: 1.5 } })
db.pokemons.find({ gender: 'Male' })

# Remove fields
db.pokemons.updateOne({ '_id': 562 }, { $unset: { gender: [], height: 0.5, weight: 'Value is not unnecessary' } })
db.pokemons.find({ _id: 562 })
```

## Update field value
```shell
# Increment and Multiply field value
db.pokemons.find({ _id: 247 }, { hp: 1, defense: 1 })
db.pokemons.updateOne({ _id: 247 }, { $inc: { hp: 1 }, $mul: { defense: 2 } })
db.pokemons.find({ _id: 247 }, { hp: 1, defense: 1 })
```

## Limit (min and max) field value
```shell
# Find some documents
db.pokemons.find({ _id: { $in: [164, 233] } }, { name: 1, attack: 1})

# Nothing to update. **Only update field if the value specified is LESS THAN current value
db.pokemons.updateMany({ _id: { $in: [164, 233] } }, { $min: { attack: 190 } })

# Update only _id: 164.
db.pokemons.updateMany({ _id: { $in: [164, 233] } }, { $min: { attack: 187 } })
db.pokemons.find({ _id: { $in: [164, 233] } }, { name: 1, attack: 1})

# **Only update field if the value specified is GREATER THAN current value
db.pokemons.updateMany({ _id: { $in: [164, 233] } }, { $max: { attack: 200 } })
db.pokemons.find({ _id: { $in: [164, 233] } }, { name: 1, attack: 1})
```

## Upsert statement
```shell
# Find document existed
db.pokemons.find({ name: "Charmander"}, { name: 1, attack: 1})

# The document will be updated
db.pokemons.updateOne(
  { name: "Charmander" },
  { $set: { attack: 150} },
  { upsert: true})

# Upsert a document
db.pokemons.updateOne(
  { name: "Charmandlermoon" },
  { $set: { _id: 5000, attack: 200} },
  { upsert: true})

# Find documents
db.pokemons.find({ name: /^Charman/})

# Note: The document that was inserted contains only '_id' and 'attack' fields, unlike existing documents.

# Insert fields only insert scenario.
db.pokemons.updateOne(
  { name: "Charmandlerise" },
  { 
    $set: { 
      hp: 75,
      defense: 90,
      attack: 198,
      speed: 120
    },
  },
  { upsert: true})

# Find documents
db.pokemons.find({ name: /^Charman/})
```

```shell
# Stopping the MongoDB image
docker stop mongodb-shell
```