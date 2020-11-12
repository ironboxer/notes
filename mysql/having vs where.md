### having vs where



> Why is it that you need to place columns you create yourself (for example "select 1 as number") after HAVING and not WHERE in MySQL?

`WHERE` is applied before `GROUP BY`, `HAVING` is applied after (and can filter on aggregates).

In general, you can reference aliases in neither of these clauses, but `MySQL` allows referencing `SELECT` level aliases in `GROUP BY`, `ORDER BY` and `HAVING`.

> And are there any downsides instead of doing "WHERE 1" (writing the whole definition instead of a column name)

If your calculated expression does not contain any aggregates, putting it into the `WHERE` clause will most probably be more efficient.

https://stackoverflow.com/questions/2905292/where-vs-having



当聚合查询需要group时, where 作用于group之前的每一条数据, having作用于group之后的每一条数据。



一般情况下用where，聚合查询用到group的时候可能会用到having。

