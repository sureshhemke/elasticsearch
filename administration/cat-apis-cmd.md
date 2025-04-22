##
```
GET _cat    # cat api help
GET _cat/master?v=true    # Check master node details
GET _cat/master?format=yaml
GET _cat/nodes?v    # Check all nodes details
GET _cat/nodes?format=json
```

# Check clutser status
```
GET /_cat/health?v
GET /_cluster/health?pretty
GET _cat/health?format=json
GET _cat/health?format=yaml
```
# Get cluster state details
```
GET _cluster/stats?human&pretty
```
# Check shards details
GET _cat/shards/?v
GET /_cat/shards?v&h=index,shard,prirep,state,unassigned.reason

# Indices related CAT apis
```
GET /_cat/indices?v   # To see all indices
GET /_cat/indices?v&h=index,store.size    # check indices with headers like, index,store.size 
GET _cat/indices/history_man?v  # Check indice health status for 'history_man'
GET _cat/indices?v&s=index&h=index,docs.count    # Check indices with specific header
GET _cat/indices/lead*    # Check indices start with "lead"
GET _cat/indices?v&h=index
```

# Check hindden indices
```
GET /_cat/indices/*.*?v
```
# Check all indices except hidden
```
GET /_cat/indices/*,-.*?v
```
# Snapshot and repo related commands
# List all the repository from linux terminal
```
curl -u username:password -X GET http://10.151.43.8:9200/_snapshot/_all?pretty
```
# List all the repository from linux terminal without cred
```
curl -X GET http://10.151.43.87:9200/_snapshot/_all?pretty
GET /_snapshot/_all
GET /_snapshot/_all?pretty
GET _snapshot/training-repo/_all?pretty 
```
# Delete single repository from linux terminal
```
curl -u username:password -X DELETE http://10.151.43.87:9200/_snapshot/training-repo
curl -X DELETE http://10.151.43.87:9200/_snapshot/training-repo
DELETE /_snapshot/training-repo
```
# List all snapshots
```
GET _cat/snapshots?pretty
```
# To create a repository use below command.
@ Connect to master node and run below command
```
curl -u username:password -X PUT "http://10.151.43.87:9200/_snapshot/training-repo" -H 'Content-Type: application/json' -d'
{
  "type": "s3",
  "settings": {
    "bucket": "training-repo-bucket",
    "region": "us-east-1",
    "base_path": "snapshots/training-repo-folder"
  }
}'
```

# To list all the snapshot from particular repo
```
curl -u username:password -X GET http://10.151.43.87:9200/_cat/snapshots/training-repo/?v&pretty
```
curl -X GET http://10.151.43.87:9200/_cat/snapshots/training-repo/?v&pretty
GET _cat/snapshots/training-repo
```
# Check all available repos
```
GET _cat/repositories
```

# Check all shards allocation
```
GET _cat/allocation?v
```
# Check pending task
```
GET /_cat/pending_tasks?v
```
# Node related commands with this commands we can check operation is going on node. like _search etc.
```
GET /_cat/nodes/datahot-i-082eb774d17f24756/stats
GET /_nodes/stats
GET /_nodes/10.151.42.208/stats
```

# Check data of docreports
```
GET docreports/_search
```
# Check data of docreports with filed
GET activityflow_docreports_2023_05_v1/_search
{
  "size": 1,
  "sort": [
    {
      "createdTime": {
        "order": "desc"
      }
    }
  ]
}

# Close docreports indice
```
POST /docreports/_close
```
# Open  docreports indice
```
POST /docreports/_open
```
GET /docreports/_search?size=10

# Get all indices
GET /*,-.security"


POST /_snapshot/ghx-corex-es-prd-va-7-17-af-for-refresh1/corex_prd_es_af_all_indices_202503110900/_restore
{
  "indices": "accounts_v1"
}

GET /_snapshot/ghx-corex-es-prd-va-7-17-af-for-refresh1/corex_prd_es_af_all_indices_202503110900/_status



# Create accounts_v1 indice with # 2 shard and # 1 replica
```
PUT /accounts_v1
{
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 1
  }
}
````
# Change # of replica setting
```
PUT /accounts_v1/_settings
{
  "settings": {
    "number_of_replicas": 1
  }
}
```

# Check number of replica
GET _cat/indices?h=i,rep,s&format=json

# Delet all indices expect . hidden
```
DELETE *,-.*
```

# Check Threads Pool
--------------------------------------
# Shows active, queued, rejected thread counts of all nodes
GET _cat/thread_pool/search?v
All nodes all operations
GET _cat/thread_pool/?v
GET _cat/thread_pool/search?v&h=node_name,name,active,queue,rejected
Check thread pool for specific node with all operations:
GET _nodes/<node ID or node name>/stats/thread_pool
GET _nodes/B5EGhGc6Qd2nCo4vkHyPBg/stats/thread_pool
GET _nodes/_datahot-i-00d7e1bcbcfac92d8/stats/thread_pool

To check all the nodes thread pool operaton
GET _nodes/stats/thread_pool
Check thread_pool.search.queue_size for all nodes
GET _nodes/thread_pool?filter_path=**.thread_pool.search
Get node info
GET _nodes/B5EGhGc6Qd2nCo4vkHyPBg/_search
To check the Node ID, Name , IP and Role
GET _cat/nodes?v&h=ip,name,nodeId,nodeRole
GET _nodes?filter_path=nodes.*.name,nodes.*.host
Check Running  _search Task/Queries
shows search-related tasks currently executing (e.g. long queries, scrolls).
GET _tasks?detailed=true&actions=*search
GET _tasks?actions=*search
List all tasks
GET _tasks
Cancel specific task.
POST _tasks/<task_id>/_cancel
POST _tasks/UuqJDVRzSumtUcYBIovhHA:8772126217/_cancel
Task ID format: <node_id>:<task_number>
Filter long-running tasks (> 5s):
GET _tasks?detailed=true&actions=*search&group_by=none
Then filter them manually by running_time_in_nanos.

Check CPU Load of all the nodes:
GET _nodes/stats/os

Check snapshot Recovery Progress
To see how shards are being restored on nodes:
GET /_cat/recovery?
GET /_cat/recovery?v&active_only=true
active_only=true: Shows only active recovery operations (not completed ones).
GET /_cat/recovery?v=true&h=index,shard,time,type,stage,files_percent,bytes_percent,repository,snapshot,source_host,target_host&active_only=true
You can add customs field as you want. Use true or false as per need.

Monitor Snapshot restore Task Status
GET /_tasks?detailed=true&actions=*start_recovery&group_by=none

Check Cluster Health
GET /_cluster/health?pretty
Look for:
•	status (should ideally be yellow or green, not red)
•	number_of_pending_tasks
•	initializing_shards
•	unassigned_shards (might show temporarily during restore)

During restores, Elasticsearch may throttle recoveries to avoid overloading nodes. You can speed it up (if resources allow) with settings like:
PUT /_cluster/settings
{
  "transient": {
    "indices.recovery.max_bytes_per_sec": "100mb",
    "cluster.routing.allocation.node_concurrent_recoveries": 10
  }
}

How to View Current Settings
GET /_cluster/settings?include_defaults=true&pretty
You can see:
persistent settings (set explicitly)
transient settings (temporary)
defaults (if not overridden)

Create a backup & restore user
1.	Create a backup_role with below cluster privileges
Monitor 
manage_slm 
cluster:admin/snapshot 
cluster:admin/repository
and with below Index privileges
Indices		Privileges
monitor 	all
2.	Create a user backup_user with backup-role.
