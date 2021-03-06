# MongoDB backup and restore to/from Amazon S3 using Akka Streams

This is an example of using Akka Streams and Alpakka to stream:
 - MongoDB collection to AWS S3.
 - File containing Extended JSONs stored on AWS S3 to MongoDB collection.

This example contains a full runnable code presented in a two-part article:
 - [Crafting production-ready Backup as a Service solution using Akka Streams](https://medium.com/@bszwej/crafting-production-ready-backup-as-a-service-solution-using-akka-streams-130725df20cb)
- [Crafting production-ready Backup as a Service solution using Akka Streams: part 2](https://medium.com/@bszwej/crafting-production-ready-backup-as-a-service-solution-using-akka-streams-part-2-5ac84ca45b47)

## Dependencies

- Akka Streams
- Alpakka S3 connector
- MongoDB Reactive Streams driver

## Running

The scenario located in `Main` is the following:

1. Perform backup to S3.
1. Drop the collection.
1. Perform restore.

In order to run it:

1. Go to `application.conf` and fill the missing properties.

2. Run MongoDB locally and prepare the database and collection with some documents. You can quickly [run MongoDB as well as MongoShell using Docker](https://hub.docker.com/_/mongo/).

    Run MongoDB:
    ```sh
    docker run -p 27017:27017 --name backup-mongo -d mongo
    ```
    
    Run Mongo shell:
    ```sh
    docker run -it --net host --rm mongo sh -c 'exec mongo "localhost:27017"'
    ```
    
    Insert a document:
    ```sh
    > use CookieDB
    switched to db CookieDB
    
    > db.cookies.insert({"name" : "cookie1", "delicious" : true})
    WriteResult({ "nInserted" : 1 })
    ```

3. `sbt run`

## MongoDB basics

The following snippet presents a basic set of MongoDB commands useful when playing with backup/restore streams.

```sh
> show dbs
admin  0.000GB
local  0.000GB

> use CookieDB
switched to db CookieDB

> db.cookies.insert({"name" : "cookie1", "delicious" : true})
WriteResult({ "nInserted" : 1 })

> show collections
cookies

> db.cookies.find()
{ "_id" : ObjectId("599b0d9a266a67c9516e0245"), "name" : "cookie1", "delicious" : true }

> db.dropDatabase()
{ "dropped" : "CookieDB", "ok" : 1 }
```
