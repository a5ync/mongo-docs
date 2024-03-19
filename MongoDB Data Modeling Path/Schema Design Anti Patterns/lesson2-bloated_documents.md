# Code Summary: Bloated Documents

To retrieve the number of documents in a collection using the `stats()` method in `mongosh`, use the following:

```js
db.collection.stats().count;
```

To retrieve the average size of documents in a collection using the `stats()` method in `mongosh`, use the following:

```js
db.collection.stats().avgObjSize;
```
