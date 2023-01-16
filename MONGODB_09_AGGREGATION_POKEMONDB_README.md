# Sample Database

**Pre-requisites**: 
- The MongoDB container running.
- Pokemon database up.

## Setup Environment

```shell
docker start mongodb-shell

# Copy data file to container
docker cp ./data/combats.csv mongodb-shell:/

# Verify if the file exists
docker exec -it mongodb-shell ls

# Import data
docker exec -it mongodb-shell mongoimport -d pokemondb -c combats --type csv combats.csv --headerline

# Accessing MongoDB shell
docker exec -it mongodb-shell mongosh

# Listing databases 
show dbs

# Using previously created database 
use pokemondb
db.getCollectionNames()
```

## Aggregate
```shell
db.combats.aggregate([
  // Stage 1
  { 
    $lookup: { 
      from: "pokemons",
      localField: "First_pokemon",
      foreignField: "_id",
      as: "pokemon1_array"
    }  
  },
  
  // Stage 2
  { 
    $lookup: { 
      from: "pokemons",
      localField: "Second_pokemon",
      foreignField: "_id",
      as: "pokemon2_array"
    }  
  },
  
  // Stage 3
  { 
    $project: { 
      _id: 0,
      Winner: 1,
      pokemon1: {
        $arrayElemAt: [ "$pokemon1_array", 0 ]
      },
      pokemon2: {
        $arrayElemAt: [ "$pokemon2_array", 0 ]
      }
    }
  },
  
  // Stage 4
  { 
    $project: {
      first_pokemon: "$pokemon1.name",
      second_pokemon: "$pokemon2.name",
      winner: {
        $cond: { 
          if: {
            $eq: ["$Winner", "$pokemon1._id"]
          },
          then: "$pokemon1.name",
          else: "$pokemon2.name"
         }
      }
    }
  }
])

```

## Visit the site to learn more about index types and properties
https://www.mongodb.com/docs/manual/indexes/

```shell
# Stopping the MongoDB image
docker stop mongodb-shell
```