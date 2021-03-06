## function_score 查询

**function_score**作用于**过滤**之后的数据

[`function_score` 查询](https://www.elastic.co/guide/en/elasticsearch/reference/5.6/query-dsl-function-score-query.html) 是用来控制评分过程的终极武器，它允许为每个与主查询匹配的文档应用一个函数，以达到改变甚至完全替换原始查询评分 `_score` 的目的。

实际上，也能用过滤器对结果的 *子集* 应用不同的函数，这样一箭双雕：既能高效评分，又能利用过滤器缓存。

Elasticsearch 预定义了一些函数：

- **`weight`**

    为每个文档应用一个简单而不被规范化的权重提升值：当 `weight` 为 `2` 时，最终结果为 `2 * _score` 。

- **`field_value_factor`**

    使用这个值来修改 `_score` ，如将 `popularity` 或 `votes` （受欢迎或赞）作为考虑因素。

- **`random_score`**

    为每个用户都使用一个不同的随机评分对结果排序，但对某一具体用户来说，看到的顺序始终是一致的。

- ***衰减函数\* —— `linear` 、 `exp` 、 `gauss`**

    将浮动值结合到评分 `_score` 中，例如结合 `publish_date` 获得最近发布的文档，结合 `geo_location` 获得更接近某个具体经纬度（lat/lon）地点的文档，结合 `price` 获得更接近某个特定价格的文档。

- **`script_score`**

    如果需求超出以上范围时，用自定义脚本可以完全控制评分计算，实现所需逻辑。

如果没有 `function_score` 查询，就不能将全文查询与最新发生这种因子结合在一起评分，而不得不根据评分 `_score` 或时间 `date` 进行排序；这会相互影响抵消两种排序各自的效果。这个查询可以使两个效果融合：可以仍然根据全文相关度进行排序，但也会同时考虑最新发布文档、流行文档、或接近用户希望价格的产品。正如所设想的，查询要考虑所有这些因素会非常复杂，让我们先从简单的例子开始，然后顺着梯子慢慢向上爬，增加复杂度。



https://www.elastic.co/guide/cn/elasticsearch/guide/current/function-score-query.html

