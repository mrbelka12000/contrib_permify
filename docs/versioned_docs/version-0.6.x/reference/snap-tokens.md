
# Snap Tokens & Zookies

A Snap Token is a token that consists of an encoded timestamp, which is used to ensure fresh results in access control checks. 

## Why you should use Snap Tokens ?

Basically, you should use snap tokens both for consistency and performance. The main goal of Permify is to provide an authorization system that ensures excellent performance that can handle millions of requests from different environments while ensuring data consistency.

Performance standards can be achievable with caching. In Permify, the cache mechanism eliminates re-computing of access control checks that once occurred, unless any relationships of resources don't change.

Still, all caches suffer from the risk of becoming stale. If some schema update happens, or relations change then all of the caches should be updated according to it to prevent false positive or false negative results.

Permify avoids this problem with an approach of snapshot reads. Simply, it ensures that access control is evaluated at a consistent point in time to prevent inconsistency. 

To achieve this, we developed tokens called Snap Tokens that consist of a timestamp that is compared in access checks to ensure that the snapshot of the access control is at least as fresh as the resource timestamp - basically its stored snap token.

## How to use Snap Tokens

Snap Tokens used in endpoints to represent the snapshot and get fresh results of the API's. It mainly used in [Write API] and [Check API]. 

The general workflow for using snap token is getting the snap token from the reponse of Write API request - basically when writing a relational tuple - then mapped it with the resource. One way of doing that is storing snap token in the additional column in your relational database.

Then this snap token can be used in endpoints. For example it can be used in access control check with sending via `snap_token` field to ensure getting check result as fresh as previous request.

```json 
{
  "schema_version": "ce8siqtmmud16etrelag",
  "snap_token": "gp/twGSvLBc=",
  "entity": {
    "type": "repository",
    "id": "1"
  },
  "permission": "edit",
  "subject": {
    "type": "user",
    "id": "1",
  },
}
```

[Write API]: ../../api-overview/data/write-data/
[Check API]: ../../api-overview/permission/check-api

### When Snap Token is NOT Provided

In Permify, every transaction is recorded in the 'transactions' table, and when a Snap Token is not provided, it retrieves the ID of the latest transaction from this table. This ID represents the most current snapshot of the database. After a query is executed with this ID, the results are then cached using this ID.

When two identical requests are made and neither specifies a Snap Token, the latest transaction ID will be requested from the database for both requests. Subsequently, the first request will write its result to the cache using a key and value like this:

```
check_{TRANSACTION_ID}_{schema_version}_{context}_organization:1#admin@user:1 -> true
```

When the second request arrives, since a transaction ID was not provided, the latest transaction ID will again be requested from the database. However, since the first request has already written the example above to the cache, and the second request will generate the same hash, this result will be retrieved from the cache.

## More on Cache Mechanism 

Permify implements several cache mecnanisims in order to achieve low latency in scaled distributed systems. See more on the section [Cache Mechanisims](./cache.md) 