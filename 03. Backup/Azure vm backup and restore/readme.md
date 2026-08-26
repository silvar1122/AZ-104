## Azure VM Backup and Restore
**Objective**
Configure Azure Backup for a virtual machine, create a recovery point, delete data from the VM, and restore the VM to verify that the backup works.

**Scenario**
You are an Azure administrator responsible for a production virtual machine called AZ104-VM01.
The company wants to protect the VM against accidental deletion or data loss. Your task is to configure Azure Backup and demonstrate that the VM can be recovered.

**Obstacle**
The VM currently has no backup configured.
If important files are accidentally deleted or the VM becomes corrupted, there is no recovery point available.

## Action
* **Step 1 — Create the Virtual Machine**
* **Step 2 — Create a Recovery Services Vault**
* **Step 3 — Configure VM Backup**
* **Step 4 — Create an On-Demand Backup**
* **Step 5 — Verify the Recovery Point**
* **Step 6 — Simulate Data Loss**
* **Step 7 — Restore the VM**
* **Step 8 — Verify the Restore**

## Result

The Azure VM was successfully protected using Azure Backup.
A Recovery Services vault was created, a backup policy was applied, and an on-demand recovery point was created.
After simulating accidental data loss, the VM was successfully restored from the recovery point and the deleted file was recovered.
