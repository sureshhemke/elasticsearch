# All cluster related settings
# Note:: transient = Temporary || persistent = permanent

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
|---------|-------------|-------|
|cluster.routing.allocation.allow_rebalance|	Controls when shards can be rebalanced. During restore, you may want to delay rebalancing.|	"indices_all_active"|
|indices.recovery.concurrent_streams|	Number of concurrent file streams during recovery. Higher = faster, but heavier.|	5 or more|
|indices.recovery.concurrent_small_file_streams|	Specifically for small files, improving snapshot restore speed.|	3|
|indices.recovery.max_concurrent_file_chunks|	Controls how many files are sent at once in recovery. Useful for large indices.|	2 or more|

# Memory and Performance Tuning (General)
|Setting	|Description	|Example|
|---------|-------------|-------|
|thread_pool.search.size|	Number of search threads. Depends on CPU cores.|	2x #cores|
|thread_pool.write.size|	Write thread pool size. Critical during indexing or bulk restores.|	2x #cores|
|indices.store.throttle.type|	Throttling type for store IO. Set to none during restores for speed.|	"none"|
|indices.store.throttle.max_bytes_per_sec|	Limits how fast Elasticsearch writes data to disk. Can increase temporarily.|	"250mb"|

# Index and Cluster Operations
|Setting	|Default Value	|Recommended Value	|Explanation|
|---------|---------------|-------------------|-----------|
|action.auto_create_index|	true	|false	|Determines whether Elasticsearch automatically creates an index when a document is indexed to a non-existent index. It's recommended to set it to false to prevent unexpected index creation.|
|action.destructive_requires_name|	true	|true	|Ensures that destructive actions (like deleting indices) require explicit index names, preventing accidental deletion of multiple indices.|

# 📦Shard and Index Management
|Setting	|Default Value	|Recommended Value	|Explanation|
|---------|---------------|-------------------|-----------|
|cluster.max_shards_per_node	|1000	|Based on hardware and workload	|Limits the total number of primary and replica shards per node. Adjust according to your cluster's capacity.|
|cluster.max_shards_per_node.frozen	|3000	|Based on hardware and workload	|Limits the total number of frozen primary and replica shards per node.|

# 🧩 Miscellaneous Settings
|Setting	|Default Value	|Recommended Value	|Explanation|
|---------|---------------|-------------------|-----------|
|monitor.fs.health.enabled	|true	|true	|Enables periodic filesystem health checks.

# 🧠 JVM and Memory Settings
|Setting	|Default Value	|Recommended Value	|Explanation|
|---------|---------------|-------------------|-----------|
|JVM heap size	|Auto-configured	|50% of available RAM, up to 32GB	|Elasticsearch automatically sets the JVM heap size based on system memory. It's recommended to set it to 50% of available RAM, with a maximum of 32GB, to optimize performance.|
|-XX:HeapDumpPath	|Default path	|Custom path	|Specifies the path for heap dumps in case of OutOfMemoryError.|
|-XX:ErrorFile	|Default path	|Custom path	|Specifies the path for JVM fatal error logs.|

# ⚙️ Cluster Stability and Fault Detection
|Setting	|Default Value	|Recommended Value	|Explanation|
|---------|---------------|-------------------|-----------|
|cluster.election.duration	|500ms	500ms	|Maximum time allowed for each election.|
|cluster.election.initial_timeout	|100ms	100ms	|Initial timeout before attempting the first election.|
|cluster.election.max_timeout	|10s	10s	|Maximum timeout before retrying an election after a failure.|
|cluster.fault_detection.follower_check.interval	|1s	|1s	|Interval between follower checks to each node.|
|cluster.fault_detection.follower_check.timeout	|10s	|10s	|Timeout for follower check responses.|
|cluster.fault_detection.follower_check.retry_count	|3	|3	|Number of consecutive follower check failures before considering a node faulty.|
|cluster.fault_detection.leader_check.interval	|1s	|1s	|Interval between leader checks to the elected master.|
|cluster.fault_detection.leader_check.timeout	|10s	|10s	|Timeout for leader check responses.|
|cluster.fault_detection.leader_check.retry_count	|3	|3	|Number of consecutive leader check failures before considering the master faulty.|
