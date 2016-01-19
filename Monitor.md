#Monitor


當要安裝一個 Ceph cluster 的第一步就是必續先建立 Monitor (MON),  一般而言至少需要有3個 MON 以上來確保整個系統的可靠度.

 > **NOTE: ** MON 最好是分散在不同的Host上面而且必須是奇數個

**Ceph Monitor daemon的主要功能如下**

1. 透過 cluster map 來記錄 Ceph cluster 的所有狀態
2. Maintain a master copy of the cluster map
2. 確保 MON 之間的資料一致性 (透過Quorum和Paxos演算法)
3. 驗證來自用戶端的請求 (但是 data path 並不會經過 MON)

**Cluster map 包含下面5種 map :**

1. Monitor map
2. OSD map
3. Placement Group (PG) map
4. CRUSH map
5. MDS map (若使用 Ceph File System 才會有)


##Cluster MAPs
###Monitor Map

```
{ "election_epoch": 10,
  "quorum": [
        0,
        1,
        2],
  "monmap": { "epoch": 1,
      "fsid": "444b489c-4f16-4b75-83f0-cb8097468898",
      "modified": "2011-12-12 13:28:27.505520",
      "created": "2011-12-12 13:28:27.505520",
      "mons": [
            { "rank": 0,
              "name": "monitor-1",
              "addr": "172.17.100.1:6789\/0"},
            { "rank": 1,
              "name": "monitor-2",
              "addr": "172.17.100.2:6789\/0"},
            { "rank": 2,
              "name": "monitor-3",
              "addr": "172.17.100.3:6789\/0"},
           ]
    }
}
```
###OSD Map
###Placement Group (PG) map
###CRUSH Map



----------



##Create Monitor
Ceph monitors are light-weight processes

###流程:
![enter image description here](https://lh3.googleusercontent.com/-xYdFPy5GVD8/Vp5nBIJwtOI/AAAAAAAACdc/RALgw77aCuI/s0/Image.png "create_mon.png")

1. Admin user 配置好 ceph.conf, 裡面指定 Ceph monitor 的 host name 和 host ip
2. 透過 `ceph-authtool` 指令去建立 administrator keyring 和 monitor keyring
>**Monitor Keyring:** Monitors communicate with each other via a secret key. You must generate a keyring with a monitor secret and provide it when bootstrapping the initial monitor(s).
>
**Administrator Keyring:** To use the ceph CLI tools, you must have a client.admin user. So you must generate the admin user and keyring, and you must also add the client.admin user to the monitor keyring.

3. 把 administrator keyring  中的 client.admin user 加入到  monitor keyring 內
>因為之後 user 的認證會透過 monitor, 所以要先把 admin user 加進去, 這樣之後擁有admin keyring 的這個 user 向 monitor 認證的時候才會通過, 就可以做 admin 權限的操作 (例如: ceate/delete RBD, add monitor...)

3. 用 `monmaptool` 指令去建立"第一個" monitor map (目前內容只有一個 monitor 的資料)
4. 此時 monitor daemon 還沒有起來, 必須用 `ceph-mon` 指令去建立第一個 monitor daemon, 建立的同時必須把剛剛建好的 monitor keyring 和 monitor map 一起丟到 monitor daemon 上面
5. 啟動 monitor daemon



建立完MON的同時, 也會產生其他的 OSD map, PG map, CRUSH map,  裡面除了OSD map是空的以外其他兩個map 都已經有內容˙了, 這是因為 Ceph 會建立一些預設的 Pool 和 RUSH rule

**Ceph default pool:**

 * **RBD pool** (RBD 使用)
 
 * **Data pool** (Ceph FS使用)
 
 * **Metadata** (Ceph FS使用)


**Ceph default CRUSH rule:**

* TBD


此時的 Ceph cluster 狀態是 **"HEALTH_WARN"**, 因為還沒有加入任何的OSD.

(cluster status 圖)


-----
