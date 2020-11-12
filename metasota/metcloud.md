http://172.16.0.96:9939/v2/search/metacloud?uid=u&sid=s&q=document&debug=query

http://47.94.212.241:8601/app/kibana#/dev_tools/console?_g=()



### delete_by_qurey

```shell
curl -X POST "http://localhost:9222/log_nginx_uranus/_delete_by_query" -H 'Content-Type: application/json' -d '{"query": {"range": {"logtime": {"lte": "2020"}}}}'
```

