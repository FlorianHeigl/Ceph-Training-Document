
#Data Placement


![enter image description here](https://lh3.googleusercontent.com/-9ZmRUyh65yw/VqCeP8hrLdI/AAAAAAAACe8/APtyj4Atv0o/s0/imageedit_3_8764371134.png "pool_mapping.png")


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
> Ceph maps objects to placement groups (PGs). Placement groups (PGs) are shards or fragments of a logical object pool that place objects as a group into OSDs. Placement groups reduce the amount of per-object metadata when Ceph stores the data in OSDs. A larger number of placement groups (e.g., 100 per OSD) leads to better balancing.

http://docs.ceph.com/docs/hammer/rados/operations/placement-groups/


### Why Placement Group ?
PG 也是一個邏輯層概念, 可以把他想成是一群 object 的集合, 加入PG這一層是為了能夠更好的分散 object 和定位 object, 並且達到系統的擴充性

Ceph 的設計上是去避免"單點XXX", 所以 Client 是直接去對 OSD 寫入 object 的, 如果沒有 PG 這一層的話, 當 object 數量達到數以萬計的時候要去計算 object 的位置會非常困難且消耗資源, 因為這些 object 都分別寫到不同的 OSD 當中

* 將 objects 集合成一個 PG 的最大好處就是降低了要追蹤和處理 metadata 的數量, 節省大量計算資源 
>不需要去記錄每一個 object 的位置和 metadata, 只要去管理 PG 的 metadata 就好, 而 PG 的數量級遠遠低於 object 的數量級

* 增加 PG 數量可均衡每一個 OSD 的附載, 提高資料存取的併行度 
* 分隔故障域，提高数据的可靠性



### Summary
 * Pool 裡面包含了 objects
 * PG 裡面包含了所有在 Pool 裡面的一群 objects (一個 PG有多個 object)
 * 一個 object 只會屬於一個 PG
 * 一個 PG 可以對應到多個 OSD

Ceph的命名空间是 (Pool, Object)，每个Object都会映射到一组OSD中(由这组OSD保存这个Object)：
(Pool, Object) → (Pool, PG) → OSD set → Disk
 ### Where PG??

##OSD