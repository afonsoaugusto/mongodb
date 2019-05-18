https://university.mongodb.com/mercury/M040/

## Chapter_0
* ACID trasactions
* Pipeline Builder (query)

### Exercise
```
wget https://s3.amazonaws.com/edu-downloads.10gen.com/M040_2019_May/static/handouts/m040/m040-vagrant-env.zip
unzip  m040-vagrant-env.zip

wget https://s3.amazonaws.com/edu-downloads.10gen.com/M040_2019_May/static/handouts/m040/environment_setup.zip
mkdir shared
unzip -d environment_setup environment_setup.zip
cd environment_setup
cp rs*linux.conf   ../shared
cp validate_lab1.js ../shared
cd ../shared

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
vagrant destroy
```