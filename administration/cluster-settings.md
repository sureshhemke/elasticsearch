# All cluster related settings

# View Current cluster Settings
```
GET /_cluster/settings?include_defaults=true&pretty
```
**You can see:
persistent settings (set explicitly)
transient settings (temporary)
defaults (if not overridden)**

# "thread_pool.search.queue_size"
•	This parameter defines the maximum number of search requests that can be queued when all the available search threads are busy on a node.
• You can define it in your **elasticsearch.yml** config file on each node:
**thread_pool.search.queue_size: 1000**

# "thread_pool.write.queue_size"
• This is similar to **search.queue_size**, but for write-heavy workloads.
•	If the write threads are all busy (indexing, bulk inserts), new writes queue up here.
• You can define it in your **elasticsearch.yml** config file on each node:
**thread_pool.write.queue_size: 1000**
**Setting via Cluster Update API (Dynamic Configuration)**
via API:
```
PUT /_cluster/settings
{
  "persistent": {
    "thread_pool.write.queue_size": "1000"
  }
}
```
# "indices.fielddata.cache.size"
• This setting controls how much heap memory Elasticsearch can use to cache field data — usually for aggregations, sorting, or scripting on text/keyword/numeric fields.

🛠️ You can set it like:
indices.fielddata.cache.size: 40%   # Max 40% of heap for fielddata
via API:
```
PUT /_cluster/settings
{
  "persistent": {
    "indices.fielddata.cache.size": "40%"
  }
}
```
# Speed up snapshot restore process
• During restores, Elasticsearch may throttle recoveries to avoid overloading nodes. You can speed it up (if resources allow) with settings like:
via API:
```
PUT /_cluster/settings
{
  "transient": {
    "indices.recovery.max_bytes_per_sec": "100mb",
    "cluster.routing.allocation.node_concurrent_recoveries": 10
  }
}
```

# Recovery & Restore Related Settings
|Setting	|Description	|Example |
|---------|-------------|--------|
|indices.recovery.max_bytes_per_sec|	Controls recovery speed (network/disk usage). Default is 40mb. You can increase to speed up restore.|	"100mb"|
|cluster.routing.allocation.node_concurrent_recoveries|	How many shards a node can recover at once (e.g. from another node or snapshot).|	10|
|cluster.routing.allocation.node_initial_primaries_recoveries|	Controls number of primary shards a node can recover in parallel on startup.|	30|
|cluster.routing.allocation.enable|	Controls shard allocation. Useful to disable during restores (none) and re-enable after (all).|	"none" / "all"|


# Snapshot Related Settings
|Setting	|Description	|Example|
|cluster.routing.allocation.allow_rebalance|	Controls when shards can be rebalanced. During restore, you may want to delay rebalancing.|	"indices_all_active"|
|indices.recovery.concurrent_streams|	Number of concurrent file streams during recovery. Higher = faster, but heavier.|	5 or more|
|indices.recovery.concurrent_small_file_streams|	Specifically for small files, improving snapshot restore speed.|	3|
|indices.recovery.max_concurrent_file_chunks|	Controls how many files are sent at once in recovery. Useful for large indices.|	2 or more|
