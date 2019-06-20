---
title: PowerShell ile birden fazla Azure VM'yi yedekleme
description: Azure PowerShell kullanarak bir kurtarma Hizmetleri kasasına birden çok Azure VM'lerin yedeklenmesi Bu öğretici ayrıntıları.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 03/05/2019
ms.author: dacurwin
ms.custom: mvc
ms.openlocfilehash: 7cbe2cca37ce237409042e40b4a60311aed2446c
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273997"
---
# <a name="back-up-azure-vms-with-powershell"></a>PowerShell ile Azure Vm'lerini yedekleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticide nasıl dağıtılacağını açıklar bir [Azure Backup](backup-overview.md) PowerShell kullanarak birden fazla Azure sanal makinelerini yedeklemek için kurtarma Hizmetleri kasası.  

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir kurtarma Hizmetleri kasası oluşturun ve kasa bağlamını ayarlayın.
> * Yedekleme ilkesi tanımlama
> * Birden çok sanal makineyi korumak için yedekleme ilkesini uygulama
> * Tetikleyici, önce korumalı sanal makineler için bir isteğe bağlı yedekleme işi yedekleyebilir (veya korumak) bir sanal makine, tamamlamalısınız [önkoşulları](backup-azure-arm-vms-prepare.md) sanal makinelerinizi korumak için ortamınızı hazırlamak için. 

> [!IMPORTANT]
> Bu öğretici, önceden bir kaynak grubu ve bir Azure sanal makinesi oluşturmuş olduğunuzu varsayar.


## <a name="log-in-and-register"></a>Oturum açma ve kaydetme


1. `Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

    ```powershell
    Connect-AzAccount
    ```
2. İlk kez kullandığınız Azure Backup, Azure kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz gerekir [Register-AzResourceProvider](/powershell/module/az.Resources/Register-azResourceProvider). Zaten kayıtlı, bu adımı atlayın.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

A [kurtarma Hizmetleri kasası](backup-azure-recovery-services-vault-overview.md) Azure Vm'leri gibi korunan kaynakları için yedekleme verilerini depolayan bir mantıksal kapsayıcıdır. Bir yedekleme işi çalıştığında kurtarma Hizmetleri kasasının içinde bir kurtarma noktası oluşturur. Daha sonra bu kurtarma noktalarından birini kullanarak verileri dilediğiniz zaman geri yükleyebilirsiniz.


- Bu öğreticide, kasa ile aynı kaynak grubunda ve konumda yedeklemek istiyorsanız VM oluşturun.
- Azure yedekleme, yedeklenen veriler için depolamayı otomatik olarak işler. Kasa varsayılan olarak kullandığı [coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy-grs.md). Coğrafi yedeklilik sağlar yedeklediğiniz verileri, bir ikincil Azure bölgesine yüzlerce mil uzaktaki birincil bölgeye çoğaltılır.

Kasa şu şekilde oluşturun:

1. Kullanım [yeni AzRecoveryServicesVault](/powershell/module/az.recoveryservices/new-azrecoveryservicesvault)kasası oluşturmak için. Yedeklemek istediğiniz VM konumunu ve kaynak grubu adı belirtin.

    ```powershell
    New-AzRecoveryServicesVault -Name myRSvault -ResourceGroupName "myResourceGroup" -Location "EastUS"
    ```
2. Çoğu Azure Backup cmdlet’i, girdi olarak Kurtarma Hizmetleri kasasını gerektirir. Bu nedenle, Yedekleme Kurtarma Hizmetleri kasasının bir değişkende depolanması uygundur.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault –Name myRSVault
    ```
    
3. Kasa bağlamını ayarlayın [kümesi AzRecoveryServicesVaultContext](/powershell/module/az.RecoveryServices/Set-azRecoveryServicesVaultContext).

   - Kasa bağlamı, kasada korunan veri türüdür.
   - Bağlamı ayarlandıktan sonra sonraki tüm cmdlet'ler için geçerlidir.

     ```powershell
     Get-AzRecoveryServicesVault -Name "myRSVault" | Set-AzRecoveryServicesVaultContext
     ```

## <a name="back-up-azure-vms"></a>Azure VM'lerini yedekleme

Yedekleme ilkesinde belirtilen zamanlamaya uygun olarak yedekleme çalıştırma. Bir Kurtarma Hizmetleri kasası oluşturduğunuzda bu, varsayılan koruma ve saklama ilkeleri ile birlikte gelir.

- Varsayılan koruma İlkesi, belirtilen bir zamanda günde bir kez yedekleme işini tetikler.
- Varsayılan saklama ilkesi, 30 gün boyunca günlük kurtarma noktasını korur. 

Bu öğreticide Azure VM'yi yedekleme ve etkinleştirmek için biz aşağıdakileri yapın:

1. Yedekleme verilerinizi barındıran kasa içinde bir kapsayıcı belirtin [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-Azrecoveryservicesbackupcontainer).
2. Her VM yedekleme için bir öğedir. Bir yedekleme işi başlatmak için VM hakkında bilgi edinmek [Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem).
3. Geçici bir yedekleme ile çalıştırmak[yedekleme AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/backup-Azrecoveryservicesbackupitem). 
    - İlk ilk yedekleme işi tam kurtarma noktası oluşturur.
    - İlk yedekleme her yedekleme işinden artımlı kurtarma noktaları oluşturur.
    - Yalnızca son yedekleme sonrasında yapılan değişiklikleri aktardığından artımlı kurtarma noktaları depolama alanı ve süre açısından verimlilik sağlar.

Etkinleştirme ve yedekleme gibi çalıştırın:

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered -FriendlyName "V2VM"
$item = Get-AzRecoveryServicesBackupItem -Container $namedContainer -WorkloadType AzureVM
$job = Backup-AzRecoveryServicesBackupItem -Item $item
```

## <a name="troubleshooting"></a>Sorun giderme 

Sanal makinenizi yedeklerken sorunlarla karşılaşırsanız çalıştırırsanız, bilgileri gözden geçirdikten [sorunlarını giderme makalesine](backup-azure-vms-troubleshoot.md).

### <a name="deleting-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası silme

Bir kasayı silme gerekiyorsa, önce kasadaki kurtarma noktalarını silmeniz ve kasanın, şu şekilde kaydını:


```powershell
$Cont = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered
$PI = Get-AzRecoveryServicesBackupItem -Container $Cont[0] -WorkloadType AzureVm
Disable-AzRecoveryServicesBackupProtection -RemoveRecoveryPoints $PI[0]
Unregister-AzRecoveryServicesBackupContainer -Container $namedContainer
Remove-AzRecoveryServicesVault -Vault $vault1
```

## <a name="next-steps"></a>Sonraki adımlar

- [Gözden geçirme](backup-azure-vms-automation.md) daha ayrıntılı bir kılavuz yedekleme ve PowerShell ile Azure Vm'lerini geri yükleme. 
- [Azure Vm'leri yönetme ve izleme](backup-azure-manage-vms.md)
- [Azure Vm'lerini geri yükleme](backup-azure-arm-restore-vms.md)
