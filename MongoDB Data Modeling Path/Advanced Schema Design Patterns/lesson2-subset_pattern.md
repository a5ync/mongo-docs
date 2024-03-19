# Code Summary: Subset Pattern

To apply the Subset pattern to the documents in our bookstore app, we ran the following aggregation pipeline:

```json
[
  {
    "$unwind": {
      "path": "$reviews"
    }
  },
  {
    "$set": {
      "reviews.product_id": "$product_id"
    }
  },
  {
    "$replaceRoot": {
      "newRoot": "$reviews"
    }
  },
  {
    "$out": "reviews"
  }
]
```

Next, we defined a clean up pipeline to ensure the reviews array in the book documents only includes three reviews:

```json
[
  {
    "reviews": {
      "$slice": ["$reviews", 3]
    }
  }
]
```

Finally, we ran this pipeline for the books collection with the updateMany method:

```js
db.books.updateMany({}, update_reviews_array_pipeline);
```
