---
title: "Ölçekli azure'da Azure Vm'leri yedekleme | Microsoft Docs"
description: "Birden fazla Azure sanal makineleri bir kurtarma Hizmetleri kasasına yedekleme Bu öğretici ayrıntıları."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "sanal makine yedeklemesi; sanal makineyi geri; Yedekleme ve olağanüstü durum kurtarma"
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: 
ms.openlocfilehash: db4e1392acaeb2431d29a851113b7bc5a6dc1e9d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-azure-virtual-machines-in-azure-at-scale"></a>Azure Azure sanal makinelerinde ölçekte yedekleyin

Bu öğretici Azure sanal makineleri bir kurtarma Hizmetleri kasasına yedekleme konusunda ayrıntılı olarak açıklanmaktadır. Sanal makineleri yedekleme için işlerin çoğunu hazırlık olur. Geri yükleyebileceğiniz önce Yukarı (veya koruma) bir sanal makine, tamamlamanız gereken [Önkoşullar](backup-azure-arm-vms-prepare.md) Vm'lerinizi koruma için ortamınızı hazırlamak için. 

> [!IMPORTANT]
> Bu öğretici, bir kaynak grubu ve bir Azure sanal makinesi zaten oluşturmuş varsayar.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

A [kurtarma Hizmetleri kasası](backup-azure-recovery-services-vault-overview.md) yedeklenmekte olan öğeler için kurtarma noktalarını tutan bir kapsayıcıdır. Kurtarma Hizmetleri kasası dağıtılan ve bir Azure kaynak grubu bir parçası olarak yönetilen bir Azure kaynak değil. Bu öğreticide, korunan sanal makine ile aynı kaynak grubunda bir kurtarma Hizmetleri kasası oluşturun.


Azure Backup, ilk kullanışınızda Azure kurtarma hizmet sağlayıcısı, aboneliğiniz ile kaydetmeniz gerekir. Sağlayıcı aboneliğinizle kaydetmediyseniz, sonraki adıma gidin.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices
```

**New-AzureRmRecoveryServicesVault** cmdlet'iyle bir Kurtarma Hizmetleri kasası oluşturun. Yedeklemek istediğiniz sanal makineyi yapılandırırken kullandığınız konumunu ve kaynak grubu adı belirttiğinizden emin olun. 

```powershell
New-AzureRmRecoveryServicesVault -Name myRSvault -ResourceGroupName "myResourceGroup" -Location "EastUS"
```

Çok sayıda Azure yedekleme cmdlet'lerini girdi olarak kurtarma Hizmetleri kasası nesnesi gerektirir. Bu nedenle, bir değişkende yedekleme kurtarma Hizmetleri kasası nesne depolamak uygundur. Ardından **kümesi AzureRmRecoveryServicesBackupProperties** ayarlamak için **- BackupStorageRedundancy** için seçenek [coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). 

```powershell
$vault1 = Get-AzureRmRecoveryServicesVault –Name myRSVault
Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
```

## <a name="back-up-azure-virtual-machines"></a>Azure sanal makinelerini yedekleme

İlk yedekleme çalıştırmadan önce kasa bağlam ayarlamanız gerekir. Kasa bağlam kasaya korunan verileri türüdür. Kurtarma Hizmetleri kasası oluşturduğunuzda, varsayılan koruma ve bekletme ilkeleri ile birlikte gelir. Varsayılan koruma İlkesi, bir yedekleme işi her gün belirtilen zamanda tetikler. Varsayılan saklama İlkesi 30 gün boyunca günlük kurtarma noktası korur. Bu öğretici için varsayılan ilkesini kabul edin. 

Kullanım  **[kümesi AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  kasası bağlam ayarlamak için. Kasa bağlam ayarladıktan sonra sonraki tüm cmdlet'ler için geçerlidir. 

```powershell
Get-AzureRmRecoveryServicesVault -Name myRSVault | Set-AzureRmRecoveryServicesVaultContext
```

Kullanım  **[yedekleme AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  yedekleme işini tetikleyecek. Yedekleme işi, bir kurtarma noktası oluşturur. Ardından ilk yedekleme ise, kurtarma noktası bir tam yedekleme ' dir. Sonraki yedeklemelerin artımlı bir kopyasını oluşturun.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered -FriendlyName "V2VM"
$item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType AzureVM
$job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
```

## <a name="delete-the-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını Sil

Kurtarma Hizmetleri kasası silmek için önce bir kurtarma noktası kasadaki silin ve kasa kaydı silinemedi. Aşağıdaki komutları adımları ayrıntılı olarak açıklanmaktadır. 


```powershell
$Cont = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered
$PI = Get-AzureRmRecoveryServicesBackupItem -Container $Cont[0] -WorkloadType AzureVm
Disable-AzureRmRecoveryServicesBackupProtection -RemoveRecoveryPoints $PI[0]
Unregister-AzureRmRecoveryServicesBackupContainer -Container $namedContainer
Remove-AzureRmRecoveryServicesVault -Vault $vault1
```

## <a name="troubleshooting-errors"></a>Sorun giderme
Sanal makineniz yedekleme sırasında sorunları içine çalıştırırsanız bkz [makale sorun giderme Azure sanal makineleri yedekleyin](backup-azure-vms-troubleshoot.md) Yardım.

## <a name="next-steps"></a>Sonraki adımlar
Korunan, sanal makine, yönetim görevleri ve sanal makineleri bir kurtarma noktasından geri yükleme hakkında bilgi edinmek için aşağıdaki makalelere bakın.

* Yedekleme ilkesini değiştirmek için bkz: [sanal makineleri yedeklemek için kullanım AzureRM.RecoveryServices.Backup cmdlet'leri](backup-azure-vms-automation.md#create-a-protection-policy).
* [Sanal makinelerinizi yönetme ve izleme](backup-azure-manage-vms.md)
* [Sanal makineleri geri yükleme](backup-azure-arm-restore-vms.md)
