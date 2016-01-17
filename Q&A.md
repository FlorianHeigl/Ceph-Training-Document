


> Written with [StackEdit](https://stackedit.io/).

#Q&A

##What Pool?
>Pool is a logical groups for storing objects and ceph stores object within pools. Pools has the number of placement groups, the number of replicas, and the ruleset for the pool. 

簡單說 Pool 就是 PG 的集合, 一個Pool內會有好幾個PG (目前ceph default 是8), 一個Pool內有多少個PG會影響資料的分散程度, 所以PG太小不太好, PG太大也沒有好處只會消耗更多計算資源 [XX]

![enter image description here](https://lh3.googleusercontent.com/-4CmlpyLj6tk/VpuvbJwFkdI/AAAAAAAACb0/PWvkp0JAR_U/s0/Image.png "pool_pg1.png")

##Where Pool ?
Ceph "沒有儲存pool", 如上圖所示其實Pool 只是PG的一個屬性(etc .. 555, 999), 來區分那些PG是屬於同一個group.

##Why Pool ?
有Pool這個邏輯層會比較好一次管理和設定這麼多個PG

![enter image description here](https://lh3.googleusercontent.com/-CbLI6La84eE/Vpuvqol5B2I/AAAAAAAACcA/6Vh_e9y7ycc/s0/Image.png "pool_pg2.png")

**Pool的好處和功能如下:**
1. **Resilience:** You can set the number of copies/replicas of an object in the pool.
2. **Placement Groups:** You can set the number of placement groups for the pool.
3. **CRUSH Rules:** You can create a custom CRUSH rule for your pool.
4. **Set Ownership:** You can set a user ID as the owner of a pool.
5. **Snapshots:** You effectively take a snapshot of a particular pool.

 Ref: http://docs.ceph.com/docs/hammer/rados/operations/pools/

##What PG ?
>PG = “**placement group**”. When placing data in the cluster, objects are mapped into PGs, and those PGs are mapped onto OSDs. Increasing the number of PGs can reduce the variance in per-OSD load across your cluster, but each PG requires a bit more CPU and memory on the OSDs that are storing it. We try and ballpark it at 100 PGs/OSD, although it can vary widely without ill effects depending on your cluster. 

PG 主要是透過 CRUSH 演算法將每個 object 平均分配到不同的 OSD上面, 來達到 distributes data 和 rebalances 的效果.


##Where PG ?

##Why PG ?
>The layer(placement group) of **indirection** allows Ceph to rebalance dynamically when new OSDs come online.

可以增加 reliability, loadblance, 並且減少當有 OSD 掛掉時候的資料大量搬移


PG數量應該要多少的計算方法
https://access.redhat.com/documentation/en/red-hat-ceph-storage/1.3/storage-strategies/chapter-14-pg-count

##Data Placement
