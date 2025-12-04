# MongoDB Aggregation Pipelines for Users & Departments

This README contains multiple MongoDB aggregation pipelines for a common
schema where:

-   **users** collection stores user details with a `departmentId`
-   **departments** collection stores department details

------------------------------------------------------------------------

# 📌 Collections Example

## users

``` json
{
  "_id": ObjectId("U1"),
  "name": "John",
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

# 📘 Aggregation Pipelines

------------------------------------------------------------------------

## 1️⃣ Get all users grouped by department (with count)

``` js
db.users.aggregate([
  {
    $lookup: {
      from: "departments",
      localField: "departmentId",
      foreignField: "_id",
      as: "department"
    }
  },
  { $unwind: "$department" },
  {
    $group: {
      _id: "$department._id",
      departmentName: { $first: "$department.name" },
      users: { $push: "$$ROOT" },
      totalUsers: { $sum: 1 }
    }
  }
]);
```

------------------------------------------------------------------------

## 2️⃣ Get highest salary user from each department

``` js
db.users.aggregate([
  {
    $lookup: {
      from: "departments",
      localField: "departmentId",
      foreignField: "_id",
      as: "department"
    }
  },
  { $unwind: "$department" },
  { $sort: { salary: -1 } },
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

## 3️⃣ Get users, count, top salary and highest salary user

``` js
db.users.aggregate([
  {
    $lookup: {
      from: "departments",
      localField: "departmentId",
      foreignField: "_id",
      as: "department"
    }
  },
  { $unwind: "$department" },
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

## 4️⃣ Get departments with 0 users (LEFT JOIN)

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

## 5️⃣ Count of users per department (clean output)

``` js
db.users.aggregate([
  { $group: { _id: "$departmentId", totalUsers: { $sum: 1 } } }
]);
```

------------------------------------------------------------------------

## 6️⃣ Get average, max & min salary per department

``` js
db.users.aggregate([
  {
    $group: {
      _id: "$departmentId",
      avgSalary: { $avg: "$salary" },
      maxSalary: { $max: "$salary" },
      minSalary: { $min: "$salary" }
    }
  }
]);
```

------------------------------------------------------------------------

## 7️⃣ Department summary (clean format)

``` js
db.users.aggregate([
  {
    $lookup: {
      from: "departments",
      localField: "departmentId",
      foreignField: "_id",
      as: "dept"
    }
  },
  { $unwind: "$dept" },
  {
    $group: {
      _id: "$dept._id",
      department: { $first: "$dept.name" },
      totalUsers: { $sum: 1 },
      maxSalary: { $max: "$salary" },
      users: { $push: "$name" }
    }
  }
]);
```

------------------------------------------------------------------------

# ✅ Summary

This README includes:

-   User → Department join pipelines\
-   Grouping & counting\
-   Highest salary logic\
-   Full summary pipelines\
-   Clean output formats

You can extend these pipelines easily for any analytics use case.
