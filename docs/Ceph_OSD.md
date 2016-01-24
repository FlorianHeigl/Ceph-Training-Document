


#OSD (Object Storage Daemon)

##Introduction
>A Ceph OSD Daemon (Ceph OSD) stores data, **handles data replication, recovery, backfilling, rebalancing, and provides some monitoring information to Ceph Monitors** by checking other Ceph OSD Daemons for a heartbeat. A Ceph Storage Cluster requires at least two Ceph OSD Daemons to achieve an `active + clean` state when the cluster makes two copies of your data (Ceph makes 3 copies by default, but you can adjust it).

###OSD recommendations:
* minimum hard disk drive size of 1 terabyte
* more memory ~ 1GB per Ceph OSD (during rebalancing, backfilling and recovery)
* one drive for each Ceph OSD Daemon, need avoid multiple OSDs, and/or multiple journals on the same drive

> 
>You may run multiple Ceph OSD Daemons per hard disk drive, but this will likely lead to resource contention and diminish the overall throughput.
>
> **NOTE:** Running multiple OSDs on a single disk–irrespective of partitions–is **NOT** a good idea.



## Add OSD
### 流程:
![enter image description here](https://lh3.googleusercontent.com/-gBCmmOg428A/VqQzppCpFeI/AAAAAAAAChE/f86I92jEYGs/s0/Image.png "add_osd2.png")

1. Admin user 執行 `ceph-deploy osd activate storage_server3:/dev/sdb` 指令給 MON , 對 stroage server 上面的某個硬碟建立OSD.
2. 經過 MON 認證通過之後才會執行指令
3. 先對硬碟對格式化成 EXT4 or XFS 之後建立 OSD 相關資料和 keyring
4. 啟動 OSD daemon
5. MON 更新 cluster map 並且把最新的 cluster map 丟給所有 OSD

> **NOTE:** OSD 上面會有 MON map, OSD map 和 PG  map (應該也有 CRUSH)


##OSD Heartbeat and Peering
###Heartbeat 的應用

Heartbeat 主要用於及時發現OSD的變化 (down/up), 並且通知Monitor去更新OSD Map, 最後將最新版本的OSD Map同步到相關的 OSD 上面.

![enter image description here](http://docs.ceph.com/docs/master/_images/ditaa-2ad4d285aa0fb0ed30f32eb7137638c5d045f92a.png)

**OSD Heartbeat 的方法有兩種:**

**1. OSD <--> MON:** OSD 本身會定期回報自身的狀態給 Monitor (default 120s)

**2. OSD <--> OSD:** OSD 之間也會有 Heartbeat (default 6s), 來監聽其他OSD是否有掛掉的情況發生, 並且通知 Monitor

###如何選擇 Heartbeat 的對象?
OSD 之間的 heartbeat 並不是去ping 所有的 OSD, 這樣會消耗太多 OSD 的資源且對網路的負擔也很大. 
這裡是 OSD 只會對"OSD本身所在的PG, 所包含的其他OSD"去做heartbeat !! (所以會用到 PG map)

**Example:** 假設有三個PG 他們分別對應的OSD如下:

PG 1.1 -> [OSD.1, OSD.5, OSD.2]

PG 1.2 -> [OSD.3, OSD.1, OSD.6]

PG 2.1 -> [OSD.2, OSD.1, OSD.9]

那OSD.1的heartbeat 對象就會為 OSD.2 OSD.5 OSD.3 OSD.6 OSD.9


###如何做 Heartbeat ?
Heartbeat 本身是透過 socket 去實作, 所以每個OSD都會開3個port, 分別給下列三者使用:

![enter image description here](http://docs.ceph.com/docs/hammer/_images/ditaa-7aacc46d3624d8b5f65c30b294080e1e69bbd29c.png)

[\[1\]](http://docs.ceph.com/docs/hammer/rados/configuration/network-config-ref/#osd-ip-tables)

+ **MON/Clinet:**  跟 MON 溝通和 client R/W object 
+ **OSD:** OSD 之間傳送複本使用
+ **Heartbeat:** 這個 port 才是專們給 MON 和 OSD 之間做 heartbeat 使用

![enter image description here](https://lh3.googleusercontent.com/-pd8bQzWrnKM/VpvZEWTUK8I/AAAAAAAACdA/G5072pEWeFA/s0/Image.png "osd_port.png")