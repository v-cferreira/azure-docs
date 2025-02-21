---
title: Configure storage policy
description:  Learn how to configure storage policy for your Azure VMware Solution virtual machines.
ms.topic: how-to
ms.date: 08/31/2021

#Customer intent: As an Azure service administrator, I want set the vSAN storage policies to determine how storage is allocated to the VM.

---

# Configure storage policy

vSAN storage policies define storage requirements for your virtual machines (VMs). These policies guarantee the required level of service for your VMs because they determine how storage is allocated to the VM. Each VM deployed to a vSAN datastore is assigned at least one VM storage policy.

You can assign a VM storage policy in an initial deployment of a VM or when you do other VM operations, such as cloning or migrating. Post-deployment cloudadmin users or equivalent roles can't change the default storage policy for a VM. However, **VM storage policy** per disk changes is permitted. 

The Run command lets authorized users change the default or existing VM storage policy to an available policy for a VM post-deployment. There are no changes made on the disk-level VM storage policy. You can always change the disk level VM storage policy as per your requirements.


>[!NOTE]
>Run commands are executed one at a time in the order submitted.


In this how-to, you learn how to:

> [!div class="checklist"]
> * List all storage policies
> * Set the storage policy for a VM
> * Specify default storage policy for a cluster



## Prerequisites

Make sure that the [minimum level of hosts are met](https://docs.vmware.com/en/VMware-Cloud-on-AWS/services/com.vmware.vsphere.vmc-aws-manage-data-center-vms.doc/GUID-EDBB551B-51B0-421B-9C44-6ECB66ED660B.html).

|  **RAID configuration** | **Failures to tolerate (FTT)** | **Minimum hosts required** |
| --- | :---: | :---: |
| RAID-1 (Mirroring) <br />Default setting.  | 1  | 3  |
| RAID-5 (Erasure Coding)  | 1  | 4  |
| RAID-1 (Mirroring)  | 2  | 5  |
| RAID-6 (Erasure Coding)  | 2  | 6  |
| RAID-1 (Mirroring)  | 3  | 7  |


 

## List storage policies

You'll run the `Get-StoragePolicy` cmdlet to list the vSAN based storage policies available to set on a VM.

1. Sign in to the [Azure portal](https://portal.azure.com).

1. Select **Run command** > **Packages** > **Get-StoragePolicies**.

   :::image type="content" source="media/run-command/run-command-overview-storage-policy.png" alt-text="Screenshot showing how to access the storage policy run commands available." lightbox="media/run-command/run-command-overview-storage-policy.png":::

1. Provide the required values or change the default values, and then select **Run**.

   :::image type="content" source="media/run-command/run-command-get-storage-policy.png" alt-text="Screenshot showing how to list storage policies available. ":::
   
   | **Field** | **Value** |
   | --- | --- |
   | **Retain up to**  | Retention period of the cmdlet output. The default value is 60.  |
   | **Specify name for execution**  | Alphanumeric name, for example, **Get-StoragePolicies-Exec1**. |
   | **Timeout**  |  The period after which a cmdlet exits if taking too long to finish.  |

1. Check **Notifications** to see the progress.




## Set storage policy on VM

You'll run the `Set-AvsVMStoragePolicy` cmdlet to Modify vSAN based storage policies on an individual VM. 

>[!NOTE]
>You cannot use the vSphere Client to change the default storage policy or any existing storage policies for a VM. 

1. Select **Run command** > **Packages** > **Set-AvsVMStoragePolicy**.

1. Provide the required values or change the default values, and then select **Run**.

   | **Field** | **Value** |
   | --- | --- |
   | **VMName** | Name of the target VM. |
   | **StoragePolicyName** | Name of the storage policy to set. For example, **RAID-FTT-1**. |
   | **Retain up to**  | Retention period of the cmdlet output. The default value is 60.  |
   | **Specify name for execution**  | Alphanumeric name, for example, **changeVMStoragePolicy**.  |
   | **Timeout**  |  The period after which a cmdlet exits if taking too long to finish.  |

1. Check **Notifications** to see the progress.


## Specify storage policy for a cluster

You'll run the `Set-ClusterDefaultStoragePolicy` cmdlet to specify default storage policy for a cluster,

>[!NOTE]
>Changing the storage policy of the default management cluster (Cluster-1) isn't allowed. 

1. Select **Run command** > **Packages** > **Set-ClusterDefaultStoragePolicy**.

1. Provide the required values or change the default values, and then select **Run**.

   | **Field** | **Value** |
   | --- | --- |
   | **ClusterName** | Name of the cluster. |
   | **StoragePolicyName** | Name of the storage policy to set. For example, **RAID-FTT-1**. |
   | **Retain up to**  | Retention period of the cmdlet output. The default value is 60.  |
   | **Specify name for execution**  | Alphanumeric name, for example, **Set-ClusterDefaultStoragePolicy-Exec1**.  |
   | **Timeout**  |  The period after which a cmdlet exits if taking too long to finish.  |

1. Check **Notifications** to see the progress.



## Next steps

Now that you've learned how to configure vSAN storage policies, you can learn more about:

- [How to attach disk pools to Azure VMware Solution hosts (Preview)](attach-disk-pools-to-azure-vmware-solution-hosts.md) - You can use disks as the persistent storage for Azure VMware Solution for optimal cost and performance.

- [How to configure external identity for vCenter](configure-identity-source-vcenter.md) - vCenter has a built-in local user called cloudadmin and assigned to the CloudAdmin role. The local cloudadmin user is used to set up users in Active Directory (AD). With the Run command feature, you can configure Active Directory over LDAP or LDAPS for vCenter as an external identity source.
