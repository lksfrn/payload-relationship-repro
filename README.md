# Payload camelCase relationship issues reproduction

### 1. When using collection with two or more words in camelCase, eg. `myExamples`, one cannot build a relationship field.

See files `src/collections/MyExamples.ts` and `src/collections/Users.ts`. The error is:

```
TypeError: Cannot read properties of undefined (reading 'config')
    at relationship (/home/luky/Projects/payload-relationship-repro/node_modules/payload/src/graphql/schema/buildMutationInputType.ts:148:68)
    at /home/luky/Projects/payload-relationship-repro/node_modules/payload/src/graphql/schema/buildMutationInputType.ts:214:12
    at Array.reduce (<anonymous>)
    at buildMutationInputType (/home/luky/Projects/payload-relationship-repro/node_modules/payload/src/graphql/schema/buildMutationInputType.ts:205:20)
    at /home/luky/Projects/payload-relationship-repro/node_modules/payload/src/collections/graphql/init.ts:131:85
    at Array.forEach (<anonymous>)
    at initCollectionsGraphQL (/home/luky/Projects/payload-relationship-repro/node_modules/payload/src/collections/graphql/init.ts:38:36)
    at registerSchema (/home/luky/Projects/payload-relationship-repro/node_modules/payload/src/graphql/registerSchema.ts:36:18)
    at init (/home/luky/Projects/payload-relationship-repro/node_modules/payload/src/init.ts:75:19)
    at initSync (/home/luky/Projects/payload-relationship-repro/node_modules/payload/src/init.ts:150:7)
[nodemon] app crashed - waiting for file changes before starting...
```

### 2. Do not change collecation name casing when accessing via REST.

Currently, said `myExamples` would be transformed to `my-examples` in REST API. I think this is bad from a numerous reasons:

1. Devs should not expect changes in collection names. It feels unnatural.
2. The name is not changed constantly. Only when `/api/my-examples`. But when your query your field, the casing remains in camelCase. Also when accessing globals, it is not transformed: `/api/globals/myOtherGlobal`.
3. It creates unnecessary overhead. It could be easily skipped. Also, it would prevent point #1.
