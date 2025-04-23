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
## Check all available repos
```
GET _cat/repositories
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
