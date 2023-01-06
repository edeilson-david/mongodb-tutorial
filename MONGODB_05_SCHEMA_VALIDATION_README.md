# CRUD operation

**Pre-requisites**: The MongoDB container running.

## In the terminal run the following commands:

```shell
# Accessing the MongoDB shell in the image
docker start mongodb-shell
docker exec -it mongodb-shell mongosh
```

## Insert some documents
```shell
# Initialize a database 
use cars

# Insert cars into the collection
db.car.insert({ name: 'Mustang', year: 2022, color: 'Blue' })
db.car.insert({ name: 'Camaro', year: '2018', color: 'Yellow' })
db.car.insert({ name: 'Challenger', year: 2020, color: 'Red' })
```

## Find each one
```shell
use cars

# Find documents
db.car.find({ year: 2020  })
db.car.find({ year: 2022  })
db.car.find({ year: 2018  }) # No results.

# The Camaro car was not returned because the query filter the year field as number, 
# but in the document this field is a string.
```

## Applying schema validation
```shell
use cars_v2

# Creating schema
db.createCollection("car", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name", "year"],
      properties: {
        name: {
          bsonType: "string",
          description: "Car name",
          minLength: 0,
          maxLength: 120,
        },
        year: {
          bsonType: "int",
          minimum: 1200,
          maximum: 2100,
          minLength: 4
        },
        color: {
          bsonType: "string"
        }
      }
    }
  }
})
```

## Insert some documents
```shell
use cars_v2

# Insert cars into the collection
db.car.insert({ name: "Mustang", year: 2022, color: "Blue" })
db.car.insert({ name: 'Challenger', year: 2020, color: 'Red' })
db.car.insert({ name: 'Camaro', year: '2018', color: 'Yellow' }) # Invalid. Year as string.
db.car.insert({ name: 'Porshe', year: 2120, color: 'Black' }) # Invalid. Year out of range.
```

## Capped collection
```shell
use logsapp

db.createCollection('logs', {
    capped: true,
    size: 2048,
    max: 5
  }
)

# Insert cars into the collection
db.logs.insertOne({ message: 'INFO - Hello.' })
db.logs.insertOne({ message: 'INFO - Log application 1.' })
db.logs.insertOne({ message: 'INFO - Log application 2.' })
db.logs.insertOne({ message: 'INFO - Log application 3.' })
db.logs.insertOne({ message: 'INFO - Log application 4.' })
db.logs.find()
db.logs.insertOne({ message: 'INFO - Log application 5.' }) # At this moment, MongoDB will remove the first document inserted.
db.logs.find()
```

```shell
# Stopping the MongoDB image
docker stop mongodb-shell
```