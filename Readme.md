# MongoDB Task

- Design a mongodb database for Zen class programme.

## Table of Contents

| S. No | Topics                        |
| ----- | ----------------------------- |
| 1.    | [Task Explain](#task-explain) |
| 2.    | [Questions](#questions)       |
| 3.    | [Answers](#answers)           |

## Task Explain

- to design a database for zen class programme using mongodb with the following collections
  1. users
  2. codekata
  3. attendance
  4. topics
  5. tasks
  6. company_drives
  7. mentors
- to answer the following [questions](#questions)

## Questions

1. [Find all the topics and tasks which are thought in the month of October](#1)
2. [Find all the company drives which appeared between 15 oct-2020 and 31-oct-2020](#2)
3. [Find all the company drives and students who are appeared for the placement](#3)
4. [Find the number of problems solved by the user in codekata](#4)
5. [Find all the mentors with who has the mentee's count more than 15](#5)
6. [Find the number of users who are absent and task is not submitted between 15 oct-2020 and 31-oct-2020](#6)

## Answers

### 1

- Topics taught in October

```js
db.topics.find({
  date: { $gte: "2023-10-01", $lte: "2023-10-31" },
});
```

- Tasks assigned in October

```js
db.tasks.find({
  date: { $gte: "2023-10-01", $lte: "2023-10-31" },
});
```

[Bact to Questions](#questions)

### 2

```js
db.companyDrives.find({
  date: { $gte: "2020-10-15", $lte: "2020-10-31" },
});
```

[Bact to Questions](#questions)

### 3

```js
db.company_drives.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "eligible_batches",
      foreignField: "batch",
      as: "students",
    },
  },
]);
```

[Bact to Questions](#questions)

### 4

```js
db.codekata.aggregate([
  {
    $group: {
      _id: "$user_id",
      total_problems_solved: { $sum: "$problems_solved" },
    },
  },
]);
```

[Bact to Questions](#questions)

### 5

```js
db.mentors.find({
  students_assigned: { $gt: 15 },
});
```

[Bact to Questions](#questions)

### 6

```js
db.attendance.aggregate([
  {
    $match: {
      date: { $gte: "2020-10-15", $lte: "2020-10-31" },
      status: "Absent",
    },
  },
  {
    $lookup: {
      from: "tasks",
      localField: "user_id",
      foreignField: "user_id",
      as: "taskDetails",
    },
  },
  {
    $match: { "taskDetails.status": "Pending" },
  },
  {
    $count: "absent_and_pending_tasks",
  },
]);
```

[Bact to Questions](#questions)
