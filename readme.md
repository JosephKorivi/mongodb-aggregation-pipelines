

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