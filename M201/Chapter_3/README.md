https://university.mongodb.com/mercury/M201

# Chapter_3

* Tradoffs between foreground and background indexes
* How to use background indexes
* Using currentOp() and killOp()

```sh
wget https://s3.amazonaws.com/edu-downloads.10gen.com/M201_2019_May/static/handouts/restaurants.json.zip

unzip restaurants.json.zip

mongoimport --host Sandbox-shard-0/sandbox-shard-00-00-felrd.mongodb.net:27017,sandbox-shard-00-01-felrd.mongodb.net:27017,sandbox-shard-00-02-felrd.mongodb.net:27017 \
            --ssl --username m001-student \
            --password <PASSWORD> \
            --authenticationDatabase admin \
            --db m201 \
            --collection restaurants \
            --type json \
            --file restaurants.json
```


## Build Operations

* Background Indexes are slower, but don't block operations

[Build Operations](https://docs.mongodb.com/manual/core/index-creation/?jmp=university)

## Query Plans

[Query Plans](https://docs.mongodb.com/manual/core/query-plans/?jmp=university)

## Explain

```js
// Object explain
exp = db.collection.explain()
exp.find({})
```

[Explain](https://docs.mongodb.com/manual/reference/method/cursor.explain/)
[deciphering-explain-output](https://www.mongodb.com/presentations/deciphering-explain-output)

* Execution time
* Number of keys read, documents read and documents returned
* Plans selected and rejected

## Forcing Indexes with Hint

```js
db.collection.find({field:"value"}).hint({field:1})
db.collection.find({field:"value"}).hint("name_of_the_index")
```

[hint](https://docs.mongodb.com/manual/reference/method/cursor.hint/#cursor.hint)

## Resource Allocation for Indexes

* Determine Indez Size

```js
db.collection.totalIndexSize()
```

* Resource Allocation
	- Disk
	- Memory

* Define the indexes stats of the collection

```js
db.collection.stats({indexDetails:true})

var stats = db.collection.stats({indexDetails:true})
stats.indexDetails.index_name.cache
```

* Determine the cache size in initialization

```js
storage.wiredTiger.engineConfig.cacheSizeGB and --wiredTigerCacheSizeGB.
```

[WiredTiger](https://docs.mongodb.com/manual/core/wiredtiger/)

* Edge Cases
	- Occasional Reports
	- Right-end-side index increments

## Basic Benchmarking

* Public Test Suite
* Specific or Private Testing Enviroment

### Low Level Benchmarking

* File I/O Performance
* Scheduler Performance
* Memory allocation and transfer speed
* Thread performance
* Database server performance
* Transaction Isolation
* ...

* Tools for benchmarking

[sysbench](https://launchpad.net/sysbench)
[iiBench](https://github.com/tmcallaghan/iibench-mongodb)

### Database Server Benchmarking

* Data set load
* Writes per second
* Reads per second
* Balanced workloads
* Read/Write ratio

[YCSB](https://github.com/brianfrankcooper/YCSB)
[TPC Benchmarks](http://www.tpc.org/information/benchmarks.asp)

### Distributed Systems Benchmarking

* Linearization
* Serialization
* Fault tolerance

[HiBench](https://github.com/Intel-bigdata/HiBench)
[jepsen](https://github.com/jepsen-io/jepsen)

### Small Execution Benchmarking

[POCDriver](https://github.com/johnlpage/POCDriver)

### Benchmarking Anti-Patterns

* Database swap replace
* Using mongo shell for write and read requests

```js
for(i=0;i<1000;i++){db.benchmark.insert({'v':i})}
```

* Using mongoimport to test write response
* Local laptop to run tests
* Using default MongoDB parameters

### Benchmarking Conditions

* Hardware
* Clients
* Load

## Exercise

```js
var exp = db.restaurants.explain("executionStats")
exp.find({ "address.state": "NY", stars: { $gt: 3, $lt: 4 } }).sort({ name: 1 }).hint(REDACTED)

{ "address.state": 1, "stars": 1, "name": 1 }
```