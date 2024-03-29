# Code Summary: Schema Validation

To create a new "sales" collection schema validation, we use the `createCollection` method with the validator option. In this example, we combine a query operator, `$and`, and JSON schema to define the schema validation rules. `$and` checks the `discounterPrice` is lower than the item `price` for a given item and `$jsonSchema` ensures the items field is an array.

```js
db.createCollection("sales", {
  validator: {
    $and: [
      {
        $expr: {
          $lt: ["$items.discountedPrice", "$items.price"],
        },
      },
      {
        $jsonSchema: {
          properties: {
            items: { bsonType: "array" },
          },
        },
      },
    ],
  },
});
```

To find all the documents that do not match a validator expression use the following:

```js
db.collection.find({ $nor: [validator] });
```

This is the reviews validator used for our bookstore:

```js
const bookstore_reviews_default = {
    bsonType: "object",
    required: [“_id”, "review_id", "user_id", "timestamp", "review", "rating"],
    additionalProperties: false,
    properties: {
        _id: { bsonType: "objectId" },
        review_id: { bsonType: "string" },
        user_id: { bsonType: "string" },
        timestamp: { bsonType: "date" },
        review: { bsonType: "string" },
        rating: {
            bsonType: "int",
            minimum: 0,
            maximum: 5,
        },
        comments: {
            bsonType: "array",
            maxItems: 3,
            items: {
                bsonType: "object",
            },
        },
    },
};
```

To enable or update schema validation for an existing collection use the `collMod` command providing the validator option. In our example, we used the `$jsonSchema` operator to build the validator. We also overrided the default validation level and validation action options to be `strict` and `error` respectively:

```js
const schema_validation = { $jsonSchema: bookstore_reviews_default };

db.runCommand({
  collMod: "reviews",
  validator: schema_validation,
  validationLevel: "strict",
  validationAction: "error",
});
```
