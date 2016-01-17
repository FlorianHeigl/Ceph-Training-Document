


> Written with [StackEdit](https://stackedit.io/).

#Q&A

##What Pool?
>Pool is a logical groups for storing objects and ceph stores object within pools. Pools has the number of placement groups, the number of replicas, and the ruleset for the pool. 

簡單說 Pool 就是 PG 的集合, 一個Pool內會有好幾個PG (目前ceph default 是8), 一個Pool內有多少個PG會影響資料的分散程度, 所以PG太小不太好, PG太大也沒有好處只會消耗更多計算資源 [XX]

##Where Pool ?
Ceph "沒有儲存pool", 如上圖所示其實Pool 只是PG的一個屬性(etc .. 555, 999), 來區分那些PG是屬於同一個group.

##Why Pool ?
有Pool這個邏輯層會比較好一次管理和設定這麼多個PG

Pool的好處和功能如下:
>**Resilience:** You can set the number of copies/replicas of an object in the pool.
**Placement Groups:** You can set the number of placement groups for the pool.
**CRUSH Rules:** You can create a custom CRUSH rule for your pool.
**Set Ownership:** You can set a user ID as the owner of a pool.
**Snapshots:** You effectively take a snapshot of a particular pool.
