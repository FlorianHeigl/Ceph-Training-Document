#OSD Hearbeat and Peering
##Hearbeat 的應用
Heartbeat 主要用於及時發現OSD的變化 (down/up), 並且通知Monitor去更新OSD MAP, 最後將最新版本的OSD MAP同步到相關的 OSD 上面.

OSD Heartbeat 的方法有兩種:
1. OSD <-> MON, OSD 本身會定期回報自身的狀態給 Monitor (default 是120s)
2. OSD <-> OSD, OSD 之間也會有 Heartbeat (default 是6s), 來監聽其他OSD是否有掛掉的情況發生, 並且通知 Monitor

#如何選擇 Heartbeat 的對象?
OSD 之間的 heartbeat 並不是去ping 所有的OSD, 這樣會消耗太多OSD的資源且對網路的負擔也很大. 
這裡是OSD只會對"本OSD所在的PG中, 所包含的其他OSD"去做heartbeat !!

Example:
假設有三個PG 他們分別對應的OSD如下:
PG 1.1 -> [OSD.1, OSD.5, OSD.2]
PG 1.2 -> [OSD.3, OSD.1, OSD.6]
PG 2.1 -> [OSD.2, OSD.1, OSD.9]

那OSD.1的heartbeat 對象就會為 OSD.2 OSD.5 OSD.3 OSD.6 OSD.9

