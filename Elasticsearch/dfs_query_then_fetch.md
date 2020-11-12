### dfs_query_then_fetch



###  Query Then Fetch[edit](https://github.com/elastic/elasticsearch/edit/6.8/docs/reference/search/request/search-type.asciidoc)

Parameter value: **query_then_fetch**.

The request is processed in two phases. In the first phase, the query is forwarded to **all involved shards**. Each shard executes the search and generates a sorted list of results, local to that shard. Each shard returns **just enough information** to the coordinating node to allow it to merge and re-sort the shard level results into a globally sorted set of results, of maximum length `size`.

During the second phase, the coordinating node requests the document content (and highlighted snippets, if any) from **only the relevant shards**.

This is the default setting, if you do not specify a `search_type` in your request.

### Dfs, Query Then Fetch[edit](https://github.com/elastic/elasticsearch/edit/6.8/docs/reference/search/request/search-type.asciidoc)

Parameter value: **dfs_query_then_fetch**.

Same as "Query Then Fetch", except for an initial **scatter** phase which goes and computes the **distributed term** frequencies for more accurate scoring.



问题的本质是TF-IDF中的IDF的信息是局部的，而不是全局的，所以得出的结论是局部最优解，而不是全局最优解。



一定要最终关注到问题上，并且思考问题是如何产生的。







