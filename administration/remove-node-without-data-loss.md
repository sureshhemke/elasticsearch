# Step-by-Step: Remove Node Without Data Loss
🎯 Goal: Migrate all shards off the node (datahot6) → safely remove the node → cluster stays green.

# Step 1: Identify the node you want to remove
🔹Run this to list nodes:
```
GET _cat/nodes?v
```
> Let’s say the node name is: datahot6

# Step 2: Exclude the node from shard allocation
🔹 Below cmd tells "do not allocate any shards to this node", and migrate existing one safely on other available note.
```
PUT /_cluster/settings
{
  "transient": {
    "cluster.routing.allocation.exclude._name": "datahot6"
  }
}
```
🔹 You can also **use _ip**, **_host**, or **_id** instead of **_name**.

✅ Elasticsearch will now start moving shards away from this node to others.

# Step 3: Wait for shard relocation to complete

🔹Check that the node no longer has any shards:
```
GET /_cat/shards?v
```
🔹You can see no shards, Indices on node and disk space released
```
GET /_cat/allocation?v
```
🔹Check cluster health is green:
```
GET _cluster/health
```
📌 Wait until:
```
No shards are assigned to datahot6
Cluster status is GREEN
```
# Step 4: Stop and Disable the Elasticsearch service on that node
```
systemctl stop elasticsearch
systemctl disable elasticsearch
```

# Step 5: Remove exclusion setting
```
PUT /_cluster/settings
{
  "transient": {
    "cluster.routing.allocation.exclude._name": ""
  }
}
```

# 📘 Summary

|Step	|Action|
|-----|------|
|1	|Exclude node from allocation|
|2	|Wait for shard migration|
|3	|Stop the node|
|4	|Clean up exclusion setting|
