# RU202 Redis Streams

## Week 1

### Overview of RU202
#### Welcome to RU202
* Week 1
    * Distributed Systems and Stream Processing
    * Introduction to Redis Streams
* Week 2
    * Producers
    * Consumers
    * Queries
* Week 3
    * Consumere Groups
* Week 4
    * Redis Streams Patterns and Best Practices
    * Administration

### Introduction to Distributed Systems
#### Introduction to Distributed Systems
* _Re_mote _di_ctionary _s_erver
    * Accessible over the network
    * Client-server paradigm
    * Forms part of a distributed system
* A distributed system is one in which the failure of a computer you didn't even know existed can render your own computer unusable - Leslie B. Lamport, a foundational distributed system researcher
* More components = greater probability of failure
* Stateful components are generally reliable
* Containerization makes it easy to replace faulty components
* Failure in Distributed Systems:
    * Component loss = OK
    * These components are often redundant
    * Want to avoid message loss when possible

##### Quiz
* Why is Redis considered a part of a distributed system?

[] It's written in C

[x] It uses the client-server paradigm

[x] It serves data over a network, as its full name, REmote DIctionary Server, implies

[] It implements a set of common data structures

* Which of the following increases the probability of failure in a distributed system?

() An increase in the reliability of the storage systems, such as the disks

() An increase in the redundancy of the system's components

(x) An increase in the number of system components

#### Stream Processing
* What is Stream Processing?
    * Acting on data in real time
    * Processing streams, i.e., continuous data
    * Example: Temperature monitoring
* Stream Processing Advantages:
    * Deals with recent data
    * Provides near-instantaneous insights
    * Smooth the influx of data
* Stream Processing Use Case
    * Time-series data
    * Continuous data streams
    * Data set too big to fit in memeory
    * Aggregations or summaries of larger streams

#### Stream Pipelines
##### Quiz
* Why are stream pipelines good for managing continuous flows of data?

() Because they batch up all of the data before performing any processing.

(x) Because they can effectively produce results in near real-time.

* What benefits do stream pipelines provide to distributed systems? (select all that apply):

[x] Increased throughput

[x] Asynchronous processing

[] Fewer system component failures

[x] Decoupling of system components

The use of streams helps to enable parallel processing for greater throughput, allows components to process data at different speeds from each other and provides a way for disparate system components to communicate with each other using standard messages.

* What's the name for the input to a streaming pipeline?

() The input batch

(x) The data source

() The data sink

() The database

### Introduction to Redis Streams
#### Redis Streams Overview
* Redis Streams:
    * it's a data structure!
    * Acts like an append-only list
    * Entries are hashes
    * Entries have unique IDs
    * Supports ID-based range queries
    * Consumer groups

##### Quiz
* How are streams are implemented in Redis?

() As an optional module

() Using sorted sets

(x) As a new Redis data structure

() Using RPC to Apache Spark

* How can entries can be added to a stream?

(x) By appending to the end of the stream

() By prepending to the front of the stream

() By inserting them at a specific location in the stream

() By swapping with an existing stream entry

* Which of these best describes the role of a consumer group?

() A random-access stream reader

(x) A subscriber to the stream correct

() A partial consumer of the stream

#### Comparison to Standard Redis Data Structure
* Redis Streams Comparisons:
    * Blocking/Non-Blocking
    * Mutability
    * Storage and Delivery with respect to Redis Pub/Sub
* Blocking vs. Non Blocking Commands:
    * Blocking vs. Non-Blocking
    * Does the command return immediately?
    * Important: This is not about non-blocking, or async, I/O.
* Redis Streams Blocking vs. Non-Blocking
    * When reading a stream, commands my optionally block.
    * This allows for a variety of processing strategies.
* Redis Streams Immutability
    * A stream is append-only
    * You cannot write to the beginning or middle of a stream
    * Stream entries are immutable
* Storage and Delivery
    * Redis Data structures are stored in memory
    * Streams are a data structure and thus are stored in memory, too.
    * Redis Pub/Sub messages are not kept in memory. Subscribers must be connected to the Redis server to receive Pub/Sub messages.
* Unit Review
    * Blocking vs. Non-blocking Commands
        * Redis Streams has both
    * Mutability
        * Streams are mostly immutable
    * Storage and Delivery
        * Redis Streams stores messages for later delivery

##### Quiz
* Which modes of consumption are supported by Redis streams?

() Blocking

() Non-Blocking

(x) Both blocking and non-blocking

* Can the values stored in a stream entry be changed?

() Yes

(x) No

* Which of the following is a key difference between Redis Pub/Sub and Redis Streams?

() Streams allow multiple clients to receive data data. Pub/Sub does not.

() Pub/Sub allows multiple clients to connect and receive data, but streams does not.

(x) Streams store data in memory for later consumption by clients. Pub/Sub doesn't store any messages.

() Pub/Sub stores data in memory for later consumption by clients. Streams doesn't store any messages.

#### Redis Streams Example in Python
##### Quiz
* What portion of the stream's messages will be read by each of our two consumer groups: "data_warehouse_writer" and "rolling_average_printer"?

() Each can expect to read half of the messages

(x) Each can expect to read all of the messages

### Hands On
#### Using Virtual Labs
#### Launch Your Virtual Lab
#### Hands On Exercise

### Homework
#### HW 1.1
#### HW 1.2
#### HW 1.3
#### HW 1.4
#### HW 1.5
#### HW 1.6

### Wrap Up
* Recap
    * Intro to streams and stream processing
    * Basics of Redis Streams
    * Redis Streams vs. Other Data Structures
    * Example in Python