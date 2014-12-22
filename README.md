# Introduction to MongoDB

## Data Types
**_null_** <br />
Represents a null value or non-existent field.

	> {"a" : null}
**_boolean_** <br />
Represents true or false.

	> {"a" : true}

**_Number_** <br />
Shell defaults to using 64-bit floating point numbers.

	> {"a" : 3.14}

Use NumberInt (4 byte) or NumberLong (8 byte) integers.

	> {"a" : NumberInt(10)}
	> {"a" : NumberLong(10)}

**_String_** <br />
Represents any string of UTF-8 characters.

	> {"name" : "Tilak Thapa"}

**_date_** <br />
Represents a date stored as milliseconds since the epoch.

	> {"dob" : new Date()}
	
	> Date() // returns current date as string.
	Mon Dec 22 2014 15:58:22 GMT+0545 (Nepal Standard Time)
	>
	> new Date() // constructs a Date object using the ISODate() wrapper.
	ISODate("2014-12-22T09:57:30.801Z")
	>
	> new ISODate() // constructs a Date object.
	ISODate("2014-12-22T09:57:35.021Z")
	>


	


