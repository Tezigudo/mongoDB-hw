## Create MongoDB database with following information.



create a docker container with this command

```sh
docker run --name "HwNOSQL" -e MONGO_INITDB_ROOT_USERNAME=myAdmin -e MONGO_INITDB_ROOT_PASSWORD=<<ENTER PASSWORD>>-d mongo
```

connect to the container

```sh
docker exec -it HwNOSQL bash
```

Authenticate to the container

```sh
mongosh -u myAdmin --authenticationDatabase admin –p
```

run mongo interactive shell
```sh
mongosh
```

Create new database with name `Student_Performance`

```
use Student_Performance
```

Insert data to the database

```mongo
db.students.insertMany([
    {"name":"Ramesh","subject":"maths","marks":87},
    {"name":"Ramesh","subject":"english","marks":59},
    {"name":"Ramesh","subject":"science","marks":77},
    {"name":"Rav","subject":"maths","marks":62},
    {"name":"Rav","subject":"english","marks":83},
    {"name":"Rav","subject":"science","marks":71},
    {"name":"Alison","subject":"maths","marks":84},
    {"name":"Alison","subject":"english","marks":82},
    {"name":"Alison","subject":"science","marks":86},
    {"name":"Steve","subject":"maths","marks":81},
    {"name":"Steve","subject":"english","marks":89},
    {"name":"Steve","subject":"science","marks":77},
    {"name":"Jan","subject":"english","marks":0,"reason":"absent"}
])
```

- find total mark for each student across all subjects
```mongo
db.students.aggregate([
    {
        $group: {
            _id: "$name",
            totalMarks: { $sum: "$marks" }
        }
    }
])
```
here a result

<img width="815" alt="image" src="https://user-images.githubusercontent.com/80502090/229642337-f5b3fc5c-dc78-46e8-b614-cfdac526cdd2.png">



  - find the maximum mark scored in each subject
```mongo
db.students.aggregate([
    {
        $group: {
            _id: "$subject",
            maxMarks: { $max: "$marks" }
        }
    }
])
```

- find the minimum mark scored by each student
```mongo
db.students.aggregate([
    {
        $group: {
            _id: "$name",
            minMarks: { $min: "$marks" }
        }
    }
])
```
<img width="360" alt="image" src="https://user-images.githubusercontent.com/80502090/229643587-f7e79cb1-837a-460f-99b6-d2ef3569672a.png">


- find the top two subjets based on average marks
```mongo
db.students.aggregate([
    {
        $group: {
            _id: "$subject",
            avgMarks: { $avg: "$marks" }
        }
    },
    {
        $sort: { avgMarks: -1 }
    },
    {
        $limit: 2
    }
])
```
<img width="351" alt="image" src="https://user-images.githubusercontent.com/80502090/229643784-174c5749-1087-4a2b-a2e1-f2182e95eaf1.png">
