#Monitor

當要安裝一個 Ceph cluster 的第一步就是必續先建立 Monitor (MON), 一般而言至少需要有3個 MON 以上來確保整個系統的可靠度.

 > **NOTE: ** MON 最好是分散在不同的Host上面而且必須是奇數個

**Ceph Monitor daemon的主要功能如下**
1. 透過 cluster map 來記錄 Ceph cluster 的所有狀態
2. 確保資料一致性 (透過Quorum和Paxos演算法)
3. 驗證來自用戶端的請求 (但是 data path 並不會經過 MON)

**Cluster map 包含下面5種 map :**
1. Monitor map
2. OSD map
3. Placement Group (PG) map
4. CRUSH map
5. MDS map (若使用 Ceph File System 才會有)


#MAPs
##Monitor Map
##OSD Map
##Placement Group (PG) map
##CRUSH Map


#Create Monitor

-----
