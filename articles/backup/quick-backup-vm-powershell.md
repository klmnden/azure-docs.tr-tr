---
title: Azure Hızlı Başlangıcı - PowerShell ile sanal makine yedekleme
description: Azure PowerShell ile sanal makinelerinizi nasıl yedekleyeceğinizi öğrenin
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 03/05/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: c5a31e8520c0eaeb842fcb359c965e112fb0a20f
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576973"
---
# <a name="back-up-a-virtual-machine-in-azure-with-powershell"></a>PowerShell ile Azure'daki bir sanal makineyi yedekleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure PowerShell AZ](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.4.0) modülü oluşturun ve komut satırından veya betiklerdeki Azure kaynaklarını yönetmek için kullanılır. 

[Azure yedekleme](backup-overview.md) şirket içi makineleri ve uygulamaları ve Azure Vm'leri yedekler. Bu makalede AZ modülü ile bir Azure VM'yi yedekleme gösterilmektedir. Alternatif olarak, bir VM kullanarak yedekleyebilirsiniz [Azure CLI](quick-backup-vm-cli.md), veya [Azure portalında](quick-backup-vm-portal.md).

Bu hızlı başlangıç belgesi var olan bir Azure VM'de yedeklemeyi etkinleştirir. Bir sanal makine oluşturmanız gerekiyorsa [Azure PowerShell ile sanal makine oluşturabilirsiniz](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json).

Bu hızlı başlangıçta, Azure PowerShell modülü sürüm 1.0.0 AZ gerektirir veya üzeri. Sürümü bulmak için ` Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-az-ps).


## <a name="log-in-and-register"></a>Oturum açma ve kaydetme

1. `Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

    ```powershell
    Connect-AzAccount
    ```
2. İlk kez kullandığınız Azure Backup, Azure kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz gerekir [Register-AzResourceProvider](/powershell/module/az.Resources/Register-azResourceProvider)gibi:

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

A [kurtarma Hizmetleri kasası](backup-azure-recovery-services-vault-overview.md) Azure Vm'leri gibi korunan kaynakları için yedekleme verilerini depolayan bir mantıksal kapsayıcıdır. Bir yedekleme işi çalıştığında kurtarma Hizmetleri kasasının içinde bir kurtarma noktası oluşturur. Daha sonra bu kurtarma noktalarından birini kullanarak verileri dilediğiniz zaman geri yükleyebilirsiniz.

Kasayı oluşturduğunuzda:

- Kaynak grubu ve konum için kaynak grubunu ve yedeklemek istediğiniz VM'nin konumu belirtin.
- Bu kullandıysanız [örnek komut dosyası](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) VM oluşturmak için kaynak grubudur **myResourceGroup**, VM ***myVM**, ve kaynaklar **WestEurope**  bölge.
- Azure yedekleme, yedeklenen veriler için depolamayı otomatik olarak işler. Kasa varsayılan olarak kullandığı [coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy-grs.md). Coğrafi yedeklilik sağlar yedeklediğiniz verileri, bir ikincil Azure bölgesine yüzlerce mil uzaktaki birincil bölgeye çoğaltılır.

Artık bir kasa oluşturun.


1. Kullanım [yeni AzRecoveryServicesVault](/powershell/module/az.recoveryservices/new-azrecoveryservicesvault)kasası oluşturmak için:

    ```powershell
    New-AzRecoveryServicesVault `
        -ResourceGroupName "myResourceGroup" `
        -Name "myRecoveryServicesVault" `
    -Location "WestEurope"
    ```

2. Kasa bağlamını ayarlayın [kümesi AzRecoveryServicesVaultContext](/powershell/module/az.RecoveryServices/Set-azRecoveryServicesVaultContext)gibi:

    ```powershell
    Get-AzRecoveryServicesVault `
        -Name "myRecoveryServicesVault" | Set-AzRecoveryServicesVaultContext
    ```

3. Kasayla birlikte depolama yedekliliği yapılandırmasını (GRS/LRS) değiştirmek [kümesi AzRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/module/az.recoveryservices/Set-AzRecoveryServicesBackupProperties?view=azps-1.6.0)gibi:
    
    ```powershell
    Get-AzRecoveryServicesVault `
        -Name "myRecoveryServicesVault" | Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant/GeoRedundant
    ```
    > [!NOTE]
    > Depolama Yedekliliği kasaya korumalı hiç yedekleme öğesi yoksa değiştirilebilir.

## <a name="enable-backup-for-an-azure-vm"></a>Bir Azure sanal makinesi için yedeklemeyi etkinleştirme

Bir Azure VM için yedeklemeyi etkinleştirme ve bir yedekleme İlkesi belirtin.

- İlke, yedeklemeler çalıştırdığınızda ve yedeklemeler tarafından oluşturulan kurtarma noktalarının ne kadar süre tutulacağını tanımlar.
- Varsayılan koruma İlkesi, bir yedekleme, VM için günde bir kez çalıştırır ve oluşturulan kurtarma noktalarını 30 gün boyunca tutar. Bu varsayılan ilke, sanal makinenize hızlı bir şekilde korumak için kullanabilirsiniz. 

Yedekleme aşağıdaki gibi etkinleştirin:

1. İlk olarak, varsayılan ilkeyi ayarlayın [Get-AzRecoveryServicesBackupProtectionPolicy](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy):

    ```powershell
    $policy = Get-AzRecoveryServicesBackupProtectionPolicy     -Name "DefaultPolicy"
    ```

2. İle VM yedeklemeyi etkinleştirme [etkinleştir AzRecoveryServicesBackupProtection](/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection). İlke, kaynak grubu ve VM adını belirtin.

    ```powershell
    Enable-AzRecoveryServicesBackupProtection `
        -ResourceGroupName "myResourceGroup" `
        -Name "myVM" `
        -Policy $policy
    ```


## <a name="start-a-backup-job"></a>Bir yedekleme işi başlatma

Yedekleme ilkesinde belirtilen zamanlamaya uygun olarak yedekleme çalıştırma. Ayrıca, geçici bir yedekleme çalıştırabilirsiniz:

- İlk ilk yedekleme işi tam kurtarma noktası oluşturur.
- İlk yedekleme her yedekleme işinden artımlı kurtarma noktaları oluşturur.
- Yalnızca son yedekleme sonrasında yapılan değişiklikleri aktardığından artımlı kurtarma noktaları depolama alanı ve süre açısından verimlilik sağlar.

Geçici bir yedekleme çalıştırmak için kullandığınız[yedekleme AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem). 
- Yedekleme verilerinizi barındıran kasa içinde bir kapsayıcı belirttiğiniz [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupcontainer).
- Yedeklenecek her sanal makine ayrı bir öğe olarak ele alınır. Bir yedekleme işi başlatmak için VM hakkında bilgi edinmek [Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem).

Geçici bir yedekleme işi şu şekilde çalıştırın:

1. Kapsayıcıyı belirtin, VM bilgilerini almak ve yedeklemeyi çalıştırma.

    ```powershell
    $backupcontainer = Get-AzRecoveryServicesBackupContainer `
        -ContainerType "AzureVM" `
        -FriendlyName "myVM"

    $item = Get-AzRecoveryServicesBackupItem `
        -Container $backupcontainer `
        -WorkloadType "AzureVM"

    Backup-AzRecoveryServicesBackupItem -Item $item
    ```

2. İlk yedekleme işi tam kurtarma noktası oluşturduğundan 20 dakikaya kadar beklemeniz gerekebilir. İş, sonraki yordamda açıklandığı gibi izleyin.


## <a name="monitor-the-backup-job"></a>Yedekleme işini izleme

1. Çalıştırma [Get-AzRecoveryservicesBackupJob](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob) iş durumunu izlemek için.

    ```powershell
    Get-AzRecoveryservicesBackupJob
    ```
    Çıktı olarak gösteren aşağıdaki örneğe benzer **Inprogress**:

    ```
    WorkloadName   Operation         Status       StartTime              EndTime                JobID
    ------------   ---------         ------       ---------              -------                -----
    myvm           Backup            InProgress   9/18/2017 9:38:02 PM                          9f9e8f14
    myvm           ConfigureBackup   Completed    9/18/2017 9:33:18 PM   9/18/2017 9:33:51 PM   fe79c739
    ```

2. İş durumu olduğunda **tamamlandı**, sanal makine korunuyor ve tam kurtarma noktası depolanan sahiptir.


## <a name="clean-up-the-deployment"></a>Dağıtımı temizleme

Artık sanal makineyi yedeklemek istiyorsanız, bunu temizleyebilirsiniz.
- VM geri kullanıma denemek istiyorsanız, yedekleme temiz atlayın.
- Varolan bir VM'yi kullandıysanız, en son atlayabilirsiniz [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet'ini kaynak grubunu ve sanal makineyi bırakabilirsiniz.

Korumayı devre dışı bırakın, kasa ve geri yükleme noktalarını kaldırın. Ardından, kaynak grubunu ve sanal makine kaynaklarıyla ilişkilendirilmiş, aşağıda belirtildiği gibi silin:

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $item -RemoveRecoveryPoints
$vault = Get-AzRecoveryServicesVault -Name "myRecoveryServicesVault"
Remove-AzRecoveryServicesVault -Vault $vault
Remove-AzResourceGroup -Name "myResourceGroup"
```


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Kurtarma Hizmetleri kasası oluşturdunuz, bir sanal makine için koruma özelliklerini etkinleştirdiniz ve ilk kurtarma noktasını oluşturdunuz. 

- [Bilgi nasıl](tutorial-backup-vm-at-scale.md) Vm'leri Azure portalında yedekleme için.
- [Bilgi nasıl](tutorial-restore-disk.md) hızlıca bir VM'yi geri yüklemek için
