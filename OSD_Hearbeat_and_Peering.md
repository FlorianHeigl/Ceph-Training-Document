
#OSD Heartbeat and Peering
##Hearbeat 的應用

![enter image description here](http://docs.ceph.com/docs/master/_images/ditaa-2ad4d285aa0fb0ed30f32eb7137638c5d045f92a.png)

Heartbeat 主要用於及時發現OSD的變化 (down/up), 並且通知Monitor去更新OSD MAP, 最後將最新版本的OSD MAP同步到相關的 OSD 上面.

**OSD Heartbeat 的方法有兩種:**

1. OSD <-> MON, OSD 本身會定期回報自身的狀態給 Monitor (default 120s)

2. OSD <-> OSD, OSD 之間也會有 Heartbeat (default 6s), 來監聽其他OSD是否有掛掉的情況發生, 並且通知 Monitor

##如何選擇 Heartbeat 的對象?
OSD 之間的 heartbeat 並不是去ping 所有的OSD, 這樣會消耗太多OSD的資源且對網路的負擔也很大. 
這裡是OSD只會對"本OSD所在的PG中, 所包含的其他OSD"去做heartbeat !!

Example:
假設有三個PG 他們分別對應的OSD如下:
PG 1.1 -> [OSD.1, OSD.5, OSD.2]

PG 1.2 -> [OSD.3, OSD.1, OSD.6]

PG 2.1 -> [OSD.2, OSD.1, OSD.9]

那OSD.1的heartbeat 對象就會為 OSD.2 OSD.5 OSD.3 OSD.6 OSD.9


##OSD 和 MON之間如何做 Heartbeat ?
Heartbeat 本身是透過 socket 去實作, 所以每個OSD都會開3個port, 分別給下列三者使用:

+ **MON/Clinet:**  跟MON 溝通和 Client R/W object 
+ **OSD:** OSD之間傳送複本使用
+ **Heartbeat:** 這個 port 才是專們給 MON 和 OSD 之間做 heartbeat 使用
