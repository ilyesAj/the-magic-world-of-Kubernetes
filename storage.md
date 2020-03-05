# Storage Concepts
## File VS Block Vs Object storage

The most traditional service type is shared filesystem, or simply “**file storage**”, which as the name implies offers to multiple clients the ability to access a single shared folder. The two most popular shared filesystem protocols in use today are NFS and SMB/CIFS.

**Block storage** provides network access to the equivalent of raw block devices. A client machine connects to a specific volume on the storage service and formats it as if it were a local block device. Multiple clients do not generally mount the same volume, but they may in master/slave high-availability configurations where the slave needs to be ready to take over the master. Block devices are usually exported over Fibre Channel, iSCSI or AoE (ATA over Ethernet).

**Object storage** is a relatively new storage type, designed for unstructured data such as media, documents, logs, backups, application binaries and VM images. Conceptually they are like a persistent key/value store; objects are usually submitted via a REST API call, and an identifier returned. Most object stores allow attaching metadata to objects, and aggregating them into containers (or buckets). Both Ceph and Swift offer object store interfaces. The most popular cloud object store is AWS S3, and many object store implementations are compatible with it, including Ceph’s S3 RADOS Gateway service.

![file vs block vs object Storage](assets/storage-505e9.png)

## stateful apps in kubernetes
stateful applications requires persistant storage to store data BUT :
- Containers are ephemeral : no way to persist data (when container crashes/terminates -> :skull: data  )
- Containers can't share data between each other

:rocket: for this we have : kubernetes with (volumes, persistent volumes, persistant volume claims, storageClasses ... )

# Storage in kubernetes
the most important kubernetes principles that we need to keep in mind are :
  - **Workload portability**
  - **Meet the user where they are**
## Volumes
### Volume plugins
#### In-tree plugins
#### CSI plugins
### Volume types
#### Ephemeral Storage
#### Remote storage
#### Local persistent Volume
#### Host path
#### Out-of-Tree
## Provisioning Volumes
### PV/PVC
#### persistent volume claim
#### persistent volume
### storageClass
# references
https://ubuntu.com/blog/what-are-the-different-types-of-storage-block-object-and-file
https://www.caringo.com/blog/back-basics-object-storage
https://www.redhat.com/en/topics/data-storage/file-block-object-storage
