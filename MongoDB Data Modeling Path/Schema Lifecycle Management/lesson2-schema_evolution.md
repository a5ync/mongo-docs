# Code Summary: Schema Evolution

In this example, we implemented the updated `reviews` schema to include an optional `locale` field, in the `properties` document, which accepts only `fr` or `en` values:

```js
const bookstore_reviews_international = {
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
        locale: {
            enum: [“en”, “fr”]},
        },
    },
};
```

To enable or update schema validation for the existing collection, use the `collMod` command and provide the validator option. In our example, we used the `$jsonSchema` operator to build the validator and set the validation level and validation action options to `moderate` and `warn`:

```js
const schema_validation_international = {
  $jsonSchema: bookstore_reviews_international,
};

db.runCommand({
  collMod: "reviews",
  validator: schema_validation_international,
  validationLevel: "moderate",
  validationAction: "warn",
});
```
