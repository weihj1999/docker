# 微软Azure云调研

## 简介
### 整体介绍

1. 微软Azure Storage
- A cloud-based service for persisting and retrieving data
- in the Azure portal, it is called a storage account.

2. Storage account types
- Gerneral-purpose storage accounts
  - Premium (SSD)
  - Standard
- Blob storage account
  - access tiers : Hot, Cool, and Archive

| Storage Account Type | Supported Services | Supported Performance Tiers | Supported Access Tiers | Replication Options | Deployment Model | Encryption |
| --- | --- | --- | --- | --- | --- | --- |
| GP-V2 | Blob, File, Queue, Table, and Disk | Standard, Premium | Hot, Cool, archive | LRS, ZRS, GRS, RA-GRS | Resource Manager | Encrypted |
| GP-V1 | Blob, File, Queue, Table, and Disk | Standard, Premium | N/A | LRS, ZRS, GRS, RA-GRS | Resource Manager， Classisc | Encrypted |
| Blob Storage | Blob(block blobs and append blobs only) | Standard | Hot, Cool, archive | LRS, ZRS, GRS, RA-GRS | Resource Manager | Encrypted |

>GP2 means gerneral perpose<br>
LRS: locally-redundant storage<br>
ZRS: Zone-redundant storage<br>
GRS: Geo-redundant storage<br>
RA-GRA: Read-access geo-redundant Storage


###  Azure Storage for Developers

- Blobs
- Tables (Storage)
- Queues
- Files (for sharing and migrating)
- Disk (for virtual machine)

1. Azure Storage Blobs
- Scalable storage for unstructured data
- Tiers: Hot， Cool， and  Archive

2. Azure Storage Tables
- No SQL, Schemaless, key-value store
- Cosmos DB adds data models (for example, "Column-Family", documents, graph)
- Cosmos DB provides API's compatible with DocumentDB, MongoDB, Table API, Graph(Gremlin） API, and more coming soon.
- Cosmos DB table API standard is equivalent to Azure Table storage.

3. Azure Storage Queues
- Messaging for applicaitons
- Enqueue and dequeue semantics
- Parallel clients
- Decouple components

4. Azure Storage Files
- SMB, file or folder share, network share
- Net use or linux equivalent
- Also REST API
- Built on top of blobs and tables

5. Azure Storage pages Blobs (disks)
- The virtual disks used by VM (Iaas)
- Classic/unmanaged or managed
- Premium(SSD) or Standard (traditional spinning platter)
- Replicated

6. Azure Storage, Azure SQL, Azure Redis
- Azure Storage is a foundational storage service for many applications and services

7. Pricing
- Storage type
- Redundancy
- Region
- Size (per GB)
- Access type and operation
- Bandwidth billed seperately


## Create a Stroage account

![Create a Storage Account](../images/create-storage-account01.png)

![Create a Storage Account](../images/create-storage-account02.png)

![Create a Storage Account](../images/create-storage-account03.png)

![Create a Storage Account](../images/create-storage-account04.png)

![Create a Storage Account](../images/create-storage-account05.png)






