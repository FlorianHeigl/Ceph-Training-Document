#Ceph Monitor

##Introduction
>Ceph Monitors maintain a “master copy” of the cluster map, which means a Ceph Client can determine the location of all Ceph Monitors, Ceph OSD Daemons, and Ceph Metadata Servers just by connecting to one Ceph Monitor and retrieving a current cluster map. 
>**Before Ceph Clients can read from or write to Ceph OSD Daemons or Ceph Metadata Servers, they must connect to a Ceph Monitor first**. With a current copy of the **cluster map and the CRUSH algorithm**, a Ceph Client can compute the location for any object. The ability to compute object locations allows a Ceph Client to talk directly to Ceph OSD Daemons, which is a very important aspect of Ceph’s high scalability and performance. See [Scalability and High Availability](http://docs.ceph.com/docs/hammer/architecture/#scalability-and-high-availability) for additional details.

[[1]](http://docs.ceph.com/docs/hammer/rados/configuration/mon-config-ref/#background)

當要安裝一個 Ceph cluster 的第一步就是必續先建立 Monitor (MON),  一般而言至少需要有3個 MON 以上來確保整個系統的可靠度.

 > **NOTE: ** MON 最好是分散在不同的Host上面而且必須是奇數個

**Ceph Monitor daemon的主要功能如下**

1. 透過 cluster map 來記錄 Ceph cluster 的所有狀態
2. Maintain a master copy of the cluster map
2. 確保 monitor之間的資料一致性 (透過Quorum和Paxos演算法)
3. 驗證來自用戶端的請求 (但是 data path 並不會經過 monitor)

**Cluster map 包含下面5種 map :**

1. Monitor map
2. OSD map
3. Placement Group (PG) map
4. CRUSH map
5. MDS map (若使用 Ceph File System 才會有)


##Cluster Maps
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

Dump osd map command: `ceph osd dump -f json | python -m json.tool`

```
{
    ...........
    
    "flags": "",
    "fsid": "f0af2fce-313f-4162-9d49-d3a6535a0c24",
    "max_osd": 27,
    "modified": "2016-01-19 17:13:27.301319",
    "osd_xinfo": [
        {
            "down_stamp": "2016-01-19 17:12:25.032264",
            "features": 37154696925806591,
            "laggy_interval": 17,
            "laggy_probability": 0.3,
            "old_weight": 0,
            "osd": 0
        },
        {
            "down_stamp": "2016-01-19 17:12:25.032264",
            "features": 37154696925806591,
            "laggy_interval": 16,
            "laggy_probability": 0.3,
            "old_weight": 0,
            "osd": 1
        }],
    "osds": [
        {
            "cluster_addr": "192.168.10.101:6827/1005839",
            "down_at": 327,
            "heartbeat_back_addr": "192.168.10.101:6830/1005839",
            "heartbeat_front_addr": "192.168.10.101:6831/1005839",
            "in": 1,
            "last_clean_begin": 323,
            "last_clean_end": 336,
            "lost_at": 0,
            "osd": 0,
            "primary_affinity": 1.0,
            "public_addr": "192.168.10.101:6844/5839",
            "state": [
                "exists",
                "up"
            ],
            "up": 1,
            "up_from": 338,
            "up_thru": 338,
            "uuid": "70152bc9-36c4-425d-b326-1d7b7ee72f86",
            "weight": 1.0
        },
        {
            "cluster_addr": "192.168.10.101:6803/1003816",
            "down_at": 327,
            "heartbeat_back_addr": "192.168.10.101:6807/1003816",
            "heartbeat_front_addr": "192.168.10.101:6809/1003816",
            "in": 1,
            "last_clean_begin": 321,
            "last_clean_end": 335,
            "lost_at": 0,
            "osd": 1,
            "primary_affinity": 1.0,
            "public_addr": "192.168.10.101:6820/3816",
            "state": [
                "exists",
                "up"
            ],
            "up": 1,
            "up_from": 336,
            "up_thru": 336,
            "uuid": "88296e4c-26b9-4c7a-b3eb-8a7f3d18920b",
            "weight": 1.0
        }]
}
```
###Placement Group (PG) map

```
{
     "pg_stats": [
        {
            "acting": [
                19,
                4
            ],
            "acting_primary": 19,
            "blocked_by": [],
            "created": 1,
            "last_active": "2016-01-19 19:40:39.360266",
            "last_became_active": "0.000000",
            "last_became_peered": "0.000000",
            "last_change": "2016-01-19 19:40:39.360266",
            "last_clean": "2016-01-19 19:40:39.360266",
            "last_clean_scrub_stamp": "2016-01-19 19:40:39.360201",
            "last_deep_scrub": "0'0",
            "last_deep_scrub_stamp": "2016-01-13 19:22:46.541455",
            "last_epoch_clean": 333,
            "last_fresh": "2016-01-19 19:40:39.360266",
            "last_fullsized": "2016-01-19 19:40:39.360266",
            "last_peered": "2016-01-19 19:40:39.360266",
            "last_scrub": "0'0",
            "last_scrub_stamp": "2016-01-19 19:40:39.360201",
            "last_undegraded": "2016-01-19 19:40:39.360266",
            "last_unstale": "2016-01-19 19:40:39.360266",
            "log_size": 0,
            "log_start": "0'0",
            "mapping_epoch": 327,
            "ondisk_log_size": 0,
            "ondisk_log_start": "0'0",
            "parent": "0.0",
            "parent_split_bits": 0,
            "pgid": "0.17d",
            "reported_epoch": "340",
            "reported_seq": "137",
            "state": "active+clean",
            "stats_invalid": "0",
            "up": [
                19,
                4
            ],
            "up_primary": 19,
            "version": "0'0"
        }]
  }
```
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



建立完MON的同時, 也會產生其他的 OSD map, PG map, CRUSH map,  裡面除了OSD map是空的以外其他兩個map 都已經有內容了, 這是因為 Ceph 會建立一些預設的 Pool 和 CRUSH rule

**Ceph default pool:**

 * **RBD pool** (RBD 使用)
 
 * **Data pool** (Ceph FS使用)
 
 * **Metadata** (Ceph FS使用)


**Ceph default CRUSH rule:**

* TBD


此時的 Ceph cluster 狀態是 **"HEALTH_WARN"**, 因為還沒有加入任何的OSD.

(cluster status 圖)


----------


## Add Monitor
TBD


----------


## Delete Monitor
TBD

