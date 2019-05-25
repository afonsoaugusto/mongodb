https://university.mongodb.com/mercury/M201

## Chapter_2

* Indexes
* MongoDb use B-tree

### Index Overhead
[Indexes](https://docs.mongodb.com/manual/indexes/?jmp=university)

### Data is Storad in Disk

* Use the different disks possibilite increase the performance

![Files Structure](../images/files-structure.png)
![Diferrents Structure](../images/Diferrents_Disks.png)

### Single Field Indexes

* Simplest indexes in MongoDB
* db.<collection>.createIndex({<field>:<direction>})
* Key Features
    - Keys from only one field
    - Can find a single value for the indexed field
    - Can find a range of values
    - Can use dot notation to index fields in subdocuments
    - Can be used to find several distinct values in a single query

* If field not exists in document, when created index this field is registred with value null.

* Not recommended create index on subdocument without dot notation.