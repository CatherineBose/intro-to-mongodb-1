# Introduction to MongoDB

## Data Types
_null_ <br />
represents a null value or non-existential value.

	> {"a" : null}
_boolean_ <br />
true or false.

	> {"a" : true}

_Number_ <br />

	> {"a" : 3.14}
Shell defaults to using 64-bit floating point numbers.
Use NumberInt (4 byte) or NumberLong (8 byte) integers.

	> {"a" : NumberInt(10)}
	> {"a" : NumberLong(10)}

_String_ <br />
Any string of UTF-8 characters.

	> {"name" : "Tilak Thapa"}


