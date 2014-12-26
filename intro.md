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

Find all documents.

SQL:

	SELECT * FROM census;
	
MongoDB:

	> db.census.find()
	{ "_id" : ObjectId("549c8963a1d633b04b55e1b1"), "district" : "Taplejung", "vdc" : "Liwang", "households" : 359, "male" : 810, "female" : 943 }
	{ "_id" : ObjectId("549c8963a1d633b04b55e1b2"), "district" : "Taplejung", "vdc" : "Mamangkhe", "households" : 240, "male" : 519, "female" : 616 }
	...
	Type "it" for more
	
	// pretty prints in shell.
	> db.census.find().pretty()
	{
        "_id" : ObjectId("549c8963a1d633b04b55e1b2"),
        "district" : "Taplejung",
        "vdc" : "Mamangkhe",
        "households" : 240,
        "male" : 519,
        "female" : 616
	}
	...
	Type "it" for more

SQL:

	SELECT * FROM census WHERE district = 'Baglung' AND vdc = "Rayadanda";

MongoDB:
	
	> 
	{
        "_id" : ObjectId("549c8963a1d633b04b55ebde"),
        "district" : "Baglung",
        "vdc" : "Rayadanda",
        "households" : 549,
        "male" : 1008,
        "female" : 1321
	}
	>

Specify which keys to return -

SQL:

	SELECT vdc, households FROM census WHERE district = 'Baglung';

MongoDB:

	> db.census.find({district : "Baglung"}, {vdc : 1, households : 1})
	{
        "_id" : ObjectId("549c8963a1d633b04b55ebc2"),
        "vdc" : "Devisthan",
        "households" : 1616
	}
	...
	Type "it" for more
	>
	
	// note that _id key is returned by default.
	// be explicit to prevent _id from being returned.	
	> db.census.find({district : "Baglung"}, {_id : 0, vdc : 1, households : 1}).pretty()
	{ "vdc" : "Darling", "households" : 1156 }
	{ "vdc" : "Devisthan", "households" : 1616 }
	...
	Type "it" for more
	>
	
	// returns all keys except _id.
	> db.census.find({district : "Baglung"}, {_id : 0}).pretty()

### Query conditionals :
- $lt (<)
- $lte (<=)
- $gt (>)
- $gte (>=)
- $ne (!=)

SQL:

	SELECT * 
	FROM   census 
	WHERE  district = 'Kathmandu' 
		   AND households >= 10000 
		   AND households <= 20000;

MongoDB:

	> db.census.find({
	...     district: "Kathmandu",
	...     households: {
	...         "$gte": 10000,
	...         "$lte": 20000
	...     }
	... })

### OR Queries

Two ways :
- Single key - $in, $nin
- Multiple keys - $or

SQL:

	SELECT * 
	FROM   census 
	WHERE  district IN ( 'Baglung', 'Kathmandu' ); 
	
MongoDB:

	// single key - $in, $nin
	> db.census.find({district : {"$in" : ["Baglung", "Kathmandu"]}})
	> db.census.find({district : {"$nin" : ["Baglung", "Kathmandu"]}})

SQL:

	SELECT * 
	FROM   census 
	WHERE  district IN ( 'Baglung', 'Kathmandu' ) 
			OR households > 20000;

MongoDB:		

	// multiple keys - $or
	> db.census.find(
	...   {"$or": [{district: "Kathmandu"},
	...            {households: {"$gt": 20000}}]
	...    })


## Limits, Skips, Sorts

	> db.census.find().limit(3)
	
	// skips first 10 documents.
	> db.census.find().skip(10)
	
	// skips first 10 documents and returns 3 documents after the 10th.
	> db.census.find().skip(10).limit(3)
	
	// sorting order, asc = 1, desc = -1
	> db.census.sort({district : 1, households : -1})

## Querying Arrays


## Querying Embedded Documents
Two ways :
- querying for whole document
- querying for individual key/value pairs inside an embedded docuement

## Querying for whole document

	> db.conflict.find({
	...     "date-of-disappear": {
	...         day: 20,
	...         month: 11,
	...         year: 2002
	...     }
	... }).pretty()
	{
			"_id" : ObjectId("5499243bc2e9fab12b22d3d7"),
			"place-of-disappear" : {
					"district" : "Doti",
					"place" : "Dipayal"
			},
			"name" : "Bhoj Bahadur Mijar",
			"date-of-birth" : {
					"day" : 0,
					"month" : 0,
					"year" : 1972
			},
			"date-of-disappear" : {
					"day" : 20,
					"month" : 11,
					"year" : 2002
			},
			"father-name" : "Bhaire Mijar",
			"dead-or-missing" : "Death",
			"gender" : "M",
			"place-of-birth" : "Achham",
			"district" : "Achham"
	}
	...
	
Note that a query for sub-document must exactly match the sub-document even changing the order of key/value pair does not work.

	// these queries returns nothing
	> db.conflict.find({
	...     "date-of-disappear": {
	...         month: 11,
	...         year: 2002
	...     }
	... })
	>
	> db.conflict.find({
	...     "date-of-disappear": {
	...         month: 11,
	...         year: 2002,
	...         day: 20,
	...     }
	... })
	>
	
If possible, it’s usually a good idea to query for just a specific key or keys of an embedded
document. Then, if your schema changes, all of your queries won’t suddenly break
because they’re no longer exact matches. You can query for embedded keys using dot-notation:

	> db.conflict.find({"date-of-disappear.year" : 2002}).pretty()
	...
	{
        "_id" : ObjectId("5499243bc2e9fab12b22d3f8"),
        "place-of-disappear" : {
                "district" : "Bardiya",
                "place" : "Mahamadpur"
        },
        "name" : "Bali Ram Tharu",
        "date-of-birth" : {
                "day" : 0,
                "month" : 0,
                "year" : 1982
        },
        "date-of-disappear" : {
                "day" : 24,
                "month" : 6,
                "year" : 2002
        },
        "father-name" : "Phattu Tharu",
        "dead-or-missing" : "Death",
        "gender" : "M",
        "place-of-birth" : "Bardiya",
        "district" : "Bardiya"
	}
	...


## Indexing


## Aggregation
Aggregation framework lets you transform and combine documents in a collectioin.

### Pipeline Operations:
- $match
- $project
- $group
- $unwind
- $sort
- $limit
- $skip

## Replication
