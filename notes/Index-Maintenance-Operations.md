# ✅ 1. Force Merge
What it does:
Cleans up and shrinks down the storage used by an index.

🧩 Force Merge in Elasticsearch is very similar to Defragmentation/Compaction in MongoDB

🧩 Runs force merge to combine small segment files into bigger ones, freeing disk space

🧹 Think of it like compacting files — Elasticsearch combines many small files into fewer big ones to save disk space and speed up reads.

When to use:

After deleting a lot of data

After reindexing

Before taking a snapshot
```
POST /my-index/_forcemerge
```
# ✅ 2. Flush Index
What it does:
Writes all in-memory data to disk and creates a commit point.

🧾 It's like saving your work in a document so you don't lose it if your computer crashes.

When to use:

Rarely needed — Elasticsearch does it automatically.

You might force it before shutting down the cluster.
```
POST /my-index/_flush
```
# ✅ 3. Refresh Index
What it does:
Makes newly indexed documents searchable immediately.

🔍 Imagine writing in a notebook — this tells Elasticsearch to scan the page so it shows up in search.

When to use:

After bulk insert when you want the data to be instantly searchable

Usually Elasticsearch refreshes automatically every second
```
POST /my-index/_refresh
```
# ✅ 4. Clear Cache
What it does:
Wipes out cached data (like field data, filters, etc.) to free memory.

🧠 It's like clearing your browser cache — good when your data changed and cache is stale.

When to use:

If you're troubleshooting memory

After large updates or mapping changes
```
POST /my-index/_cache/clear
```
# 📚 Alternate Terms (Depending on Context)

|Operation	|Ideal Terminology	|Description|
|-----------|-------------------|-----------|
|Force Merge	|Index Optimization	|Reduces number of segments on disk|
|Flush	|Commit Operation |Disk Sync	Saves in-memory data to disk|
|Refresh	|Visibility Operation	|Makes recent writes searchable|
|Clear Cache	|Cache Invalidation / Cleanup	|Frees memory by clearing old cache|
