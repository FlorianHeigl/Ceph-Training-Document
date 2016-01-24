

#Ceph I/O

##Introduction
Ceph client interfaces read data from and write data to the Ceph storage cluster. Clients need the following data to communicate with the Ceph storage cluster:

* The Ceph configuration file, or the cluster name (usually ceph) and monitor address
* The pool name
* The user name and the path to the secret key.

Ceph clients maintain object IDs and the pool name(s) where they store the objects, but they do ***not need to maintain an object-to-OSD index or communicate with a centralized object index to look up data object* locations.** To store and retrieve data, Ceph clients access a Ceph monitor and retrieve the latest copy of the storage cluster map. Then, ***Ceph clients can provide an object name and pool name***, and Ceph will use the cluster map and the CRUSH (Controlled Replication Under Scalable Hashing) algorithm to compute the object placement group and the primary Ceph OSD for storing or retrieving data. The Ceph client connects to the primary OSD where it may perform read and write operations. There is no intermediary server, broker or bus between the client and the OSD.

When an OSD stores data, it receives data from a Ceph client—***​whether the client is a Ceph Block Device, a Ceph Object Gateway or another interface—​and it stores the data as an object.*** Each object corresponds to a file in a filesystem, which is stored on a storage device such as a hard disk. Ceph OSDs handle the read/write operations on the storage device.

![enter image description here](https://lh3.googleusercontent.com/-RmqV62KceWI/VqQ1qI7TT6I/AAAAAAAAChc/8vKDzAQRmSU/s0/Image.png "OSD3.png")

> **NOTE:** An object ID is unique across the entire cluster, not just the local filesystem.

Ceph OSDs store all data as objects in a **flat namespace (e.g., no hierarchy of directories).** An object has a cluster-wide unique identifier, binary data, and metadata consisting of a set of name/value pairs. The semantics are completely up to Ceph clients. For example, the ***Ceph block device maps a block device image to a series of objects stored across the cluster.***

> **NOTE:** 一個 RBD image 對 Ceph 來說其實只是一連串的 object而已 [XX]

![enter image description here](https://access.redhat.com/webassets/avalon/d/Red_Hat_Ceph_Storage-1.3-Red_Hat_Ceph_Architecture-en-US/images/diag-7f5336995654f25b757e87e4e588065e.png)


ref: https://access.redhat.com/documentation/en/red-hat-ceph-storage/version-1.3/red-hat-ceph-storage-13-red-hat-ceph-architecture/


##Data Striping
http://docs.ceph.com/docs/hammer/architecture/#data-striping

Ceph 為了增加硬碟的吞吐量(throughput)和效能採用了類似 RAID0 的 data strip 的方法, 將資料切割後分散到其他硬碟來增加吞吐量, 並且利用複本(replication) 來解決RAID 0 在單一硬碟壞掉時無法將資料回復的問題,  一般解決這個問題是採用 RAID 0+1 但是 Ceph 在這裡可以做到 RAID 0 + n  (n = replica size)

* **Increase Throughput**  --> RAID 0 
* **Increase Reliability** --> n-way RAID mirroring  (n = replica size)

### RAID 0
![enter image description here](https://lh3.googleusercontent.com/-nR-EI1gG5pc/VqSzFm7qdaI/AAAAAAAACi8/MScQ8im7VrI/s0/Image.png "RAID0.png")

### RAID 0+1
![enter image description here](https://lh3.googleusercontent.com/-U43dpagEyvY/VqS4YI3oo4I/AAAAAAAACjU/2fQSIup6PvY/s0/Image.png "RAID01.png")

### Ceph Data Striping
> Ceph provides three types of clients: Ceph Block Device, Ceph Filesystem, and Ceph Object Storage. **A Ceph Client converts its data from the representation format it provides to its users (a block device image, RESTful objects, CephFS filesystem directories) into objects** for storage in the Ceph Storage Cluster.

Client 將資料切成多個 object 之後透過 `LIBRADOS` 進行 striping 和 parallel I/O, 這些都是在 client 端去完成


Ceph 先將資料經過 strip 切成好幾個連續的 strip unit 0 ~ 15, 而這切連續的 strip unit 又會組成好幾個 object (object 0 ~ 3) , 這些object 的集合又稱為 object set,  對應關係如下圖

![enter image description here](https://lh3.googleusercontent.com/-l2ss35CHS-o/VqS_FnCmGtI/AAAAAAAACjs/w1WRk4N79OM/s0/Image.png "strip.png")

Three important variables determine how Ceph stripes data:

* **object size : ** the maximum object size of the stored objects
>通常可以設定2MB 或 4MB, Ceph 預設是 4MB, 因為較大的 object size 可以包含更多個 strip unit

* **Stripe width : ** the size of a stripe unit
> 最小為 64kb  且 stripe width 要能夠被 object size 整除

* **Stripe count :** the number of stripe units read/written concurrently, aka the number of stripe units in a stripe, aka the number of objects in an object set



