# Low max virtual memory areas

Elasticsearch will not run and the following exception is present in the logs:

```
Exception in thread "main" java.lang.RuntimeException: bootstrap checks failed
initial heap size [268435456] not equal to maximum heap size [2147483648]; this can cause resize pauses and prevents mlockall from locking the entire heap
max virtual memory areas vm.max_map_count [65530] likely too low, increase to at least [262144]
```
    
Solution: `sudo sysctl -w vm.max_map_count=262144`

Source: [Github issue](https://github.com/docker-library/elasticsearch/issues/111)

# Read-only mode

Elasticsearch responses contains something like:

```
({"type"=>"cluster_block_exception", "reason"=>"blocked by: [FORBIDDEN/12/index read-only / allow delete (api)]
```

It is likely that disk is full. Free some space and run:

```
curl -XPUT localhost:9300/_settings --data '{"index": {"blocks": {"read_only_allow_delete": "false"}}}' --header 'content-type: application/json'
```

# Status 409 Conflict

After an insert the following is returned:
```
{
   "error": {
      "root_cause": [
         {
            "type": "version_conflict_engine_exception",
            "reason": "[...][1]: version conflict, current [2], provided [1]",
            "index": "...",
            "shard": "3"
         }
      ],
      "type": "version_conflict_engine_exception",
      "reason": "[...][1]: version conflict, current [2], provided [1]",
      "index": "...",
      "shard": "3"
   },
   "status": 409
}
```

The problem is that the document has been already [modified in the background](https://www.elastic.co/guide/en/elasticsearch/guide/current/optimistic-concurrency-control.html). Check out the application code for concurrency issues and try to insert the document again (optimistic concurrency control).
