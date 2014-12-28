---
layout: post
title: atomic DB operation in web apps
date: 2014-12-28 20:44:11
categories: concurrency
---
Recently I met concurrency issues while trying to design a table that multiple thread on different hosts can access in a high frequency. Therfore I had to reconsider the solution to this situation.

After digging couples of references, list possbile solutions as below:
1. __distributed lock in memory__, e.g. redis, zookeeper
2. __pessimistic lock__(select .. for update), you cannot bear update failure while high-frequency collision occurs.
3. __optimistic lock__(version field), you bears update failure when collision occurs.
4. mysql `last_insert_id function`, that only works with the same db connection. This will be infeasible for web application while using db connection pools.
5. __split data__ across the machine and synchronized local thread
6. batch fetch and maintain in memory, this cannot avoid concurrency issues but on the other hand can decrease the chances on race condition.
7. You can also create __counter table__(`UPDATE hit_counter SET cnt = cnt + 1 WHERE slot = RAND() * 100;`), and retrieve statistics, just use aggregate queries(`retrieve statistics, just use aggregate queries`)

When concurrency issues happens in a high frequency rate, it is recommended to use distributed in-memory lock and perssimistic lock in order to keep avoiding update failure and retry logic in optimistic lock.

References:

1. [mysql last_insert_id](http://stackoverflow.com/questions/6153657/how-to-increment-a-counter-and-return-the-value-in-mysql)
2. [building_a_distributed_lock](http://nofluffjuststuff.com/blog/scott_leberknight/2013/07/distributed_coordination_with_zookeeper_part_5_building_a_distributed_lock)
3. [High Performance MySQL](http://www.amazon.com/High-Performance-MySQL-Jeremy-Zawodny/dp/0596003064/ref=sr_1_5?ie=UTF8&qid=1419773248&sr=8-5&keywords=high+performance+mysql)
