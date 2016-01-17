


> Written with [StackEdit](https://stackedit.io/).

#Q&A

##What Pool?
>Pool is a logical groups for storing objects and ceph stores object within pools. Pools has the number of placement groups, the number of replicas, and the ruleset for the pool. 

簡單說 Pool 就是 PG 的集合, 一個Pool內會有好幾個PG (目前ceph default 是8), 一個Pool內有多少個PG會影響資料的分散程度
所以PG太小不太好, PG太大也沒有好處只會消耗更多計算資源 [XX]
