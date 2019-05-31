https://university.mongodb.com/mercury/M201

# Final Exam

## Question 1

Which of these statements is/are true?

* You can index multiple array fields in a single document with a single compound index.
* __Creating an ascending index on a monotonically increasing value creates index keys on the right-hand side of the index tree.__
* Write concern has no impact on write latency.
* A collection scan has a logarithmic search time.
* Covered queries can sometimes still require some of your documents to be examined.

## Question 2

Which of the following statements is/are true?

* __Indexes can decrease insert throughput.__
* __Indexes can be walked backwards by inverting their keys in a sort predicate.__
* __It's important to ensure that secondaries with indexes that differ from the primary not be eligible to become primary.__
* __It's important to ensure that your shard key has high cardinality.__
* __Partial indexes can be used to reduce the size requirements of the indexes.__

## Question 3

Which of the following statements is/are true?

* MongoDB indexes are markov trees.
* __It's common practice to co-locate your mongos on the same machine as your application to reduce latency.__
* Background index builds block all reads and writes to the database that holds the collection being indexed.
* __Collations can be used to create case insensitive indexes.__
* __By default, all MongoDB user-created collections have an _id index.__

## Question 4

Which of the following statements is/are true?

* __Indexes can solve the problem of slow queries.__
* When you index on a field that is an array it creates a partial index.
* Under heavy write load you should scale your read throughput by reading from secondaries.
* On a sharded cluster, aggregation queries using $lookup will require a merge stage on a random shard.
* __Indexes are fast to search because they're ordered such that you can find target values with few comparisons.__

## Question 5

Which of the following statements is/are true?

* Compound indexes can service queries that filter on any subset of the index keys.
* __Query plans are removed from the plan cache on index creation, destruction, or server restart.__
* __If no indexes can be used then a collection scan will be necessary.__
* By default, the explain() command will execute your query.
* __Compound indexes can service queries that filter on a prefix of the index keys.__

## Question 6

Which of the following statements is/are true?

* __An index doesn't become multikey until a document is inserted that has an array value.__
* Indexes can only be traversed forward.
* __You can use the --wiredTigerDirectoryForIndexes option to place your indexes on a different disk than your data.__
* __The ideal ratio between nReturned and totalKeysExamined is 1.__
* Running performance tests from the mongo shell is an acceptable way to benchmark your database.

## Question 7

Given the following indexes:

```js
{ categories: 1, price: 1 }
{ in_stock: 1, price: 1, name: 1 }
```

The following documents:

```js
{ price: 2.99, name: "Soap", in_stock: true, categories: ['Beauty', 'Personal Care'] }
{ price: 7.99, name: "Knife", in_stock: false, categories: ['Outdoors'] }
```

And the following queries:

```js
db.products.find({ in_stock: true, price: { $gt: 1, $lt: 5 } }).sort({ name: 1 })
db.products.find({ in_stock: true })
db.products.find({ categories: 'Beauty' }).sort({ price: 1 })
```

Which of the following is/are true?

* There would be a total of 4 index keys created across all of these documents and indexes.

```js
No, there would be 5 total index keys:

{ categories: 'Beauty', price: 2.99 }
{ categories: 'Personal Care', price: 2.99 }
{ categories: 'Outdoors', price: 7.99 }
{ in_stock: true, price: 2.99, name: 'Soap' }
{ in_stock: false, price: 7.99, name: 'Knife'}
The additional index keys are due to the multikey index on categories.
```

* __Index #2 can be used by both query #1 and #2.__
* __Index #1 would provide a sort to query #3.__
* Index #2 properly uses the equality, sort, range rule for query #1.