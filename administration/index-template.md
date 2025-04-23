# Index template creation and managment
• Create Index template for load with indices patterns and for # of shard and replica setting:
```
PUT _index_template/load
{ "index_patterns": ["load-*"], "template": { "settings": { "number_of_shards": 1, "number_of_replicas": 0 } } }
```
# Verify index template
```
GET _index_template/load
```
# Create cpu index template
```
PUT _index_template/cpu
{ "index_patterns": ["cpu-*"], "template": { "settings": { "number_of_shards": 1, "number_of_replicas": 0 } } }
```
# Verify index template
```
GET _index_template/cpu
```
# Verify the patterns are working correctly by creating below indices:
```
PUT cpu-1
```
```
GET cpu-1
```
