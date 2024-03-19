# Code Summary: Unnecessary Indexes

To obtain index usage stats for a collection, use the `$indexStats` aggregation stage. The following aggregation pipeline lists the `name`, `ops` and `since` fields for the indexes in the collection and sorts them by number of times they have been used:

```js
var sortIndexesByAccessOpsPipeline = [
  { $indexStats: {} },
  {
    $project: {
      _id: 0,
      name: "$name",
      ops: "$accesses.ops",
      since: "$accesses.since",
    },
  },
  { $sort: { ops: 1 } },
];
```
