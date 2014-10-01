---
layout: post
title: "SQL - Switch/Transpose columns and rows"
date: 2014-03-10 00:28:27 +0800
comments: true
categories: sql
---
In practice, we will meet the requirement to rotate the table including convert cols to rows, convert rows to cols. In this session, I'll produce the solution in common method as well as db-specified features.
##Convert columns to rows
{% codeblock lang:sql %}
--common solution
select * from (
	select id, 'col_1' as col_name, col_1 as col_val
	from table
	union all
	select id, 'col_2' as col_name, col_2 as col_val
	from table
	union all
	... 
) order by id
{% endcodeblock %}

{% codeblock lang:sql %}
--oracle 11g unpivot query
select *
from table
unpivot (
	col_val for col_name in (col_1 as 'col_1', col_2 as 'col_2')
)
order by id
{% endcodeblock %}

##Concert rows to columns
{% codeblock lang:sql %}
	--common solution
	select id,
		max(case when col_name = 'col_1' then col_val else null end) as col_1,
		max(case when col_name = 'col_2' then col_val else null end) as col_2,
		...
	from table
	order by id
{% endcodeblock %}

{% codeblock lang:sql %}
	--oracle 11g pivot query
	select *
	from table
	pivot (
		max(col_val) for col_name in ('col_1' as col_1,'col_2' as col_2)
	)
	where ...
	order by id
{% endcodeblock %}

##Excel pivot table
For non-sql approaches, we can use excel pivot table. Excel spreadsheets are a great way to pivot and analyze Oracle data, and tools.