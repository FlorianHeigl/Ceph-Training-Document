
#Data Placement





##Pool

>Ceph stores data within pools, which are **logical groups** for storing objects. Pools manage the number of placement groups, the number of replicas, and the ruleset for the pool. To store data in a pool, you must have an authenticated user with permissions for the pool.

簡單說 Pool 就是 PG 的集合, 一個Pool內會有好幾個PG (目前ceph default 是8), 一個Pool內有多少個PG會影響資料的分散程度, 所以PG太小不太好, PG太大也沒有好處只會消耗更多計算資源 [XX]

![enter image description here](https://lh3.googleusercontent.com/-4CmlpyLj6tk/VpuvbJwFkdI/AAAAAAAACb0/PWvkp0JAR_U/s0/Image.png "pool_pg1.png")


###Why Pool ?
有Pool這個邏輯層會比較好一次管理和設定這麼多個PG

![enter image description here](https://lh3.googleusercontent.com/-A-HODlrn3zE/VqCgBv75dCI/AAAAAAAACfU/SBFUvsO9dQk/s0/Image.png "pool2.png")


**Pool的好處和功能如下:**

**1. Resilience:** You can set the number of copies/replicas of an object in the pool.

**2. Placement Groups:** You can set the number of placement groups for the pool.

**3. CRUSH Rules:** You can create a custom CRUSH rule for your pool.

**4. Set Ownership:** You can set a user ID as the owner of a pool.

**5. Snapshots:** You effectively take a snapshot of a particular pool.


 Ref: http://docs.ceph.com/docs/hammer/rados/operations/pools/



##Placement Group
![enter image description here](https://lh3.googleusercontent.com/-UKjtzqYgF-Y/VqDhn7yoH8I/AAAAAAAACgA/MZMa3M0Oyzk/s0/Image.png "PG_OSD_MAP.png")

> Ceph maps objects to placement groups (PGs). Placement groups (PGs) are shards or fragments of a logical object pool that place objects as a group into OSDs. Placement groups reduce the amount of per-object metadata when Ceph stores the data in OSDs. A larger number of placement groups (e.g., 100 per OSD) leads to better balancing.

http://docs.ceph.com/docs/hammer/rados/operations/placement-groups/


### Why Placement Group ?
PG 也是一個邏輯層概念, 可以把他想成是一群 object 的集合, 加入PG這一層是為了能夠更好的分散 object 和計算 object 位置並且達到系統的擴充性

Ceph 的設計上是去避免"單點XXX", 所以 Client 是直接去對 OSD 寫入 object 的, 如果沒有 PG 這一層的話, 當 object 數量達到數以萬計的時候要去計算 object 的位置會非常困難且消耗資源, 因為這些 object 都分別寫到不同的 OSD 當中

* 將 objects 集合成一個 PG 的最大好處就是降低了要追蹤和處理 metadata 的數量, 節省大量計算資源 
>不需要去記錄每一個 object 的位置和 metadata, 只要去管理 PG 的 metadata 就好, 而 PG 的數量級遠遠低於 object 的數量級

* 增加 PG 數量可均衡每一個 OSD 的附載, 提高資料存取的併行度 
* 分隔故障域，提高數據的可靠性 (減少大量的資料搬移)



### Summary
 * Pool 裡面包含了 objects
 * PG 裡面包含了所有在 Pool 裡面的一群 objects (一個 PG有多個 object)
 * 一個 object 只會屬於一個 PG
 * 一個 PG 可以對應到多個 OSD

Ceph的命名空间是 (Pool, Object)，每个Object都会映射到一组OSD中(由这组OSD保存这个Object)：
(Pool, Object) → (Pool, PG) → OSD set → Disk

### How to Mapping ?

![enter image description here](https://lh3.googleusercontent.com/-6yKkB_MmnW4/VqDtMTsDpLI/AAAAAAAACgw/DwDoPMMK99E/s0/Image.png "crush2.png")

>When a Ceph Client binds to a Ceph Monitor, it retrieves the latest copy of the Cluster Map. With the cluster map, the client knows about all of the monitors, OSDs, and metadata servers in the cluster. **However, it doesn’t know anything about object locations.**


> **Object locations get computed !!!  (所有的計算都是在 client 端做)**


>The only input required by the client is the object ID and the pool. It’s simple: Ceph stores data in named pools (e.g., “liverpool”). When a client wants to store a named object (e.g., “john,” “paul,” “george,” “ringo”, etc.) it calculates a placement group using the object name, a hash code, the number of PGs in the pool and the pool name. Ceph clients use the following steps to compute PG IDs.

>The client inputs the pool ID and the object ID. (e.g., pool = “liverpool” and object-id = “john”)
Ceph takes the object ID and hashes it.
Ceph calculates the hash modulo the number of PGs. (e.g., 58) to get a PG ID.
Ceph gets the pool ID given the pool name (e.g., “liverpool” = 4)
Ceph prepends the pool ID to the PG ID (e.g., 4.58).
Computing object locations is much faster than performing object location query over a chatty session. The CRUSH algorithm allows a client to compute where objects should be stored, and enables the client to contact the primary OSD to store or retrieve the objects.

![enter image description here](https://lh3.googleusercontent.com/-3h4ZkwMXe6I/Vpu6r8_6bVI/AAAAAAAACco/c0MFBSfJmLQ/s0/%25E6%2593%25B7%25E5%258F%2596.JPG "data_placement.JPG")


 ### Where PG??

##OSD