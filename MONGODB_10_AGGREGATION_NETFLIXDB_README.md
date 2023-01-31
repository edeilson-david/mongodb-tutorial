# Sample Database

**Pre-requisites**: 
- The MongoDB container running.

## Setup Environment

```shell
docker start mongodb-shell

# Copy data file to container
docker cp ./data/netflix.json mongodb-shell:/

# Verify if the file exists
docker exec -it mongodb-shell ls

# Import data
docker exec -it mongodb-shell mongoimport --type json --jsonArray netflix.json -d netflixdb -c netflix

# Accessing MongoDB shell
docker exec -it mongodb-shell mongosh

# Listing databases 
show dbs

# Using previously created database 
use netflixdb
db.getCollectionNames()

# Sample
db.netflix.findOne({})
```

## Exercises
```shell
db.netflix.find({
  release_year: { $lte: 2010 }
})

# Work with ISODate
db.netflix.aggregate([
  {
    $match:{
      date_added: {
        $lte: ISODate('2010-12-31'),
        $gte: ISODate('2009-01-01')
      }
    }
  },
  {
    $project: {
      title: 1
    }
  }
])
```

```shell
# Stopping the MongoDB image
docker stop mongodb-shell
```