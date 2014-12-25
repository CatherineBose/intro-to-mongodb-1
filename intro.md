# Introduction to MongoDB

## Terms
| SQL      | MongoDB             |
|----------|---------------------|
| database | database            |
| table    | collection          |
| row      | document            |
| column   | field               |
| index    | index               |
| joinning | embedding & linking |

## Querying

// find documents
// SELECT * FROM census
db.census.find()

db.census.find().pretty()

// SELECT * FROM census WHERE district = 'Kathmandu'
db.census.find({district : "Kathmandu"})

// specify which keys to return

// SELECT district, vdc, households FROM census WHERE district = 'Kathmandu'
db.census.find({district : "Kathmandu"}, {district : 1, vdc : 1, households : 1})
// note that _id key is returned by default

db.census.find({district : "Kathmandu"}, {_id : 0, district : 1, vdc : 1, households : 1})
db.census.find({district : "Kathmandu"}, {_id : 0}) // get all keys except _id

/******************
Query conditionals :
$lt, $lte, $gt, $gte, $ne
*/
db.census.find({district : "Kathmandu", households : {"$gte" : 1000, "$lte" : 2000}}) // AND

/*****************
OR Queries
Two ways :
1) $in, $nin - single key
2) $or - multiple keys
*/

db.census.find({district : {"$in" : ["Baglung", "Kathmandu"]}})
db.census.find({district : {"$nin" : ["Baglung", "Kathmandu"]}})

// $or
db.census.find({district : {"$or" : {district : "Kathmandu", households : {"$gt" : 20000}}}})


/*
Querying Arrays

*/


/*
Querying Embedded Documents
*/


/*
Limits, Skips, Sorts
*/
db.census.find().limit(3)
db.census.find().skip(10)
db.census.find().skip(10).limit(3)
db.census.sort({district : 1, households : -1})


/*
Indexing
*/




/*
Aggregation

pipelining

*/
db.census.find({"$group", {_id : "$district", total_households : {"$sum" : "$households"}}})


/*
Replication

*/

