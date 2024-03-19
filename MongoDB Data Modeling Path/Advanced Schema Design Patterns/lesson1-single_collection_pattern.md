# Code Summary: Single Collection Pattern

## Variant One

To apply the first variant of the Single Collection pattern to the books catalog, we added the `docType` and `relatedTo` fields to each document for the Books, Users and Reviews collections and moved the documents to the `Books Catalog` collection.
For example:

```js
{
   "review_id": 378,
  "docType": "review",
   "relatedTo": [202356]
   ...
}
```

Next, we created an index on the `relatedTo` array to support queries for related documents:

```js
db.books_catalog.createIndex({ relatedTo: 1 });
```

## Variant Two

To apply the second variant of the Single Collection pattern to the books catalog in our bookstore app, we added an overloaded `sc_id` field to our documents. Book documents used the book id in the `sc_id field`. For review documents the `sc_id` field contains the book id and the review id separated by a slash. This way we overloaded the field in order to support queries for related documents. Finally we moved the documents to the `Books Catalog` collection.

Book document example:

```json
{
   "book_id": 202356,
   "sc_id": "202356"
   ...
}
```

Review document example:

```json
{
   "review_id": 378,
   "sc_id": "202356/378"
   ...
}
```

Next, we created an index on the sc_id field to support queries for related documents:

```js
db.books_catalog.createIndex({ sc_id: 1 });
```
