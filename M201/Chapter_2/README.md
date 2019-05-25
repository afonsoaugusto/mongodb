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

### Querying on Compound Indexes Part 1

```
wget https://s3.amazonaws.com/m312/people.json

mongoimport --host Sandbox-shard-0/sandbox-shard-00-00-felrd.mongodb.net:27017,sandbox-shard-00-01-felrd.mongodb.net:27017,sandbox-shard-00-02-felrd.mongodb.net:27017 \
            --ssl --username m001-student \
            --password <PASSWORD> \
            --authenticationDatabase admin \
            --db m201 \
            --collection people \
            --type json \
            --file people.json
```

```
{"last_name": "Frazier","frist_name":"Jasmine"}
```
