# Storage Concepts
## File VS Block Vs Object storage

The most traditional service type is shared filesystem, or simply “**file storage**”, which as the name implies offers to multiple clients the ability to access a single shared folder. The two most popular shared filesystem protocols in use today are NFS and SMB/CIFS.
Disadvantage: file storage is tied to what type of data you will store


**Block storage** provides network access to the equivalent of raw block devices. A client machine connects to a specific volume on the storage service and formats it as if it were a local block device. Multiple clients do not generally mount the same volume, but they may in master/slave high-availability configurations where the slave needs to be ready to take over the master. Block devices are usually exported over Fibre Channel, iSCSI or AoE (ATA over Ethernet).
Disadvantage: data loose ?

**Object storage** is a relatively new storage type, designed for unstructured data such as media, documents, logs, backups, application binaries and VM images. Conceptually they are like a persistent key/value store; objects are usually submitted via a REST API call, and an identifier returned. Most object stores allow attaching Metadata to objects, and aggregating them into containers (or buckets). The main difference between the other concepts is that the objects are managed via the application itself that supports Object storage. That means that **no real file system is needed** here. This layer is obsolete. An application that uses Object storage sends a storage inquiry to the solution where to store the object. The object is then given an address inside the huge storage space and saved there **by the application itself**.  amazon s3 is an object storage.
Object storage is recommended for Non changed data
the biggest advantage compared to traditional storage : real estimation of data usage
Disadvantage: to update a file , you have to create a new one

![file vs block vs object Storage](assets/storage-505e9.png)

## Stateful apps in kubernetes
stateful applications requires persistant storage to store data BUT :
- Containers are ephemeral : no way to persist data (when container crashes/terminates -> :skull: data  )
- Containers can't share data between each other

:rocket: for this we have : kubernetes with (volumes, persistent volumes, persistant volume claims, storageClasses ... )

# Storage in kubernetes
the most important kubernetes principles that we need to keep in mind are :
  - **Workload portability**
  - **Meet the user where they are**

A kubernetes Volume is :
- Defined in the pod Spec
- Tied to the **Pod's Lifecycle** :
    - when a pod cease to exist , the volume cease to exist
    - Survive when Pod restart
    - Can be attached to any of the containers within the pod
- A pod can have one or more types of volumes attached to it
- Volumes can **not** mount onto other volumes or have hard links to other volumes

At its core, a volume is just a directory, possibly with some data in it, which is accessible to the Containers in a Pod.
**How that directory comes to be, the medium that backs it, and the contents of it are determined by the particular volume type used.**
### Volumes plugins
A Kubernetes Volume plugin extends the Kubernetes volume interface to support a block and/or file storage system.
two main types of plugins :

- In-tree plugins **[Deprecated]**: those plugins were built, linked, compiled, and shipped with the core Kubernetes binaries and extend the core Kubernetes API
- Out-of-tree plugins : are developed independently of the Kubernetes code base, and are deployed (installed) on Kubernetes clusters as extensions.
    - Out-of-tree FlexVolume driver [deprecated]
    - **Out-of-tree CSI driver [ primary volume plugin system for K8s]** : Container Storage Interface (CSI) is a standardized mechanism for Container Orchestration Systems (COs), including Kubernetes, to expose arbitrary storage systems to containerized workloads.
#### why In-tree plugins were deprecated ?

In-tree plugins are tightly coupled and dependent on kubernetes releases : they are hard to maintain and can cause crushes to the master components(Bugs in volume plugins affect critical Kubernetes components).in addition Source code have to be open source which can cause problem to storage vendors
sig-storage decided to :

- No new in-tree plugins are accepted
- existing plugins will be maintained until full migration to CSI plugins
- All new Volume plugin devloppment will be based on CSI  
## Volume types

### Remote storage
this type of storage is provided and maintained by a third-party storage vendor that exposes an API to create,access,update or delete volumes .
most of the remote storage plugins supports persisting data beyond a pod's lifecycle, they are referenced in pod either in-line( Not recommended for Workload portability ) or via PV/PVC
Examples:
- GCE Persistent Disk
- AWS Elastic Block Store
- Azure File Storage
- Azure Data Disk
- Dell EMC ScaleIO
- iSCSI
- Flocker
- NFS
- vSphere
- GlusterFS
- Ceph File and RBD
- Cinder
- Quobyte Volume
- FibreChannel
- VMware Photon PD
The advantage of remote storage is the fact that **storage and compute are decoupled** : If node is terminated or crashes (for Resources shortage for example) , the state of the application will be maintained regardless of the state of the node (that stores the data ) or the pod (scheduled in that node).

### Ephemeral Storage
Temp scratch file space from host machine(temporarily).this space is usually used to share data between containers within the same pod.
this type of storage can only be referenced 'in-line'in pod definition (not PV/PVC) and is loosely coupled to Tied to the **Pod's Lifecycle**
- EmptyDir
- Expose Kubernetes API ?
Built-in top of emptyDir:
  - Secret
  - ConfigMap
  - DownwardAPI

### Local persistent Volume

### Out-of-Tree
- Flex (exec a binary)
- CSI (Beta)
### Host path
## Provisioning Volumes
### PV/PVC
#### Persistent volume claim
#### Persistent volume
### StorageClass
# References
https://ubuntu.com/blog/what-are-the-different-types-of-storage-block-object-and-file
https://www.caringo.com/blog/back-basics-object-storage
https://www.redhat.com/en/topics/data-storage/file-block-object-storage
https://www.ontrack.com/blog/2018/02/22/the-evolution-of-storage-file-storage-vs-block-storage-vs-object-storage-part-1/
