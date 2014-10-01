---
layout: post
title: "SQL - SybaseASE - Alternative way to support LEAD/LAG function"
date: 2014-03-10 01:32:05 +0800
comments: true
categories: sql
---
There is audit trial requirement that need to be captured into log table these day. But we suffer from the limited function provided by Sybase ASE DB in order to achieve our goals. SybaseASE 12.5 does not support neither analytic function nor row number column, and therefore we have to implement cross comparison by ourselves via basic semantics.

{% codeblock lang:sql %}
--create rownum column
select rownum = identity(10), * from #temp_table from table where 1=2

insert into #temp_table
select * from table where pid = @pid
insert into #temp_table
select * from table_history where pid = @pid order by audit_update_date desc

--use self-join to match the cross rows
select a.rownum,
		case when a.col1 = b.col1 then 'equal' else 'inequal' end
from #temp_table a left out join #temp_table b on ( a.pid = b.pid and (a.rownum + 1) = b.rownum )
where ...
{% endcodeblock %}

In contrast, we also provide oracle version of implementation.

