微软Azure云调研

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
