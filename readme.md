

# I am Joseph Korivi, here to discuss and perform the MongoDB advance aggregation pipelines concept

# we've 3 data sets authors, users, books 
# here are the set of questions and answers we're going to solve.

**here we need to know something called stages**
1. stage 1 is the starting stage
2. stage1 is the source to perform operations in the stage 2 

## some of the questions:

***1. How many active users are there in the data set?***
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