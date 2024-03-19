# Code Summary: Bucket Pattern

To apply the Bucket pattern to the documents in our bookstore app, we created a bucket document with an id containing the `book_id` and `month` timestamp:

```json
{
  "id": {
    "book_id": 1,
    "month": {
      "$date": "2023-11-22T00:00:00.000Z"
    }
  }
}
```

Next, we added the `views` array to the bucket document to store all view documents for the month. Each view document included a `timestamp` and the `user_id`:

```json
{
  "id": {
    "book_id": 1,
    "month": {
      "$date": "2023-11-22T00:00:00.000Z"
    }
  },
  "views": [
    {
      "user_id": "user00",
      "timestamp": {
        "$date": "2023-11-22T00:00:00.000Z"
      }
    },
    {
      "user_id": "user10",
      "timestamp": {
        "$date": "2023-11-22T00:01:00.000Z"
      }
    }
  ]
}
```

Although not covered in the previous video, you can also use an aggregation pipeline to implement the Bucket pattern on an existing views collection. For example, imagine you had a views collection where each document corresponds to a user view for a book and consists of a `book_id`, `timestamp` and `user_id`.

```json
{
  "book_id": 34538756,
  "timestamp": {
    "$date": "2023-09-29T08:23:13Z"
  },
  "user_id": 271828
}
```

In order to retrieve bucket documents containing all `view` documents of a given book grouped by month and including the total view count, you could use the following aggregation pipeline:

```json
[
  {
    "$group": {
      "_id": {
        "book_id": "$book_id",
        "month": {
          "$dateFromParts": {
            "year": { "$year": "$timestamp" },
            "month": { "$month": "$timestamp" },
            "day": 1
          }
        }
      },
      "views": {
        "$push": {
          "user_id": "$user_id",
          "timestamp": "$timestamp"
        }
      }
    }
  },
  {
    "$set": {
      "views_count": { "$size": "$views" }
    }
  }
]
```
