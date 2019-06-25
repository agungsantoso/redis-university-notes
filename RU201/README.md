# RU201 RediSearch

## Week 1
### Overview of RU201
#### Welcome to RU201
* RediSearch:
    * Search & secondary index
    * Use cases:
        * Catalogue search
        * Autocomplete
        * Log search
        * Etc.
    * Refine:
        * By value
        * By geospatial position
        * By range
* This Course:
    * Intro to Modules
    * Fundamentals of Search
    * Fundamentals of Secondary Index
    * Redis vs. RediSearch
    * Query Language
    * Schema Creation
    * Dealing with Documents
    * Synonims
    * SUggestions and Autocomplete

### Modules
#### What is a Module
* Java/Ruby/Python => Runtime => x86 Machine Code
* Redis: C => x86 Machine Code
* Redis module implemented as a shared object, contain subroutine that allow the Redis server to integrate a module at runtime
    Redis Server as machine code (FFI) => Module shared object (x86 machine code)
* Module integration:
    * Through the redis.conf file
    * Through a command-line argument
    * With the MODULE LOAD command from within Redis
    * Through the Redis Enterprise GUI

##### Quiz
* What language is Redis written in?

() C++

(x) C

() Java

() Rust

* What mechanism does Redis use to load modules into the server?

() Source-level inclusion

() ODBC

(x) Shared object file

() OpenGL

* How does the Redis server invoke the subroutines in a Redis module?

(x) Foreign Function Interface

() Self-contained VM

() Object Linking and Embedding (OLE)

() stunnel

#### How are Modules Different?
Property | Modules | Lua
--- | --- | ---
Can execute Redis commands |  Y | Y
Can create new data-types | Y | N
Can create new commands | Y | N
Interpreted | N | Y
Can be created on-the-fly from within the Redis client | N | Y
Can control the atomicity of commands | Y | Y
Has execution time limits | N | Y
Limited to a single execution thread | N | Y

* Modules
    * How they are written
    * Integration
    * Similarities and Differences

##### Quiz
* Lua is…

() a language for implementing Redis modules

() the first name of the creator of Redis

() the name of the Redis serialization protocol

(x) a language embedded into Redis for database level scripting

* Can additional Redis data types be implemented in Lua?

() Yes

(x) No

### Search Engine Concepts
#### Introduction and Concepts
* Collection of documents make up an index
* A document is identified in an index by a document ID

##### Quiz
* The index is made of a collection of?

(x) documents

() rows

() columns

() sorted sets

* Can Indexes contain references to docments without storing the document itself?

(x) Yes

() No

* Must every field stored in the index be described in a schema?

() Yes

(x) No

Any number of fields can be stored in any document, but only the fields that will be queryable need to be in the schema

#### Tokenization and Stop Words
* Tokenizing punctuation
* To escape from tokenization use \\
* RediSearch default stop words: a, is, the, an, and, are, as , at , be ,but, by, for, if, in, into, it, no, not,of,on,or,such,that,their,then,there,these,they,this,ti,was,will,with

##### Quiz
* Given the phrase:

```
"John, will you return my high-resolution gauge?"
```

What are the correct resulting tokens given the default stop words?

() john will you return my high resolution gauge

(x) john you return my high resolution gauge

() john you return my high-resolution gauge

() john return high resolution gauge

#### Stemming
* Stemming: find the root of word

##### Quiz
* Which of the following types of data would not benefit from the use of a stemmer?

() A long passage from a book

(x) The author of a book

() The title of a book

() The titles of the chapters in a book

* Which of the following is affected by the language that a document is written in?

() Word order

() Schema fields

() The encoding of the text

(x) Selection of the correct stemming function

### Secondary Index Concepts
#### RediSearch as a Secondary Index
* Pros:
    * Optimized use of memory resources
    * Deals well with large documents
    * More effieciently optimized for high scale and speed
    * Flexibility by decoupling storage from index
    * Unify multiple data stores
* Cons:
    * Increased complexity
    * Potentially out-of-date
    * 2 stage latency

##### Quiz
* Why might you use RediSearch as a secondary index?

[x] Another datastore cannot or cannot easily find data using the required parameters

[x] You want change how documents are found independently of how they are stored

[x] You have data stored in multiple locations and with a variety of services and you want to be able to search all at the same time

[] None of the above

### RediSearch vs Redis
#### Keyspace vs Index
* When you add a document to an index in RediSearch you're actually creating multiple keys

##### Quiz
* Assuming that you have a pre-populated index called foo with 1000 documents. How many documents would you have after running the following command?

() 999

() 0

(x) 1000

() It depends

#### Documents vs Keys 
* Keyspace division:
    * Cluster through Redis Enterprise
    * One index, one database

##### Quiz
* You have an index called "cartoon-shows" and another index called "vitamin-brands" in the same database. You add a number of shows to the "cartoon-shows" index. Later you add some brands to "vitamin-brands". Both indexes have a document with the ID of "Flinstones" with different data. Is this a problem, and if so why?

() Not a problem. Each index is fully independent.

() Not a problem. Each index is not fully dependent but ID is not relevant

(x) Problem. The document for "Flinstones" in "cartoon-shows" overlaps with the document for "Flinstones" in "vitamin-brands". correct

() Potentially a problem. It depends how many documents are in the indices.

#### Schemas & Types
  | RDBMS | NoSQL (General) | NoSQL (RediSearch)
 --- | --- | --- | ---
 Schema Required | Yes | Sometimes | Yes, but partial
 Typed | Strictly | Freeform | Broadly
 Alter | Painful | N/A or Flexible | Flexible with special considerations

* Text fields
    * Human language
    * No need to specify size or length
    * Max 128 text fields per schema
    * Stores exact text
    * Capitalization and punctuation is not preserved for indexing
    * Can be weighted
    * Can be sortable
    * Fieldless queries
* Numeric fields
    * Non-textual
    * Countable
    * Space optimized
    * Whole & floating point
    * Sortable
    * Range-able
* Tag fields
    * Text-based
    * Represent a collection of data flags
    * Individual tags do not need predefinition
    * Stored as-is
    * No stemming or stop words
    * Specifically optimized
    * ~ tags per schema
* Geo fields
    * Store longitude and latitude
    * Stored as Sorted Sets / GeoHash
* Payloads
    * One per document
    * Binary-safe
    * Not directly indexable

##### Quiz
* You have a catalogue of movies and these movies may have no subtitles or subtitles in many different languages. Which type would you use to store the subtitle languages if you want to answer the following questions:

    * Which movies have subtitles in Estonian and Finnish?
    * Which movies do not have subtitles in French?
    * Which movies have subtitles in Tagalog or Esperanto?

Consider the most effective and efficient option.

() Text

(x) Tag

() Numeric

() Geo

#### Recap
* Manipulating indices
* Managing documents
* Schemas
* The types of data that RediSearch can index

### Week 1 Homework
#### HW 1.1
#### HW 1.2
#### HW 1.3
#### HW 1.4
#### HW 1.5
#### HW 1.6