### 	在已有的mapping中添加新的字段类型

```
PUT /twitter/_mapping
{
  "properties": {
    "email": {
      "type": "keyword"
    }
  }
}
```



```
[{"error":{"root_cause":[{"type":"remote_transport_exception","reason":"[node-242][172.16.0.242:9300][indices:data/write/update[s]]"}],"type":"illegal_argument_exception","reason":"Limit of total fields [1000] in index [metacloud-file-content-test-2020-05-15] has been exceeded"},"status":400}]
```



### 默认索引的字段数量为1000个

```
PUT /metacloud-file-content-test/_settings
{
  "index.mapping.total_fields.limit": 2000
}
```





```
curl -X PUT "http://47.94.212.241:9222/metacloud-file-content/file/_mapping?pretty" -H 'Content-Type: application/json' -d'
{
  "properties": {
    "cates": {
      "type": "keyword"
    }
  }
}
'
```



https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-put-mapping.html