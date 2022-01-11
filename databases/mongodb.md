# MongoDb


```jsx
# 批量更新

db.user.find().forEach(function(doc) {
    db.user.update(
       { _id: doc._id},
       { $set : { 'address' : doc.owner_address.toLowerCase() } },
       { multi: true }
    )
});

db.planet.find().forEach(function(doc) {
    db.planet.update(
       { _id: doc._id},
       { $set : { 'is_election' : 0 } },
       { multi: true }
    )
});

# 求和

db.profile.aggregate(
   [
     {
       $group:
         {
           _id: "_id",
           totalAmount: { $sum: {$toInt:"$amount"} },
           count: { $sum: 1 }
         }
     }
   ]
)

db.profile.find().forEach(function(doc) {
    db.profile.update(
       { _id: doc._id},
       { $set : { 'amount' : NumberDecimal(doc.amount) } },
       { multi: true }
    )
});

```

# 数据备份/恢复

### mongodump && mongorestore

```bash
mongodump -h host:port -d db_source -c collection_source -o dump/collection_source.bson
mongorestore -h host:port -d db_target -c collection_target dump/collection_source.bson

```

### mongoexport && mongoimport

```bash
mongoexport -h [ip_address] -d [database] -c [collection] > source.json
mongoimport -h [ip_address] -d [database] -c [target] source.json

mongoexport --collection=events --db=reporting --out=events.json

```

### 3.2 之后的版本, 支持管道，即时处理

```bash
mongoexport -h [ip_address] -d [database] -c [collection] | mongoimport -h [ip_address] -d [database] -c [target]

```

### 数据少，也可以用命令行

```jsx
use db_source;
var docs = db_source.collection_souce.find();
use db_target;
docs.forEach(function(d){db.collection_target.insert(d)});

```

或

```jsx
db.source.find().forEach(function(doc) {
  db.target.insert(doc);
});

```

## 参考链接

[https://segmentfault.com/q/1010000004949649](https://segmentfault.com/q/1010000004949649)

[https://docs.mongodb.com/database-tools/mongodump/](https://docs.mongodb.com/database-tools/mongodump/)

[https://docs.mongodb.com/database-tools/mongorestore/](https://docs.mongodb.com/database-tools/mongorestore/)

[https://docs.mongodb.com/database-tools/mongoexport/](https://docs.mongodb.com/database-tools/mongoexport/)

[https://docs.mongodb.com/database-tools/mongoimport/](https://docs.mongodb.com/database-tools/mongoimport/)

```bash
db.member.update(
    {"is_auth" : {$exists : false}},
    {"$set" : {"is_auth" : 0}},
    false,
    true
)

新增字段
```

```jsx

db.config.find({config_type:"sync_land"}).forEach(function(doc) {
    db.config.update(
       { _id: doc._id},
       { $set : { 'update_block' : 0 } },
       { multi: true }
    )
});

db.land.find({owner:{$ne: ""}}).forEach(function(doc) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 3 } },
       { multi: true }
    )
});

db.land.find({x:{$gt: 103}, y:{$gt: 103}}).forEach(function(doc) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 4 } },
       { multi: true }
    )
});

db.land.find({x:{$gte: 103, $lte: 256}, y:{$gte: 103, $lte: 256}}).forEach(function(doc) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 4 } },
       { multi: true }
    )
});

db.land.find({x:{$gte: 175, $lte: 185}, y:{$gte: 175, $lte: 184}}).forEach(function(doc) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 1 } },
       { multi: true }
    )
});

db.land.find().forEach(function(doc) {
   if(doc.x == doc.y) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 1 } },
       { multi: true }
    )
   }
   if(doc.x + 1 == doc.y) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 1 } },
       { multi: true }
    )
   }
   if(doc.x - 1 == doc.y) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 1 } },
       { multi: true }
    )
   }
   if(165 <= doc.x && doc.x <= 194 && 165 <= doc.y && doc.y <= 194) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 1 } },
       { multi: true }
    )
   }
   if(103 <= doc.x && doc.x <= 175 && 103 <= doc.y && doc.y <= 175) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 2 } },
       { multi: true }
    )
   }
})

    def isAllowed(self, x, y) :
        if (x == y or x + 1 == y or x - 1 == y):
            return False # 不在销售范围
        if (165 <= x and x <= 194 and 165 <= y and y <= 194):
            return False # 不在销售范围
        if (103 <= x and x <= 175 and 103 <= y and y <= 175):
            return False # 预留区域
        if (103 <= x and x <= 256 and 103 <= y and y <= 256):
            return True
        return False

class LandType(Enum):
    NOT_ON_SALE = 1 # 不在销售范围
    PRE_SALE = 2  # 销售预留区域
    HAS_OWNER = 3, # 有 owner
    AVAILABLE = 4, # 可以购买
    MINE = 5, # 我的地块信息

```

```jsx
db.land.find().forEach(function(doc) {
   if(doc.owner != "") {
      db.land.update(
          { _id: doc._id},
          { $set : { 'type' : 3 } },
          { multi: true }
       )
   }
   else if(doc.x == doc.y) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 1 } },
       { multi: true }
    )
   }
   else if(doc.x + 1 == doc.y) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 1 } },
       { multi: true }
    )
   }
   else if(doc.x - 1 == doc.y) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 1 } },
       { multi: true }
    )
   }
   else if(165 <= doc.x && doc.x <= 194 && 165 <= doc.y && doc.y <= 194) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 1 } },
       { multi: true }
    )
   else if(103 <= doc.x && doc.x <= 175 && 103 <= doc.y && doc.y <= 175) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 3 } },
       { multi: true }
    )
   else if(103 <= doc.x && doc.x <= 256 && 103 <= doc.y && doc.y <= 256) {
    db.land.update(
       { _id: doc._id},
       { $set : { 'type' : 4 } },
       { multi: true }
    )
   }
})

```