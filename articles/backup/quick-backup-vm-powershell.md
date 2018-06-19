---
title: Azure Hızlı Başlangıcı - PowerShell ile sanal makine yedekleme
description: Azure PowerShell ile sanal makinelerinizi nasıl yedekleyeceğinizi öğrenin
services: backup
author: markgalioto
manager: carmonm
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 2/14/2018
ms.author: markgal
ms.custom: mvc
ms.openlocfilehash: 4161b11e88d2b3201e18e095e13db864e25d7bfc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34607545"
---
# <a name="back-up-a-virtual-machine-in-azure-with-powershell"></a>PowerShell ile Azure'daki bir sanal makineyi yedekleme
Azure PowerShell modülü, komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Düzenli aralıklarla yedekleme yaparak verilerinizi koruyabilirsiniz. Azure Backup, coğrafi olarak yedekli kurtarma kasalarında saklanabilecek kurtarma noktaları oluşturur. Bu makalede Azure PowerShell modülüyle bir sanal makinenin nasıl yedekleneceği anlatılmaktadır. Bu adımları [Azure CLI](quick-backup-vm-cli.md) veya [Azure portalı](quick-backup-vm-portal.md) ile de gerçekleştirebilirsiniz.

Bu hızlı başlangıç belgesi var olan bir Azure VM'de yedeklemeyi etkinleştirir. Bir sanal makine oluşturmanız gerekiyorsa [Azure PowerShell ile sanal makine oluşturabilirsiniz](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json).

Bu hızlı başlangıç, Azure PowerShell modülü 4.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).


## <a name="log-in-to-azure"></a>Azure'da oturum açma
`Connect-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzureRmAccount
```

Azure Backup'ı ilk kullanımınızda [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) cmdlet'iyle Azure Kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz gerekir.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
```


## <a name="create-a-recovery-services-vaults"></a>Kurtarma hizmetleri kasası oluşturma
Kurtarma Hizmetleri kasası, Azure sanal makineleri gibi koruma altındaki kaynakların yedeklenen verilerini saklayan bir mantıksal kapsayıcıdır. Koruma altındaki bir kaynak için yedekleme işi çalıştığında Kurtarma Hizmetleri kasasının içinde bir kurtarma noktası oluşturulur. Daha sonra bu kurtarma noktalarından birini kullanarak verileri dilediğiniz zaman geri yükleyebilirsiniz.

[New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) cmdlet'iyle bir Kurtarma Hizmetleri kasası oluşturun. Korumak istediğiniz sanal makineyle aynı kaynak grubunu ve konumu belirtin. Sanal makinenizi oluşturmak için [örnek betiği](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) kullandıysanız kaynak grubu *myResourceGroup*, sanal makine ise *myVM* adına sahip olur ve kaynaklar *WestEurope* konumuna yerleştirilir.

```powershell
New-AzureRmRecoveryServicesVault `
    -ResourceGroupName "myResourceGroup" `
    -Name "myRecoveryServicesVault" `
    -Location "WestEurope"
```

Varsayılan olarak kasada Coğrafi Olarak Yedekli depolama özelliği etkindir. Bu depolama yedekliliği seviyesi verilerinizi daha fazla korumak için yedeklenen verilerinizin birincil bölgeden yüzlerce kilometre uzaktaki ikincil bir Azure bölgesinde çoğaltılmasını sağlar.

Sonraki adımlarda bu kasayı kullanmak için [Set-AzureRmRecoveryServicesVaultContext](/powershell/module/AzureRM.RecoveryServices/Set-AzureRmRecoveryServicesVaultContext) cmdlet'ini kullanarak kasa bağlamını ayarlayın

```powershell
Get-AzureRmRecoveryServicesVault `
    -Name "myRecoveryServicesVault" | Set-AzureRmRecoveryServicesVaultContext
```


## <a name="enable-backup-for-an-azure-vm"></a>Bir Azure sanal makinesi için yedeklemeyi etkinleştirme
Bir yedekleme işinin çalışma zamanını ve kurtarma noktalarının saklama süresini tanımlamak için ilke oluşturur ve kullanırsınız. Varsayılan koruma ilkesi yedekleme işini her gün çalıştırır ve kurtarma noktalarını 30 gün boyunca tutar. Sanal makinenizi hızlı bir şekilde koruma altına almak için bu varsayılan ilke değerlerini kullanabilirsiniz. İlk olarak [Get-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/AzureRM.RecoveryServices.Backup/Get-AzureRmRecoveryServicesBackupProtectionPolicy) cmdlet'iyle varsayılan ilkeyi ayarlayın:

```powershell
$policy = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "DefaultPolicy"
```

Bir sanal makinede yedekleme korumasını etkinleştirmek için [Enable-AzureRmRecoveryServicesBackupProtection](/powershell/module/AzureRM.RecoveryServices.Backup/Enable-AzureRmRecoveryServicesBackupProtection) cmdlet'ini kullanın. Kullanılacak ilkeyi seçtikten sonra korumaya alınacak kaynak grubunu ve sanal makineyi belirtin:

```powershell
Enable-AzureRmRecoveryServicesBackupProtection `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Policy $policy
```


## <a name="start-a-backup-job"></a>Bir yedekleme işi başlatma
Varsayılan ilkenin işi planlanan saatte başlatmasını beklemek yerine yedekleme işini hemen başlatmak için [Backup-AzureRmRecoveryServicesBackupItem](/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem) cmdlet'ini kullanın. İlk yedekleme işi tam kurtarma noktası oluşturur. Bu ilk yedekleme sonrasında çalıştırılan tüm yedekleme işleri artımlı kurtarma noktaları oluşturur. Yalnızca son yedekleme sonrasında yapılan değişiklikleri aktardığından artımlı kurtarma noktaları depolama alanı ve süre açısından verimlilik sağlar.

Aşağıdaki komut kümesinde [Get-AzureRmRecoveryServicesBackupContainer](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer) cmdlet'ini Kurtarma Hizmetleri kasasında yedekleme verilerinizi tutacak bir kapsayıcı belirtirsiniz. Yedeklenecek her sanal makine ayrı bir öğe olarak ele alınır. Bir yedekleme işi başlatmak için [Get-AzureRmRecoveryServicesBackupItem](/powershell/module/AzureRM.RecoveryServices.Backup/Get-AzureRmRecoveryServicesBackupItem) cmdlet'ini kullanarak sanal makine öğeniz hakkında bilgi edinin.

```powershell
$backupcontainer = Get-AzureRmRecoveryServicesBackupContainer `
    -ContainerType "AzureVM" `
    -FriendlyName "myVM"

$item = Get-AzureRmRecoveryServicesBackupItem `
    -Container $backupcontainer `
    -WorkloadType "AzureVM"

Backup-AzureRmRecoveryServicesBackupItem -Item $item
```

Bu ilk yedekleme işi tam kurtarma noktası oluşturduğundan 20 dakikaya kadar sürebilir.


## <a name="monitor-the-backup-job"></a>Yedekleme işini izleme
Yedekleme işlerinin durumunu izlemek için [Get-AzureRmRecoveryservicesBackupJob](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob) cmdlet'ini kullanın:

```powershell
Get-AzureRmRecoveryservicesBackupJob
```

Çıktı, yedekleme işinin **Sürüyor** durumda olduğunu gösteren aşağıdaki örneğe benzer olacaktır:

```
WorkloadName   Operation         Status       StartTime              EndTime                JobID
------------   ---------         ------       ---------              -------                -----
myvm           Backup            InProgress   9/18/2017 9:38:02 PM                          9f9e8f14
myvm           ConfigureBackup   Completed    9/18/2017 9:33:18 PM   9/18/2017 9:33:51 PM   fe79c739
```

Yedekleme işinin *Durum* bilgisi *Tamamlandı* olduğunda sanal makineniz Kurtarma Hizmetleri ile koruma altına alınmış ve kayıtlı tam kurtarma noktasına sahip olmuş olur.


## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Artık gerekli değilse sanal makine korumasını devre dışı bırakabilir, kurtarma noktalarını ve Kurtarma Hizmetleri kasasını kaldırabilir ve ardından sanal makine kaynaklarıyla ilişkilendirilmiş kaynak grubunu silebilirsiniz. Var olan bir sanal makineyi kullandıysanız son [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) cmdlet'ini atlayarak kaynak grubunu ve sanal makineyi bırakabilirsiniz.

Sanal makine verilerini nasıl geri yükleyeceğinizi açıklayan bir Yedekleme öğreticisine geçecekseniz bu bölümdeki adımları atlayın ve [Sonraki adımlar](#next-steps) bölümüne geçin. 

```powershell
Disable-AzureRmRecoveryServicesBackupProtection -Item $item -RemoveRecoveryPoints
$vault = Get-AzureRmRecoveryServicesVault -Name "myRecoveryServicesVault"
Remove-AzureRmRecoveryServicesVault -Vault $vault
Remove-AzureRmResourceGroup -Name "myResourceGroup"
```


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta bir Kurtarma Hizmetleri kasası oluşturdunuz, bir sanal makine için koruma özelliklerini etkinleştirdiniz ve ilk kurtarma noktasını oluşturdunuz. Azure Backup ve Kurtarma Hizmetleri hakkında daha fazla bilgi edinmek için öğreticilere geçin.

> [!div class="nextstepaction"]
> [Birden fazla Azure sanal makinesini yedekleme](./tutorial-backup-vm-at-scale.md)
