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
# Check all available repos
```
GET _cat/repositories
```
# Delete single repository from linux terminal
```
curl -u username:password -X DELETE http://10.151.43.87:9200/_snapshot/training-repo
curl -X DELETE http://10.151.43.87:9200/_snapshot/training-repo
DELETE /_snapshot/training-repo
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
# List all snapshots
```
GET _cat/snapshots?pretty
```
# To list all the snapshot from particular repo
```
curl -u username:password -X GET http://10.151.43.87:9200/_cat/snapshots/training-repo/?v&pretty
```
curl -X GET http://10.151.43.87:9200/_cat/snapshots/training-repo/?v&pretty
GET _cat/snapshots/training-repo
```
# Restore single indice from snapshot i.e account
```
POST /_snapshot/training-repo/prod_all_indices_202503110900/_restore
{
  "indices": "accounts"
}
```
# Restore complete snapshot
```
POST /_snapshot/training-repo/prod_all_indices_202503110900/_restore
```
# Check snapshot status
```
GET /_snapshot/training-repo/prod_all_indices_202503110900/_status
```

# Track snapshot restore process
```
# Check snapshot Recovery Progress
# To see how shards are being restored on nodes:
```
GET /_cat/recovery?
GET /_cat/recovery?v&active_only=true
```
**active_only=true:** Shows only active recovery operations (not completed ones).

# Check recovery details with specific headers
```
GET /_cat/recovery?v=true&h=index,shard,time,type,stage,files_percent,bytes_percent,repository,snapshot,source_host,target_host&active_only=true
```
You can add customs field as you want. Use true or false as per need.

# **Monitor Snapshot restore Task Status**
```
GET /_tasks?detailed=true&actions=*start_recovery&group_by=none
```

**During restores, Elasticsearch may throttle recoveries to avoid overloading nodes. You can speed it up (if resources allow) with settings like:**
```
PUT /_cluster/settings
{
  "transient": {
    "indices.recovery.max_bytes_per_sec": "100mb",
    "cluster.routing.allocation.node_concurrent_recoveries": 10
  }
}
```
