# Claymore MongoDB Exercises
In this project I want to use MongoDB commands to query the collection. There will be a init script that reverts the database in a starting version, so if any command is destructive there will be a script to start over. Also, the final script with the final structure of the database will be present.

For more information about the characters and data used here, take a look at [Claymore Wiki](https://claymore.fandom.com/wiki/Claymore)

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

### Find the average rank by generation

### Find the sum of ranks in all generations

### Find all offensive Claymores in each generation

### Convert generation field to String array
set Alicia and Beth to both Clare and Clarice generations

### Remove fields with None/Unknown values (epitaph, type, techniques)
