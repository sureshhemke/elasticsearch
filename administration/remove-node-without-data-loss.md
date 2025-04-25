# Step-by-Step: Remove Node Without Data Loss
🎯 Goal: Migrate all shards off the node (datahot6) → safely remove the node → cluster stays green.

# Step 1: Identify the node you want to remove
• Run this to list nodes:
```
GET _cat/nodes?v
```
> Let’s say the node name is: datahot6

