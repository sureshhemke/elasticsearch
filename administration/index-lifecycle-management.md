# Index Lifecycle Management (ILM)
# Introduction
• Efficiently ingesting continuously generated data 24/7 is one of the many things Elasticsearch is very good at. But eventually, you're going to run out of space. 
  This is why it is important to leverage the **index lifecycle management (ILM)** feature in Elasticsearch. 
  We creates ILM policies in Elasticsearch to optimize and move cold data to slower nodes and to retire old data to free up space.
  
# Create the "audit_policy" ILM Policy
```
PUT _ilm/policy/audit_policy
{
  "policy": {
   "phases": {
    "hot": {
     "actions": {}
    },
    "cold": {
     "min_age": "7d",
     "actions": {
       "freeze": {},
       "readonly": {}
     }
   },
   "delete": {
    "min_age": "365d",
    "actions": {
     "delete": {}
    }
   }
  }
 }
}
```

# Verify the policy was created successfully:
```
GET _ilm/policy/audit_policy
```
# Create the "audit_template" Index Template and map "audit_policy" to it
```
PUT _index_template/audit_template
{
  "index_patterns": ["audit-*"],
  "template": {
    "settings": {
     "number_of_shards": 1,
     "number_of_replicas": 0,
     "index.lifecycle.name": "audit_policy"
    }
  }
}
```
# Verify the template was created successfully:
```
GET _index_template/audit_template
```

# Create and Verify the First "audit-DD-MM-YYYY" Index
```
PUT audit-09-13-2021
```
# Verify the index was created successfully and audit_template and audit_policy allocated to it.
```
GET audit-09-13-2021
```

