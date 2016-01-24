

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
