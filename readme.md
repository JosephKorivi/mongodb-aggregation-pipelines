

# I am Joseph Korivi, here to discuss and perform the MongoDB advance aggregation pipelines concept

# we've 3 data sets authors, users, books 
# here are the set of questions and answers we're going to solve.

**here we need to know something called stages**
1. stage 1 is the starting stage
2. stage1 is the source to perform operations in the stage 2 

## some of the questions:

**1. How many active users are there in the data set?**
---
    ``` solution: [
        {
            $match: {
                isActive: true
            }
        }, 
        {
            count: 'activeUsers'
        }
    ]
    ```

---

**2. What is the average age of all users?**


```
    [
        {
            $group: {
            _id: "$gender",
                averageAge: {
                    $avg: "$age"
                }
            }
        }
    ]
```

---

**3. List the top 5 most common favorite fruits amongest users**

```
    [
        {
            $group: {
            _id: "$favoriteFruit",
                count: {
                    $sum: 1
                }
            }
        },
        {
            $sort: {
                count: -1
            }
        }, 
        {
            $limit: 5
        }
    ]
```

---

**4. find the total number of males and females**

```
    [
        {
            $group: {
            _id: "$gender",
                genderCount: {
                    $sum: 1
                }
            }
        }
    ]
```

---

**5. Which country has the highest number of registered users?**

```
    [
        {
            $group: {
            _id: "$company.location.country",
                userCount: {
                    $sum: 1
                }
            }
        }, 
        {
            $sort: {
                userCount: -1 
            }
        },
        {
            $limit: 5
        }
    ]
```

---

**6. List all unique eye colors present in the collection**

```
[
  {
    $group: {
      _id: "$eyeColor",
    }
  }
]

```

---

### whenever we're operating the data in the case of arrays, we first rip a part those array, spread it thorough, using ***$unwind***

**7. What is the average numbers of tags per user?**

```
    [
        {
            $unwind: {
            path: "$tags",
            }
        },
        {
            $group: {
            _id: "$_id",
                numberOfTags: {$sum: 1}
            }
        },
        {
            $group: {
            _id: null,
            averageNumberOfTags: {$avg: "$numberOfTags"}
            }
        }
    ]


    Method - II
    [
        {
            $addFields: {
                numberOfTags: {
                    $size: {$ifNull: ["$tags", []]}
                }
            }
        },
        {
            $group: {
                _id: null,
                averageNumberOfTags: {$avg: "$numberOfTags"}
            }
        }
    ]
```

---

**8. How many users have 'enim' as one of their age?**

```
[
  {
    $match: {
      tags: "enim",
    }
  },
  {
    $count: 'userWithEnimTag'
  }
]
```

---

**9. what are the names and age of the users who are inactive and have 'velit' as a tag?**

```
[
  {
    $match: {
      isActive: false,
      tags: "velit"
    }
  },
  {
    $project: {
      name: 1, age: 1
    }
  }
]
```

---

**10. How many users have phone number starting with (+1) 940?**


```

[
  {
    $match: {
      "company.phone": /^\+1 \(940\)/
    }
  },
  {
    $count: 'userswithSpecialPhoneNumber'
  }
]

```

---

**11. Who has Registered the most Recently?**

```
[
  {
    $sort: {
      registered: 1
    }
  }
]
```
---

**12. Categorize the user by favoriteFruit**

```
[
  {
    $sort: {
      registered: 1
    }
  },
  {
    $limit: 5
  }, 
  {
    $project: {
      name: 1,
      registered: 1,
      favoriteFruit: 1
    }
  }
]

```

--- 

**13. Categorize the user by their favoriteFruit**

```
[
  {
    $group: {
      _id: "$favoriteFruit",
      users: { $push: "$name" }
    }
  }
]
```