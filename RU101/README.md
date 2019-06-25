# RU101 Introduction to Redis Data Structures

## Week 1

### Data Structures - Part One
#### What are Redis Data Sturctures?
* Redis data types
    * Strings
    * Lists
    * Sets
    * Hashes
    * Sorted sets
    * Bitmaps and HyperLogLogs

#### Keys
* Redis Keys:
    * Unique
    * Case sensitive
    * Binary safe
        * "Foo", 42, 3.1415, 0xff
    * Key names can be up to 512MB
    * Length versus Readability
* Key spaces:
    * Logical Databases
    * Flat key space
    * No automatic namespacing
    * Naming conventions
* Logical Databases:
    * Database Zero (zero based index)
    * Key spaces within a database
    * Why not use Databases?
* Example:
    * user:id:followers
        * "user:1000:followers"
            * user: object name
            * 1000: unique identifier of the instance
            * followers: composed object
* Redis response as string using double quotes convention

```
SET key value [EX seconds] [PX milliseconds] [NX|XX]
GET key
```

KEYS: | SCAN:
---|---
Blocks until complete | Iterates using a cursor
Never use in production | Returns a slot reference
Useful for debugging | May return 0 or more keys per call
  | Safe for production

```
SCAN slot [MATCH pattern] [COUNT count]
```

```
DEL key [key ...]
UNLINK key [key ...]
```

* Unlink is non blocking command

```
EXISTS key [key ...]
```

* NX on SET means only set if key not exists, XX on SET means only set if key exists

##### Quiz
* Which of the following are valid for Redis key names?

[x] The string "Foo"
[x] A binary string
[] More than 512 MB
[x] Less than 512 MB

* What can a key name be made up of?

() ASCII characters only
(x) Any sequence of bytes correct
() Alphanumeric and underscore only
() Any alphanumeric, excuding $ symbol
() Any alphanumeric, but cannot start with "system" or "sys"

#### Key Expiration
Set Expiration | Inspect Expiration | Remove Expiration
--- | --- | ---
EXPIRE | PTTL | PERSIST
EXPIREAT | TTL | 
PEXPIRE | | 
PEXPIREAT | | 

* Expiration times can be set
* Expiration can be changed by the user
* Expiration can be removed
* TTL Commands:
    * Set
        * EXPIRE key seconds
        * EXPIREAT key timestamp
        * PEXPIRE key milliseconds
        * PEXPIREAT key milliseconds timestamp
    * Examine:
        * TTL key
        * PTTL key
    * Remove:
        * PERSIST key
* Redis Key Names:
    * Unique key
    * Binary strings
    * Flat namespace
    * Can be up to 512MB
    * Presences/absences can be used to determine if a set occurs
    * Can be automatically expired

##### Quiz
* How can the expiration of keys be set?

[x] In seconds
[x] In milliseconds
[x] In Unix epoch time

#### Strings
* Strings:
    * Text data
    * Integers and Floating Point numbers
    * Binary data
    * Maximum 512MB
* Manipulating Strings:
    * DECRBY key decrement
        * DECR key
    * INCRBY key increment
        * INCR key
    * INCRBYFLOAT key increment
* Strings:
    * Basic form of storage
    * Encoding verified before command execution
    * Strings can be manipulated as Text, Floats and Integers

##### Quiz
* What can be stored in a Redis String?

[x] Text
[x] Integers
[x] Floating point numbers
[x] Binary data

* What can the INCR command work on?

() Text
(x) Integers
() Floating point numbers
() Binary data

#### Hashes
* Hashes:
    * Key with named fields
    * Single level
    * Provide commands on those fields e.g. INCR
    * Dynamically add and remove fields
    * Stored extremely efficiently
    * Are not recursive
* HSET key field value [field value ...]
* HDEL key field [field ...]
* HINCRBY key field increment
* HINCRBYFLOAT key field increment
* HSET key field value
* HSETNX key field value
* Field access:
    * HGET key field
    * HGETALL key
    * HSCAN key cursor [MATCH patter] [COUNT count]
    * HMGET key field [field ...]
    * HKEYS key
    * HVALS key
* Use cases:
    * Rate Limiting
        * hmset ep-20180210
            "/pet/{petid}" 100
            "/booking/{petid}" 100
        * hincrby ecp-20180210
            "/pet/{petid}" -1
        * expire ep-20180210 86400
    * Session cache
        * hmset session:a3fWa ts 1518132669 host www.example.org
        * hincrby session:a3fWa requests 1
        * expire session:a3fWa 60

##### Quiz
* If the following commands are executed:

```
hset hash-one a "abc"
hincrby hash-one b 10
```

What is returned when the following is executed?

```
hgetall hash-one
```

() 1) "a"
   2) "abc"
 
() 1) "a"
   2) "abc"
   3) "b"
   4) "0"
 
(x) 1) "a"
    2) "abc"
    3) "b"
    4) "10"

* If the following command is executed:

```
hset hash-two a 123
```

What is returned when the following is executed?

```
hget hash-two foo
```

() "" (empty string)
() Error: Field does not exist.
(x) (nil) correct

#### Lists
* Lists:
    * Ordered collection of Strings
    * Duplicates are allowed
    * Elements can be added and removed at Left or Right
    * Elements can be inserted relative to another
    * Used to implement Stacks and Queues
    * Are not nested
    * Implemented as a linked list
* PUSH:
    * LPUSH key value [value ...]
    * RPUSH key value [value ...]
* POP:
    * LPOP key
    * RPOP key
* LLEN key
* LRANGE key start stop
* LINDEX key index
* LINSERT key BEFORE|AFTER pivot value
* LSET key index value
* LREM key count value
* Use Cases:
    * Activity Stream
        * lpush stream one two three four five
        * lrange stream 0 2
        * ltrim stream 0 3
    * Inter process communication
        * Produce
            * rpush queue "event1"
        * Consume
            * lpop queue
* Lists:
    * Ordered elements
    * Duplicate are allowed
    * Elements can be added and removed at Right or Left
    * Elements can be inserted relative to another
    * Implemented as a linked list

##### Quiz
* If the following is executed

```
rpush list-two a b c d
lpop list-two
```

What does this command output?

```
lindex list-two 1
```

() a
() b
(x) c
() d

* What happens when the following commands are executed?

```
set list-four abc
rpush list-four a b c d
```

What does list-four now contain?

() list-four contains "abc", "a", "b", "c", "d"
() list-four contains "a", "b", "c", "d"
(x) An error occurs correct

#### Review
* Basic Redis Data Structures:
    * Keys & Expiration
    * Strings
    * Hashes
    * Lists

### Data Structures - Part Two
#### Introduction to Redis Collections
* This Chapter:
    * Sets & Sorted Sets

#### Sets - Part One
* Sets:
    * Unordered collection of unique strings
    * Allows for set operations
        * Difference
        * Intersect
        * Union
    * Are not nested
* SADD key member [member ...]
* SMEMBERS key
* SSCAN key cursor [MATCH pattern] [COUNT count]
* SISMEMBER key member
* SREM key member [member ...]
* SPOP key [count]

##### Quiz
* Can a Set contain duplicate values?

() Yes
(x) No

* Can you retrieve a value in a Set by position?

() Yes
(x) No

#### Sets - Part Two
* SDIFF
    * SDIFFSTORE
* SINTER
    * SINTERSTORE
* SUNION
    * SUNIONSTORE
* Use Cases:
    * Tag Cloud
        * SADD wrench tool metal
        * SADD coin courrency metal
        * SSCAN wrench match*
        * SINTER wrench coin
        * SUNION wrench coin
    * Unique visitors
        * URL + Time period
        * SADD about.html:20180210 jim jane john
        * SSCAN about.html:20180210 match *
        * EXPIRE about.html:20180210 86400

##### Quiz
* Given the following commands

```
sadd set-one a b c d e
sadd set-two e f g
sadd set-three c e
```

What does the following command output?

```
sdiff set-one set-two set-three
```


(x) 1. "a"
    2. "b"
    3. "d"

() 1. "a"
   2. "b"
   3. "d"
   4. "e"
   5. "f"
   6. "g"
 
() 1. "e"

#### Sorted Sets - Part One
* Sorted Sets:
    * Ordered collection of unique strings
    * Floating point Score
    * Manipulation by value, position, score or lexicography
    * Set commands Sorted Sets:
        * Union
        * Intersection
    * Are NOT nestable
* ZADD KEY [NX|XX] [CH] [INCR] SCORE MEMBER [SCORE MEBER ...]
* ZRANGE key start stop [WITHSCORES]
* ZREVRANGE key start stop [WITHSCORES]
* ZRANK key member
* ZREVRANK key member
* ZSCORE key member
* ZCOUNT key min max
* ZRANGE key start stop [WITHSCORES]
* ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]
* ZRANGEBYLEX key min max [LIMIT offset count]
* ZREM - By value
* ZREMRANGEBYLEX - Lexicographically
* ZREMRANGEBYRANK - Position
* ZREMRANGEBYSCORE - Score

##### Quiz
* If the following command is executed

```
zadd q-1 10 a 20 b 30 c 40 d
```

What is the Rank of of "b" element?

() 0
(x) 1
() 2
() 3
() 4

* Can members be added to a Sorted Set with the same score?

(x) Yes
() No

* Can elements of a Sorted Set have the same Rank?

() Yes
(x) No

* Can you have two elements in a Sorted Set with the same value but different rank?

() Yes
(x) No


#### Sorted Sets - Part Two
* ZINTERSTORE destination numkeys key [key ...] [WEIGHTS weight [weight]] [AGGREGATE SUM|MIN|MAX]
* ZUNIONSTORE destination numkeys key [key ...] [WEIGHTS weight [weight]] [AGGREGATE SUM|MIN|MAX]
* Leaderboards:
    * ZADD lb 90 jane 100 john 75 bob 67 kevin 88 mel 22 kate
    * ZINCRBY lb 50 jane
    * ZREVRANGE lb 0 2 withscores
    * ZREMRANGEBYRANK lb 0 -4
* Priority Queue:
    * ZADD pq 1 p1-item1 2 p2-item1 3 p3-item1 1 p1-item2
    * ZRANGE pq 0 0
    * ZREM pq p1-item1
* Sorted Sets:
    * Ordered collection of unique strings
    * Floating point Score
    * Manipulation by value, position, score or lexigraphically
    * Set commands Sorted Sets:
        * Union
        * Intersection
    * Typical Use Cases:
        * Priority Queue
        * Leaderboards

##### Quiz
* What set operators do Sorted Sets support?

[] Difference
[x] Intersection
[x] Union

#### Review

### Lab

### Homework
#### HW 1.1
Can you define an Expiration time of a field in a Hash?
() Yes
(x) No

#### HW 1.2
What command is used to set a value in a Hash, only if the field does not already exist?
() HEXISTS
() HMSET
() HSET
(x) HSETNX

#### HW 1.3
What criteria does Redis use to determine if an operation can be performed against a key?
[x] Datatype
[x] Encoding of value
[] Time of day

#### HW 1.4
If the following commands are executed:

```
set hw1-4 123
incr hw1-4
append hwq-4 "abc"
```

What does the GETRANGE command return?

```
getrange hw1-4 1 2
```

() Nil
() 12
() 23
(x) 24
() ab
() bc
() bc1

#### HW 1.5
The Hash `event:UOYW-ZJAB-SZZQ-RLUJ` in the sample data (in the virutal lab) holds details about an event. What value is stored in the `name` field?

() Nil
() "Judo"
() "Swimming"
(x) "Taekwondo"

#### HW 1.6
The Sorted Set `invoice_totals` in the sample data, contains the _order total_ as the score and the _order id_ as the value, for each sale made.

What is the order total, for the order with the highest order value?

() 0
() 2365
(x) 3150
() 3275

#### HW 1.7
If the following commands are executed:

```
sadd hw1-7 "hello"
spop hw1-7
```

What does the folowing EXISTS command return?

```
exists hw1-7
```

(x) 0
() 1

#### HW 1.8
If a Sorted Set is created as follows:

```
zadd hw1-8 1 a 2 b 3 c 4 d 5 e 6 f
```

What command would return members with a score greater than 3, regardless of the current members of the Sorted Set

() zrange hw1-8 3-1
() zrange hw1-8 4 6
(x) zrangebyscore hw1-8 (3 +inf
() zrangebyscore hw1-8 (3 6