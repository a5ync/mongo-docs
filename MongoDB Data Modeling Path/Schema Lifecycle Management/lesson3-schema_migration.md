# Code Summary: Schema Migration

Here’s the default schema for the `reviews` documents in our bookstore:

```js
const bookstore_reviews_default = {
  bsonType: "object",
  required: ["_id", "review_id", "user_id", "timestamp", "review", "rating"],
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

The new version of the schema includes the `locale` field and looks like this:

```js
const bookstore_reviews_international = {
    bsonType: "object",
    required: [
        …

        "locale",
        "schema_version"
    ],
    additionalProperties: false,
    properties: {
        …

        locale: {
          enum: ["en", "fr"],
        },
        schema_version: {
          enum: ["1.0.0"],
        },
    },
};
```

To enable schema validation for both schemas simultaneously, we used the `$jsonSchema` operator and the `oneOf` keyword:

```js
const schema_migration_validation = {
  $jsonSchema: {
    oneOf: [bookstore_reviews_default, bookstore_reviews_international],
  },
};
```

To update the schema validation rules for the existing collection we used the `collMod` command:

```js
db.runCommand({
  collMod: "reviews",
  validator: schema_migration_validation,
  validationLevel: "strict",
  validationAction: "error",
});
```
