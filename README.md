# Claymore MongoDB Exercises
In this project I want to use MongoDB commands to query the collection. There will be a init script that reverts the database in a starting version, so if any command is destructive there will be a script to start over. Also, the final script with the final structure of the database will be present.

For more information about the characters and data used here, take a look at [Claymore Wiki](https://claymore.fandom.com/wiki/Claymore)

Also, these commands are executed in the MongoDB Playground extension for VS Code.

## Exercises

### Find all Claymores with no epitaph
`db.claymore.find({ epitaph: "None" });`

### Find all Claymores with no symbol
`db.claymore.find({ symbol: { $exists: false} });`

### Find all Claymores killed in the Northern Campaign
`db.claymore.find({ generation:"Clare" , fate: { $eq:"Dead (Killed in the Northern Campaign)"} } );`

`db.claymore.find({ fate: { $regex: ".*Killed in the Northern Campaign.*" } } );`

### Find all Dead/Awakened Claymores
`db.claymore.find({ fate: { $regex: ".*Dead.*Awakened" } } );`

### Order by rank
`db.claymore.find({}).sort({rank:1});`

### Count all Claymores by generation
```
const aggregation = [
  { $group: { 
    _id : "$generation", 
    count: { $sum: 1 } } }
];

db.claymore.aggregate(aggregation);
```
Result
```
[
  {
    "_id": "Clare",
    "count": 29
  },
  {
    "_id": "Teresa",
    "count": 6
  },
  {
    "_id": "Clarice",
    "count": 21
  }
]
```

### Find the average rank by generation
```
const aggregation = [
  { $group: { 
    _id : "$generation", 
    avgOfRanks: { $avg: "$rank" } } }
];

db.claymore.aggregate(aggregation);
```

Result
```
[
  {
    "_id": "Clare",
    "avgOfRanks": 23.379310344827587
  },
  {
    "_id": "Teresa",
    "avgOfRanks": 2.8333333333333335
  },
  {
    "_id": "Clarice",
    "avgOfRanks": 12.666666666666666
  }
]
```

### Find the sum of ranks in all generations
```
const aggregation = [
  { $group: { 
    _id : null, 
    sumOfRanks: { $sum: "$rank" } } }
];

db.claymore.aggregate(aggregation);
```

Result
```
[
  {
    "_id": null,
    "sumOfRanks": 961
  }
]
```

### Find all offensive Claymores in each generation - NOT CORRECT
```
const aggregation = [
  { $match: { type: "Offensive" } },
  { $group: { _id : "$generation", count: { $sum:1 } } }
];

db.claymore.aggregate(aggregation);
```
Result
```
[
  {
    "_id": "Clare",
    "count": 8
  },
  {
    "_id": "Teresa",
    "count": 6
  },
  {
    "_id": "Clarice",
    "count": 3
  }
]
```

### Find the distinct generations for all Claymores
`db.claymore.distinct("generation");`
Result
```
[
  "Clare",
  "Clarice",
  "Teresa"
]
```

### Convert generation field to String array
Update documents by generation
```
db.claymore.updateMany(
  { generation: "Clare" },
  { $set: { generation: [ "Clare" ] } }
);
```
```
db.claymore.updateMany(
  { generation: "Clarice" },
  { $set: { generation: [ "Clarice" ] } }
);
```
```
db.claymore.updateMany(
  { generation: "Teresa" },
  { $set: { generation: [ "Teresa" ] } }
);
```
Set Alicia and Beth to both Clare and Clarice generations
```
db.claymore.updateOne(
  { _id: "beth" },
  { $set: { generation: [ "Clare", "Clarice" ] } }
);
```
```
db.claymore.updateOne(
  { _id: "alicia" },
  { $set: { generation: [ "Clare", "Clarice" ] } }
);
```

### Remove fields with None/Unknown values (epitaph, type, techniques)
Remove **epitaph** field when is Unknown
```
db.claymore.updateMany(
  { epitaph: "None" },
  { $unset: { epitaph: "" } }
);
```
Remove **techniques** field when contains Unknown
```
db.claymore.updateMany(
  { techniques: "Unknown" },
  { $unset: { techniques: "" } }
);
```
Remove **type** field when is Unknown
```
db.claymore.updateMany(
  { type: "Unknown" },
  { $unset: { type: "" } }
);
```
