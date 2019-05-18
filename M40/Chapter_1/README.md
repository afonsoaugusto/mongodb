# Chapter_1: Replica Set Transactions

* ACID Data Guarantees
    - Complete isolation
    - Consistent view of data
* Multi  document trasactions

## Considerations:

* Per trasaction
 - No more 1.000 documents modification
    - Pressure WiredTiger Cache
 - Time limit 60 seconds
    - Trastions use the same snapshot thoughout the duration of  the trasaction
 - No limit read document

* DDL Operations lock the collection for the other locks
 - Create index
 - Drop collection

## Write Conflicts

* Loop infinit with Backoff 
* Reads can still occur while a write lock is taken.

[MongoDB Multi-Document ACID Transactions are GA](https://www.mongodb.com/blog/post/mongodb-multi-document-acid-transactions-general-availability)

## Transient transaction errors

The cases where TransientTransactionError can occur are related with:

* Network failures
Network failures may be related with server-client communication failure or abnormal issues with the network layer.

* WriteConflict errors
WriteConflict will be detected when two concurrent transactions try to write to the same document, where one of those transactions acquires the document lock and subsequent requests from other transactions will result in WriteConflict errors raised.

```js
def run_transaction_and_retry_commit(client):
  with client.start_session() as s:
          s.start_transaction()
          collection_one.insert_one(doc1, session=s)
          collection_two.insert_one(doc2, session=s)
          while True:
              try:
                  s.commit_transaction()
                  break
              except (OperationFailure, ConnectionFailure) as exc:
                  if exc.has_error_label("UnknownTransactionCommitResult"):
                      print("Unknown commit result, retrying...")
                      continue
                  raise

while True:
  try:
      return run_transaction_and_retry_commit(client)
  except (OperationFailure, ConnectionFailure) as exc:
      if exc.has_error_label("TransientTransactionError"):
          print("Transient transaction error, retrying...")
          continue
      raise
```

## Production considerations

[Note: Production Consideration](https://docs.mongodb.com/manual/core/transactions-production-consideration/)

## Exercise

```sh
wget https://s3.amazonaws.com/edu-downloads.10gen.com/M040_2019_May/static/handouts/m040/m040-vagrant-env.zip
unzip m040-vagrant-env.zip

wget https://s3.amazonaws.com/edu-downloads.10gen.com/M040_2019_May/static/handouts/m040/environment_setup.zip
mkdir shared
unzip -d environment_setup environment_setup.zip
cd environment_setup
cp rs*linux.conf   ../shared
cp validate_lab1.js ../shared
cd ../shared

wget https://s3.amazonaws.com/edu-downloads.10gen.com/M040_2019_May/static/handouts/m040/error_handling_lab.zip
unzip error_handling_lab.zip

cd ..
vagrant up
vagrant ssh


mkdir -p /data/db/m040/repl/{1,2,3}

# start mongod
mongod -f /shared/rs1_linux.conf
mongod -f /shared/rs2_linux.conf
mongod -f /shared/rs3_linux.conf

# Initiate  Replica Set
mongo --eval 'rs.initiate()'

mongo --eval "rs.add('$(hostname):27027');rs.add('$(hostname):27037')"
mongo --eval 'rs.status()'
mongo --quiet /shared/validate_lab1.js
```

```
python3  /shared/loader.py --uri mongodb://m040:27017/m040?replicaSet=M040 --file /shared/data.json

vagrant@m040:~$ python3  /shared/loader.py --uri mongodb://m040:27017/m040?replicaSet=M040 --file /shared/data.json
Error - what shall I do ??!??! WriteConflict
Unexpected error found: WriteConflict
Duplicate Key Found: E11000 duplicate key error collection: m040.cities index: _id_ dup key: { : 81 }
Duplicate Key Found: E11000 duplicate key error collection: m040.cities index: _id_ dup key: { : 41 }
Duplicate Key Found: E11000 duplicate key error collection: m040.cities index: _id_ dup key: { : 90 }
Documents Inserted: 60
Total Population: 1062658071


vagrant destroy
```