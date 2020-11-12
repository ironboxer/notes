### mysql中count函数速度比较

```
count(field) < count(id) < count(1) <= count(*)
```

1. count(*): 专门优化过, 按照主键遍历+1, 但是不取字段的值
2. count(1): 按照主键遍历+1, 也不取值
3. count(id): 按照主键(id)遍历, 需要把id这个字段先去取出来, 虽然不做计算, 然后+1
4. count(field): 如果该field存在索引, 按照该field遍历, 然后获取主键id, 然后根据主键id找到该field的值, 取出来之后根据设置, 判断是否为NULL, 决定是否+1.
