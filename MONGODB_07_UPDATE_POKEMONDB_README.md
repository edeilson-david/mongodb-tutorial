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
    $set: { attack: 80 },
    $setOnInsert: {
      hp: 75,
      defense: 90,
      speed: 120
    }
  },
  { upsert: true}
)

# Find documents
db.pokemons.find({ name: /^Charman/})
```

## Update an array field
```shell
# Find document existed
db.pokemons.find({ _id: 1 })

# Update a specific element in array from query statement
db.pokemons.updateOne({ _id: 1, types: 'Poison' }, { $set: { 'types.$': 'PoisonX' } })
db.pokemons.find({ _id: 1 })

# Update all elements in array
db.pokemons.updateOne({ _id: 1 }, { $set: { 'types.$[]': 'PoisonY' } })
db.pokemons.find({ _id: 1 })

# Update a specific element in array
db.pokemons.updateOne({ _id: 1 }, { $set: { 'types.0': 'PoisonZ' } })
db.pokemons.find({ _id: 1 })

# Update to initial value
db.pokemons.updateOne({ _id: 1 }, { $set: { types: ['Grass', 'Poison'] } })
db.pokemons.find({ _id: 1 })

# Add an element to array
db.pokemons.find({ _id: 2 }, { name: 1, types: 1 })
db.pokemons.updateOne({ _id: 2 }, { $push: { types: 'Water' } })
db.pokemons.find({ _id: 2 }, { name: 1, types: 1 })

# Add N elements to array
db.pokemons.updateOne({ _id: 2 }, { $push: { types: { $each: ['Fire', 'Eletric'] } } })
db.pokemons.find({ _id: 2 }, { name: 1, types: 1 })

# Add an element from specific index in array 
db.pokemons.find({ _id: 3 }, { name: 1, types: 1 })
db.pokemons.updateOne({ _id: 3 }, { $push: { types: { $each: ['Fire', 'Eletric'], $position: 1 } } })
db.pokemons.find({ _id: 3 }, { name: 1, types: 1 })

# Do not allows values duplicated
db.pokemons.find({ _id: 22 }, { name: 1, types: 1 })
db.pokemons.updateOne({ _id: 22 }, { $addToSet: { types: { $each: ['Fire', 'Flying', 'Water'] } } })
db.pokemons.find({ _id: 22 }, { name: 1, types: 1 })

# Sort element from array
db.pokemons.updateOne({ _id: 22 }, { $push: { types: { $each: [], $sort: 1 } } })
db.pokemons.find({ _id: 22 }, { name: 1, types: 1 })

# Keeping N elements into array
db.pokemons.updateOne({ _id: 22 }, { $push: { types: { $each: [], $slice: 3 } } })
db.pokemons.find({ _id: 22 }, { name: 1, types: 1 })

# Keeping -N elements into array
db.pokemons.updateOne({ _id: 22 }, { $push: { types: { $each: [], $slice: -2 } } })
db.pokemons.find({ _id: 22 }, { name: 1, types: 1 })
```

## Working array as queue
```shell
# Prepare document
db.pokemons.find({ _id: 55 }, { name: 1, types: 1 })
db.pokemons.updateOne({ _id: 55 }, { $push: { types: { $each: ['Normal', 'Fire'], $sort: 1} } })

# Remove the last element
db.pokemons.updateOne({ _id: 55 }, { $pop: { types: 1 } })
db.pokemons.find({ _id: 55 }, { name: 1, types: 1 })

# Remove the first element
db.pokemons.find({ _id: 55 }, { name: 1, types: -1 })
db.pokemons.find({ _id: 55 }, { name: 1, types: 1 })

# Remove N elements from array
db.pokemons.find({ _id: 365 }, { name: 1, types: 1 })
db.pokemons.updateOne({ _id: 365 }, { $pull: { types: { $in: ['Normal', 'Fire'] } } })
db.pokemons.find({ _id: 365 }, { name: 1, types: 1 })

# Remove all elements if condition is true
db.pokemons.updateOne({ _id: 365 }, { $pullAll: { types: ['Normal', 'Fire', 'Flying'] } })
db.pokemons.find({ _id: 365 }, { name: 1, types: 1 })
```

## Structure of object into array
```shell
# Prepare document
db.pokemons.updateOne({ _id: 365 }, { $set: { types: [ { name: 'Fire', bonus_points: 45, weakness: 'Water'}, { name: 'Rock', bonus_points: 12, weakness: 'Paper'} ] } })
db.pokemons.find({ _id: 365 }, { name: 1, types: 1 })

# Update document array
db.pokemons.updateOne({ _id: 365, 'types.name': 'Fire' }, { $set: { 'types.$.bonus_points': 35 } })
db.pokemons.find({ _id: 365 }, { name: 1, types: 1 })

# Sort document array
db.pokemons.updateOne({ _id: 365, }, { $push: { types: { $each: [], $sort: { weakness: -1 } } } })
db.pokemons.find({ _id: 365 }, { name: 1, types: 1 })
```

## Find And Return
```shell
# Find original data
db.pokemons.findOne({ _id: 257 })

# Find and Update: will return old data
db.pokemons.findOneAndUpdate(
  { _id: 257 },
  { $set: { weakness: 'Ice' }}
)

# Find original data
db.pokemons.findOne({ _id: 258 })

# Find and Update: will return new data
db.pokemons.findOneAndUpdate(
  { _id: 258 },
  { $set: { weakness: 'Fire' } },
  { returnNewDocument: true }
)
```

```shell
# Stopping the MongoDB image
docker stop mongodb-shell
```