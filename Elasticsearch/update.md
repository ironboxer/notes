### update doc

```
curl -X POST "http://47.94.212.241:9222/metacloud-file-content/file/11400/_update?pretty" -H 'Content-Type: application/json' -d'
{
  "doc": {
    "cates": ["联营", "委托代理", "劳务"]
  }
}
'
```



### update by query

```
curl -X POST "http://172.16.0.96:9222/happywrite/knowledge/_update_by_query?pretty&conflicts=proceed" -H 'Content-Type: application/json' -d'
{
  "script": {
    "source": "ctx._source.searchable = false",
    "lang": "painless"
  },
  "query": {
    "range": {
      "pubtime": {
        "gte": "2020-05-01"
      }
    }
  }
}
'

```



### update es refresh_interval

```
curl -X PUT "http://47.94.212.241:9222/metacloud-file-content/_settings?pretty" -H 'Content-Type: application/json' -d'
{
    "index" : {
        "refresh_interval" : "1s"
    }
}
'
```



```
curl -X POST "http://172.16.0.96:9222/happywrite/knowledge/_update_by_query?pretty&conflicts=proceed" -H 'Content-Type: application/json' -d'
{
  "script": {
    "source": "ctx._source.hit_count = 0",
    "lang": "painless"
  },
  "query": {
    "bool": {
      "must": {
        "match_all": {}
      }
    }
  }
}
'
```

