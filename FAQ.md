


> Written with [StackEdit](https://stackedit.io/).

#Q&A

##What Pool?
>Pool is a logical groups for storing objects and ceph stores object within pools. Pools has the number of placement groups, the number of replicas, and the ruleset for the pool. 

簡單說 Pool 就是 PG 的集合, 一個Pool內會有好幾個PG (目前ceph default 是8), 一個Pool內有多少個PG會影響資料的分散程度, 所以PG太小不太好, PG太大也沒有好處只會消耗更多計算資源 [XX]

![enter image description here](https://lh3.googleusercontent.com/-4CmlpyLj6tk/VpuvbJwFkdI/AAAAAAAACb0/PWvkp0JAR_U/s0/Image.png "pool_pg1.png")

-------

##Where Pool ?
Ceph "沒有儲存pool", 如上圖所示其實Pool 只是PG的一個屬性(etc .. 555, 999), 來區分那些PG是屬於同一個group.

-------

##Why Pool ?
有Pool這個邏輯層會比較好一次管理和設定這麼多個PG

![enter image description here](https://lh3.googleusercontent.com/-CbLI6La84eE/Vpuvqol5B2I/AAAAAAAACcA/6Vh_e9y7ycc/s0/Image.png "pool_pg2.png")

**Pool的好處和功能如下:**

**1. Resilience:** You can set the number of copies/replicas of an object in the pool.

**2. Placement Groups:** You can set the number of placement groups for the pool.

**3. CRUSH Rules:** You can create a custom CRUSH rule for your pool.

**4. Set Ownership:** You can set a user ID as the owner of a pool.

**5. Snapshots:** You effectively take a snapshot of a particular pool.


 Ref: http://docs.ceph.com/docs/hammer/rados/operations/pools/

-------

##What PG ?
>PG = “**placement group**”. When placing data in the cluster, objects are mapped into PGs, and those PGs are mapped onto OSDs. Increasing the number of PGs can reduce the variance in per-OSD load across your cluster, but each PG requires a bit more CPU and memory on the OSDs that are storing it. We try and ballpark it at 100 PGs/OSD, although it can vary widely without ill effects depending on your cluster. 

PG 主要是透過 CRUSH 演算法將每個 object 平均分配到不同的 OSD上面, 來達到 distributes data 和 rebalances 的效果.

-------

##Where PG ?

##Why PG ?
>The layer(placement group) of **indirection** allows Ceph to rebalance dynamically when new OSDs come online.

可以增加 reliability, loadblance, 並且減少當有 OSD 掛掉時候的資料大量搬移


PG數量應該要多少的計算方法
https://access.redhat.com/documentation/en/red-hat-ceph-storage/1.3/storage-strategies/chapter-14-pg-count

-------
##Data Placement
![enter image description here](http://docs.ceph.com/docs/master/_images/ditaa-c7fd5a4042a21364a7bef1c09e6b019deb4e4feb.png)

>Each pool has a number of placement groups. CRUSH maps PGs to OSDs dynamically. When a Ceph Client stores objects, CRUSH will map each object to a placement group.

>Mapping objects to placement groups creates a layer of indirection between the Ceph OSD Daemon and the Ceph Client. The Ceph Storage Cluster must be able to grow (or shrink) and rebalance where it stores objects dynamically. If the Ceph Client “knew” which Ceph OSD Daemon had which object, that would create a tight coupling between the Ceph Client and the Ceph OSD Daemon. Instead, the CRUSH algorithm maps each object to a placement group and then maps each placement group to one or more Ceph OSD Daemons. This layer of indirection allows Ceph to rebalance dynamically when new Ceph OSD Daemons and the underlying OSD devices come online. The following diagram depicts how CRUSH maps objects to placement groups, and placement groups to OSDs.

每個Pool中有多個PG, PG 透過 CRUSH 演算法動態的對應到一某些OSD.
所以當Client 要將一個 Object 寫入的話, 首先第一步是要先找出這個Object 是對應到哪一個PG, 最後透過   CRUSH 就會知道這個Object 該寫入到哪一個OSD之中.

###Calculating PG IDs

>When a Ceph Client binds to a Ceph Monitor, it retrieves the latest copy of the Cluster Map. With the cluster map, the client knows about all of the monitors, OSDs, and metadata servers in the cluster. **However, it doesn’t know anything about object locations.**


> **Object locations get computed !!!**


>The only input required by the client is the object ID and the pool. It’s simple: Ceph stores data in named pools (e.g., “liverpool”). When a client wants to store a named object (e.g., “john,” “paul,” “george,” “ringo”, etc.) it calculates a placement group using the object name, a hash code, the number of PGs in the pool and the pool name. Ceph clients use the following steps to compute PG IDs.

>The client inputs the pool ID and the object ID. (e.g., pool = “liverpool” and object-id = “john”)
Ceph takes the object ID and hashes it.
Ceph calculates the hash modulo the number of PGs. (e.g., 58) to get a PG ID.
Ceph gets the pool ID given the pool name (e.g., “liverpool” = 4)
Ceph prepends the pool ID to the PG ID (e.g., 4.58).
Computing object locations is much faster than performing object location query over a chatty session. The CRUSH algorithm allows a client to compute where objects should be stored, and enables the client to contact the primary OSD to store or retrieve the objects.


![enter image description here](https://lh3.googleusercontent.com/-3h4ZkwMXe6I/Vpu6r8_6bVI/AAAAAAAACco/c0MFBSfJmLQ/s0/%25E6%2593%25B7%25E5%258F%2596.JPG "data_placement.JPG")

-------

#OSD has Maps?
Yes, OSD has MON MAP, OSD MAP
(PIC)

-------
#OSD heartbeat and peering 

-------
