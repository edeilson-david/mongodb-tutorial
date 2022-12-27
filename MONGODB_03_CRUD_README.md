# CRUD operation

**Pre-requisites**: The MongoDB container running.

## In the terminal run the following commands:

```shell
# Accessing the MongoDB shell in the image
docker start mongodb-shell
docker exec -it mongodb-shell mongosh
```

```shell
# What CRUD means?
C - Create a resource
R - Read a resource
U - Update a resource
D - Delete a resource
```

## C - Insert, InsertOne and InsertMany commands
```shell
# Use the collection exist 
use schools

# Insert students into the collection
db.students.insert({ name: 'Maria', age: 15, hobbies: [ 'swimming', 'play tennis' ], is_student: true, schoolSubjects: { mathmatics: 8, science: 7.5 }})
db.students.insertOne({ name: 'Anna', age: 16, hobbies: [ 'volleyball', 'play tennis' ], is_student: true, schoolSubjects: { mathmatics: 8, science: 9 }})
db.students.insertMany([
    { name: 'Felipe', age: 13, hobbies: [ 'chess', 'poker' ], is_student: true, schoolSubjects: { mathmatics: 10, science: 10 }},
    { name: 'David', age: 14, hobbies: [ 'chess', 'poker', 'eletronic games', 'watch movies', 'read book' ], is_student: true, schoolSubjects: { mathmatics: 10, science: 10 }},
    { name: 'Joe', age: 17, hobbies: [ 'soccer', 'voleyball', 'basketball' ], is_student: true, schoolSubjects: { mathmatics: 7.5, science: 8 }},
    { name: "Joao", "age": 12, "hobbies": ["soccer", "swimming"], "is_student": true, "schoolSubjects": { "mathmatics": 9.5, "science": 10 }}
    ])
```

## R - Find and FindOne commands
```shell
use schools

# Find documents
db.students.find()
db.students.findOne()

# Applying filter 
db.students.find( { age: 15 } )
db.students.find( { name: "Joao" } )
db.students.findOne( { name: "David" } )

# Select only name and age fields for David student. Note: the _id field is add automatically
db.students.findOne( { name: "David" }, { name: 1, age: 1 } )

# Omitting _id field
db.students.findOne( { name: "David" }, { name: 1, age: 1, _id: 0 } )
```

## U - UpdateOne and UpdateMany commands
```shell
use schools

# Insert a new student
db.students.insert({ name: 'Bryan', age: 16, hobbies: [ 'play golf', 'watch movies' ], is_student: true, schoolSubjects: { mathmatics: 8.5, science: 9 }})
db.students.findOne( { name: 'Bryan' }, { age: 1 } )

# Update a student
db.students.updateOne( 
    { name: 'Bryan' },    /* filter */
    { $set: { age: 17 } } /* set new value */
)
db.students.findOne( { name: 'Bryan' }, { age: 1 } )

# Add the city field to all student documents
db.students.updateMany(
    {}, /* No filter. All documents */ 
    { $set: { city: 'Sao Paulo' } }
)
db.students.find( { } )

# Update the city
db.students.findOne( { name: 'Bryan' }, { city: 1 } )
db.students.updateOne(
    { name: 'Bryan' },    /* filter */
    { $set: { city: 'Curitiba' } }
)
db.students.findOne( { name: 'Bryan' }, { city: 1 } )
```

## D - DeleteOne and DeleteMany commands
```shell
use schools

# Delete one student
db.students.deleteOne({ name: 'Bryan'})
db.students.find()

# Insert a new student
db.students.insert({ name: 'Maria', age: 16, hobbies: [ 'chess' ], is_student: true, schoolSubjects: { mathmatics: 7, science: 7.5 }})
db.students.find( { name: 'Maria' } )

# Delete many students
db.students.deleteMany(
    { name: 'Maria' }
)
db.students.find( { name: 'Maria' } )
```

```shell
# Stopping the MongoDB image
docker stop mongodb-shell
```