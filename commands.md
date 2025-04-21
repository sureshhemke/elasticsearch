Check clutser status
```
GET /_cat/health?v
GET /_cluster/health?pretty
```
Check cluster status in YMAL format
```
GET _cat/health?format=yaml
```
GET _cluster/stats?human&pretty

GET _cat/master?format=yaml
GET _cat/nodes?format=json

GET _cat/shards/?v
GET /_cat/shards?h=index,shard,prirep,state,unassigned.reason
GET /_cat/shards?h=index,shard,prirep,state
GET _cat/indices?v
GET /_cat/indices?h=index,store.size
GET /_cat/indices?.
GET _cat/indices/po_history_lines_v1?v
GET /_cat/indices
GET _cat/indices?v&s=index
GET _cat/indices?v&s=index&h=index,docs.count
GET _cat/indices/prediction*
Check hindden index
GET /_cat/indices/*.*?v
GET _cat/indices?v&h=index

List all the repository from linux terminal
```
curl -u username:password -X GET http://10.151.43.8:9200/_snapshot/_all?pretty
```
List all the repository from linux terminal without cred
```
curl -X GET http://10.151.43.87:9200/_snapshot/_all?pretty
```
Delete single repository from linux terminal
```
curl -u username:password -X DELETE http://10.151.43.87:9200/_snapshot/training-repo
```
curl -X DELETE http://10.151.43.87:9200/_snapshot/training-repo
```
To create a repository use below command.
Connect to master node and run below command
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
To list all the snapshot from particular repo
```
curl -u username:password -X GET http://10.151.43.87:9200/_cat/snapshots/training-repo/?v&pretty
```
curl -X GET http://10.151.43.87:9200/_cat/snapshots/training-repo/?v&pretty
```

GET _cat/allocation?v
POST /_cluster/allocation/explain
{
  "index": "accounts_v1",
  "shard": 2,
  "primary": true
}

DELETE /x*

#DELETE /*, -".kibana", -".security", -".monitoring-*"

GET _snapshot/ghx-corex-es-prd-va-7-17-bt-for-restore/_all?pretty 
GET /_snapshot/_all
GET /_snapshot/_all?pretty
DELETE /_snapshot/ghx-corex-es-prd-va-7-17-bt-new-refresh

GET _cat/snapshots?pretty

GET /_cat/nodes/datahot-i-082eb774d17f24756/stats

GET /_nodes/stats
GET /_cat/pending_tasks?v

GET _cat/repositories

GET /_nodes/10.151.42.208/stats

DELETE /_snapshot/snapshotname

DELETE /g*

GET activityflow_docreports_2023_05_v1/_search

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


POST /activityflow_docs_2024_09_v9/_open

GET /activityflow_docs_2024_09_v9/_search?size=10

GET /*,-.security"


GET _cat/snapshots/ghx-corex-es-prd-va-7-17-af-for-refresh1

corex_prd_es_af_all_indices_202503110900

POST /_snapshot/ghx-corex-es-prd-va-7-17-af-for-refresh1/corex_prd_es_af_all_indices_202503110900/_restore
{
  "indices": "accounts_v1"
}

GET /_snapshot/ghx-corex-es-prd-va-7-17-af-for-refresh1/corex_prd_es_af_all_indices_202503110900/_status

PUT /accounts_v1/_settings
{
  "settings": {
    "number_of_shards": 2
  }
}


PUT /accounts_v1/_settings
{
  "settings": {
    "number_of_replicas": 1
  }
}

PUT /accounts_v1
{
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 1
  }
}

POST /accounts_v1/_close

GET _cat/shards?v

PUT /accounts_v1
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 1
  }
}
Check number of replica
GET _cat/indices?h=i,rep,s&format=json

Delet all indices expect . hidden
DELETE *,-.*

Check Threads Pool
--------------------------------------
Shows active, queued, rejected thread counts of all nodes
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




