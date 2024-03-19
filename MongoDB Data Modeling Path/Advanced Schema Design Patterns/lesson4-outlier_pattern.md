# Code Summary: Outlier Pattern

To apply the Outlier pattern to the review documents in our bookstore app, we added the `outlier` field to the documents in the reviews collection. This field is set to true when the comments array in a review document exceeds the three comments threshold:

```json
{
  "rating": 1,
  "userId": 318,
  "productId": 34538756,
  "review_text": "Lorem ipsum dolor sit amet...",
  "comments": [{...}, {...}, ... ],
  "outlier": true,
  ...
}
```

To update the existing documents in the reviews collection we used the `updateMany` method with two arguments. First a filter to match all documents with a comments array larger than 3 elements. The second argument is an aggregation pipeline with $set operator to add the outlier field and set it to true.

```js
db.reviews.updateMany(
  {
    "comments.3": { $exists: true },
  },
  {
    $set: { outlier: true },
  }
);
```
