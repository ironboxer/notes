### terms

Returns documents that contain one or more **exact** terms in a provided field.

The `terms` query is the same as the [`term` query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-term-query.html), except you can search for multiple values.

#### Highlighting

[Highlighting](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-body.html#request-body-search-highlighting) is **best-effort** only. Elasticsearch may not return highlight results for `terms` queries depending on:

- Highlighter type
- Number of terms in the query

#### Terms lookup

Terms lookup fetches the field values of an existing document. Elasticsearch then uses those values as search terms. This can be helpful when searching for a large set of terms.

Because terms lookup fetches values from a document, the [`_source`](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-source-field.html) mapping field must be enabled to use terms lookup. The `_source` field is enabled by default.

By default, Elasticsearch limits the `terms` query to a maximum of **65,536** terms. This includes terms fetched using terms lookup. You can change this limit using the [`index.max_terms_count`](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules.html#index-max-terms-count) setting.

To perform a terms lookup, use the following parameters.

