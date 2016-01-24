

#Ceph Client

##Introduction
![enter image description here](https://lh3.googleusercontent.com/-ezLlfTyA23E/VqRGBA8tIeI/AAAAAAAACh0/3rW2kQE8jbc/s0/Image.png "ceph_client1.png")

Ceph 提供了有三種儲存介面:

1. Object Storage 

2. Block Storage

3. Filesystem

無論哪一種儲存方法都是透過 RADOS 所提供的介面去跟 RADOS 做溝通, 這層介面就是 **LIBRADOS**

>**NOTE:** 講 RADOS 這名詞可能有點不直覺, 可以把他想成就是整個 Ceph Storage Cluster 就好, 裡面包含了 OSD 和 MON 

## LIBRADOS

![enter image description here](https://lh3.googleusercontent.com/-Viq81QpeeNs/VqRIVd7YsAI/AAAAAAAACiM/MBY485HU8GI/s0/Image.png "RADOS.png")

要透過 `librados` 對 Ceph Storage Cluster 做操作之前必須有下列東西:
* The Ceph configuration file (include monitor address)
* The pool name
* The user name and secret key.

就可以直接透過 LIBRADOS 對 MON 跟 OSD 做溝通！！目前 LIBDRADO 提供  C/C++, Python, JAVA, PHP 這幾種API

![enter image description here](https://lh3.googleusercontent.com/-aJtziB97-j8/VqSQpJnWQCI/AAAAAAAACik/b9W2ty7ctEc/s0/Image.png "librados.png")

### Sample Code (Python)
```
import rados, sys

#Create Handle Examples.
cluster = rados.Rados(conffile = 'ceph.conf', conf = dict(keyring = '/etc/ceph/ceph.client.admin.keyring'))
cluster.connect()

#get cluster status
cluster_stats = cluster.get_cluster_stats()

#list pools 
pools = cluster.list_pools()

#create pool: test_pool
cluster.create_pool('test_pool')

#Reading from and writing to the Ceph Storage Cluster requries an input/output context (ioctx).
#create a input/output context for test pool
ioctx = cluster.open_ioctx('test_pool')

#Writing object 'hw' with contents 'Hello World!' to pool 'test_pool'."
ioctx.write_full("hw", "Hello World!")

#Read content of object 'hw'
ioctx.read("hw")

#list objects of 'test_pool'
object_iterator = ioctx.list_objects()

#close connection
ioctx.close()
```

## Data Flow
