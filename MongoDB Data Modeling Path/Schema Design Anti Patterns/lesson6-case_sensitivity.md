# Code Summary: Case-Sensitivity

To create a case-insensitive index on the `author` field for an existing collection, we used the following:

```js
db.Books.createIndex(
  { author: 1 },
  {
    collation: {
      locale: "en",
      strength: 2,
    },
  }
);
```

Now to run a case-insensitive query, we ran the the following query with the same collation:

```js
db.Books.find({ author: "Paul Done" }).collation({ locale: "en", strength: 2 });
```

To check the existing indexes for a collection we used the `.getIndexes()` command:

```js
db.Books.getIndexes();
```

To drop an unused existing index on the author field missing the required collation. We used the `.dropIndex()` command. Always make sure your indexes are not necessary before dropping them.

```js
db.Books.dropIndex({ author: 1 });
```
