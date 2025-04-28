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

# To create the component template
```
PUT _component_template/audit_component_temp
{
  "template": {
    "settings": {
      "number_of_shards": 3,
      "number_of_replicas": 1,
      "refresh_interval": "1s",
      "index.lifecycle.name": "audit_policy",
      "index.lifecycle.rollover_alias": "audit-alias"
    },
    "mappings": {
      "_source": {
        "enabled": true
      },
      "dynamic": "strict",
      "properties": {
        "timestamp": {
          "type": "date"
        },
        "message": {
          "type": "text"
        },
        "status": {
          "type": "keyword"
        },
        "user_id": {
          "type": "keyword"
        }
      }
    },
    "aliases": {
      "audit-alias": {
        "is_write_index": true
      }
    }
  }
}
```

|Feature | Description|
|--------|------------|
number_of_shards: 3 | Each index has 3 primary shards
number_of_replicas: 1 | Each shard has 1 replica
refresh_interval: 1s | New data becomes searchable within 1 second
index.lifecycle.name | Attached to an ILM policy (assumes a audit_policy exists)
index.lifecycle.rollover_alias | Setting in index settings that points to the name of the alias used for rollover. It must match an alias name. which alias is used when new data is written into an index.
dynamic: strict | Only allow mapped fields (no automatic unknown fields)
timestamp, message, status, user_id | Sample fields with types
audit-alias | Alias for indexing and searching. Actual shortcut name used to read/write/search
"is_write_index": true| Use this index for writing when using the alias.

# Use this (audit_component_temp) component template in an index template:
```
PUT _index_template/audit_template
{
  "index_patterns": ["audit-*"],
  "composed_of": ["audit_component_temp"], 
  "priority": 500,
  "template": {
    "settings": {},
    "mappings": {},
    "aliases": {}
  }
}
```

