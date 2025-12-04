# MongoDB Aggregation Pipelines (Complete Guide)

This README contains **all essential MongoDB aggregation pipelines**
for: - Joins (`$lookup`) - Grouping & stats (`$group`) - Pagination
(`$skip`, `$limit`) - Projection (`$project`) - Facets (`$facet`) -
Buckets (`$bucket`, `$bucketAuto`) - Sorting, filtering, and advanced
operations

------------------------------------------------------------------------

# 📌 Collections Example

## users

``` json
{
  "_id": ObjectId("U1"),
  "name": "John",
  "age": 28,
  "salary": 70000,
  "departmentId": ObjectId("D1")
}
```

## departments

``` json
{
  "_id": ObjectId("D1"),
  "name": "IT"
}
```

------------------------------------------------------------------------

# 🧩 FULL AGGREGATION PIPELINES

------------------------------------------------------------------------

# 1️⃣ Basic Lookup --- Join Users + Departments

``` js
db.users.aggregate([
  {
    $lookup: {
      from: "departments",
      localField: "departmentId",
      foreignField: "_id",
      as: "department"
    }
  }
]);
```

------------------------------------------------------------------------

# 2️⃣ Unwind Lookup Output

``` js
{ $unwind: "$department" }
```

------------------------------------------------------------------------

# 3️⃣ Grouping --- Users per Department + Count

``` js
db.users.aggregate([
  { $unwind: "$department" },
  {
    $group: {
      _id: "$department._id",
      departmentName: { $first: "$department.name" },
      totalUsers: { $sum: 1 }
    }
  }
]);
```

------------------------------------------------------------------------

# 4️⃣ Top Salary User per Department

``` js
db.users.aggregate([
  { $lookup: { from: "departments", localField: "departmentId", foreignField: "_id", as: "department" }},
  { $unwind: "$department" },
  { $sort: { salary: -1 }},
  {
    $group: {
      _id: "$department._id",
      departmentName: { $first: "$department.name" },
      topSalaryUser: { $first: "$$ROOT" }
    }
  }
]);
```

------------------------------------------------------------------------

# 5️⃣ Full Summary: Users, Count, Top Salary, User Info

``` js
db.users.aggregate([
  { $lookup: { from: "departments", localField: "departmentId", foreignField: "_id", as: "department"}},
  { $unwind: "$department"},
  {
    $group: {
      _id: "$department._id",
      departmentName: { $first: "$department.name" },
      users: { $push: "$$ROOT" },
      totalUsers: { $sum: 1 },
      topSalary: { $max: "$salary" }
    }
  },
  {
    $lookup: {
      from: "users",
      let: { deptId: "$_id", maxSal: "$topSalary" },
      pipeline: [
        {
          $match: {
            $expr: {
              $and: [
                { $eq: ["$departmentId", "$$deptId"] },
                { $eq: ["$salary", "$$maxSal"] }
              ]
            }
          }
        }
      ],
      as: "topSalaryUser"
    }
  }
]);
```

------------------------------------------------------------------------

# 6️⃣ Projection (`$project`)

``` js
db.users.aggregate([
  {
    $project: {
      _id: 0,
      name: 1,
      salary: 1,
      departmentId: 1,
      yearlySalary: { $multiply: ["$salary", 12] }
    }
  }
]);
```

------------------------------------------------------------------------

# 7️⃣ Pagination (`$skip`, `$limit`)

``` js
db.users.aggregate([
  { $sort: { name: 1 }},
  { $skip: 20 },   // page 3 with pageSize=10 => skip=2*10
  { $limit: 10 }
]);
```

Formula:

    skip = (page - 1) * pageSize

------------------------------------------------------------------------

# 8️⃣ Faceted Search (`$facet`) --- Pagination + Count in Single Query

``` js
db.users.aggregate([
  {
    $facet: {
      metadata: [{ $count: "total" }],
      data: [
        { $sort: { name: 1 }},
        { $skip: 0 },
        { $limit: 10 }
      ]
    }
  }
]);
```

Result:

``` json
{
  "metadata": [{ "total": 57 }],
  "data": [ { ...users } ]
}
```

------------------------------------------------------------------------

# 9️⃣ Bucket --- Group Users by Salary Range

``` js
db.users.aggregate([
  {
    $bucket: {
      groupBy: "$salary",
      boundaries: [0, 30000, 60000, 90000, 120000],
      default: "Above 120k",
      output: {
        userCount: { $sum: 1 },
        users: { $push: "$name" }
      }
    }
  }
]);
```

------------------------------------------------------------------------

# 🔟 Auto Bucket (`$bucketAuto`) --- Automatic Salary Bucketing

``` js
db.users.aggregate([
  {
    $bucketAuto: {
      groupBy: "$salary",
      buckets: 4
    }
  }
]);
```

------------------------------------------------------------------------

# 1️⃣1️⃣ Group By Age Range Example

``` js
db.users.aggregate([
  {
    $bucket: {
      groupBy: "$age",
      boundaries: [18, 25, 35, 45, 60],
      default: "60+",
      output: {
        count: { $sum: 1 },
        avgSalary: { $avg: "$salary" }
      }
    }
  }
]);
```

------------------------------------------------------------------------

# 1️⃣2️⃣ Sort + Project + Limit (Clean Output)

``` js
db.users.aggregate([
  { $sort: { salary: -1 }},
  { $project: { name: 1, salary: 1, _id: 0 }},
  { $limit: 5 }
]);
```

------------------------------------------------------------------------

# 1️⃣3️⃣ Departments With Zero Users (Left Join)

``` js
db.departments.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "_id",
      foreignField: "departmentId",
      as: "users"
    }
  }
]);
```

------------------------------------------------------------------------

# 1️⃣4️⃣ Full Summary With Bucketing + Facets + Pagination (Advance)

``` js
db.users.aggregate([
  {
    $facet: {
      salaryBuckets: [
        {
          $bucket: {
            groupBy: "$salary",
            boundaries: [0, 30000, 60000, 90000, 120000],
            default: "120k+",
            output: { count: { $sum: 1 }, users: { $push: "$name" }}
          }
        }
      ],
      paginatedUsers: [
        { $sort: { salary: -1 }},
        { $skip: 0 },
        { $limit: 5 },
        { $project: { name: 1, salary: 1 }}
      ],
      metadata: [
        { $count: "totalUsers" }
      ]
    }
  }
]);
```

------------------------------------------------------------------------

# ✅ Summary of All Pipelines Included

✔ Lookup\
✔ Unwind\
✔ Group\
✔ Count\
✔ Top salary\
✔ Complex joins\
✔ Projection\
✔ Pagination\
✔ Facets\
✔ Buckets\
✔ Auto buckets\
✔ Summary analytics

------------------------------------------------------------------------

You now have a **complete MongoDB Aggregation Handbook** 🚀
