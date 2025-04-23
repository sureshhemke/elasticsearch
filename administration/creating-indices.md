# Index management
• Create an index named **leads_b2b-000001** that has 1 primary shard and 0 replica shards:
```
PUT leads_b2b-000001
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  }
}
```

• Create an index named **leads_b2c-000001** that has 1 primary shard and 1 replica shards:
```
PUT leads_b2c-000001
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 1
  }
}
```
• Check the health of the cluster to verify that the index was created successfully with the proper replication configured to maintain a green status:
```
GET _cat/health?v
```
• Check the status of all the indices on the cluster and verify the index's individual status:
```
GET _cat/indices?v
```
