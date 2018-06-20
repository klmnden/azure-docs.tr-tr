---
title: Azure sanal makinelerini uygun ölçekte yedekleme
description: Bu öğreticide, birden fazla Azure sanal makinelerinin bir Kurtarma Hizmetleri kasasına yedeklenmesi işlemi açıklanmaktadır.
services: backup
author: markgalioto
manager: carmonm
keywords: sanal makine yedeklemesi; sanal makineyi yedekleme; yedekleme ve olağanüstü durum kurtarma
ms.service: backup
ms.topic: tutorial
ms.date: 09/06/2017
ms.author: trinadhk
ms.custom: mvc
ms.openlocfilehash: 863960e012a8e345434459ad16526c8971f00b6b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34607013"
---
# <a name="back-up-azure-virtual-machines-in-azure-at-scale"></a>Azure sanal makinelerini uygun ölçekte yedekleme

Bu öğreticide, Azure sanal makinelerinin bir Kurtarma Hizmetleri kasasına yedeklenmesi işlemi açıklanmaktadır. Sanal makineleri yedekleme işleminin çoğunu hazırlık oluşturur. Bir sanal makineyi yedekleyebilmeniz (veya koruyabilmeniz) için önce sanal makinelerinizi korumak için ortamınızı hazırlamaya yönelik [önkoşulları](backup-azure-arm-vms-prepare.md) tamamlamanız gerekir. 

> [!IMPORTANT]
> Bu öğretici, önceden bir kaynak grubu ve bir Azure sanal makinesi oluşturmuş olduğunuzu varsayar.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

[Kurtarma Hizmetleri kasası](backup-azure-recovery-services-vault-overview.md), yedeklenmekte olan öğeler için kurtarma noktalarını tutan bir kapsayıcıdır. Kurtarma Hizmetleri kasası, Azure kaynak grubunun parçası olarak dağıtılabilen ve yönetilebilen bir Azure kaynağıdır. Bu öğreticide, korunmakta olan sanal makineyle aynı kaynak grubunda bir Kurtarma Hizmetleri kasası oluşturursunuz.


Azure Backup’ı ilk kullandığınızda, Azure Kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz gerekir. Sağlayıcıyı önceden aboneliğinize kaydettiyseniz sonraki adıma gidin.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices
```

**New-AzureRmRecoveryServicesVault** cmdlet'iyle bir Kurtarma Hizmetleri kasası oluşturun. Yedeklemek istediğiniz sanal makine yapılandırılırken kullanılan kaynak grubu adını ve konumunu belirttiğinizden emin olun. 

```powershell
New-AzureRmRecoveryServicesVault -Name myRSvault -ResourceGroupName "myResourceGroup" -Location "EastUS"
```

Çoğu Azure Backup cmdlet’i, girdi olarak Kurtarma Hizmetleri kasasını gerektirir. Bu nedenle, Yedekleme Kurtarma Hizmetleri kasasının bir değişkende depolanması uygundur. Daha sonra **Set-AzureRmRecoveryServicesBackupProperties** komutunu kullanarak **-BackupStorageRedundancy** seçeneğini [Coğrafi Olarak Yedekli Depolama (GRS)](../storage/common/storage-redundancy-grs.md) olarak ayarlayın. 

```powershell
$vault1 = Get-AzureRmRecoveryServicesVault –Name myRSVault
Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
```

## <a name="back-up-azure-virtual-machines"></a>Azure sanal makinelerini yedekleme

İlk yedeklemeyi çalıştırabilmeniz için önce kasa bağlamını ayarlamanız gerekir. Kasa bağlamı, kasada korunan veri türüdür. Bir Kurtarma Hizmetleri kasası oluşturduğunuzda bu, varsayılan koruma ve saklama ilkeleri ile birlikte gelir. Varsayılan koruma ilkesi, her gün belirtilen saatte bir yedekleme işini tetikler. Varsayılan saklama ilkesi, 30 gün boyunca günlük kurtarma noktasını korur. Bu öğretici için varsayılan ilkeyi kabul edin. 

Kasa bağlamını ayarlamak için **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** komutunu kullanın. Kasa bağlamı ayarlandıktan sonra, sonraki tüm cmdlet’ler için geçerli olur. 

```powershell
Get-AzureRmRecoveryServicesVault -Name myRSVault | Set-AzureRmRecoveryServicesVaultContext
```

Yedekleme işini tetiklemek için **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)** komutunu kullanın. Yedekleme işi bir kurtarma noktası oluşturur. Bu ilk yedekleme ise, kurtarma noktası tam bir yedeklemedir. Sonraki yedeklemeler, artımlı bir kopya oluşturur.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered -FriendlyName "V2VM"
$item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType AzureVM
$job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
```

## <a name="delete-the-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme

Bir Kurtarma Hizmetleri kasasını silmek için önce kasadaki kurtarma noktalarını silmeniz ve kasanın kaydını kaldırmanız gerekir. Aşağıdaki komutlar bu adımları açıklamaktadır. 


```powershell
$Cont = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered
$PI = Get-AzureRmRecoveryServicesBackupItem -Container $Cont[0] -WorkloadType AzureVm
Disable-AzureRmRecoveryServicesBackupProtection -RemoveRecoveryPoints $PI[0]
Unregister-AzureRmRecoveryServicesBackupContainer -Container $namedContainer
Remove-AzureRmRecoveryServicesVault -Vault $vault1
```

## <a name="troubleshooting-errors"></a>Hatalarda sorun giderme
Sanal makinenizi yedeklerken sorunlarla karşılaşırsanız yardım için [Azure sanal makinelerini yedekleme sorunlarını giderme makalesine](backup-azure-vms-troubleshoot.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinelerinizi koruduğunuza göre şimdi yönetim görevleri hakkında bilgi edinmek ve bir kurtarma noktasından sanal makinelerin nasıl geri yükleneceğini öğrenmek için aşağıdaki makalelere bakın.

* Yedekleme ilkesini değiştirmek için bkz. [Sanal makineleri yedeklemek için AzureRM.RecoveryServices.Backup cmdlet’lerini kullanma](backup-azure-vms-automation.md#create-a-protection-policy).
* [Sanal makinelerinizi yönetme ve izleme](backup-azure-manage-vms.md)
* [Sanal makineleri geri yükleme](backup-azure-arm-restore-vms.md)
