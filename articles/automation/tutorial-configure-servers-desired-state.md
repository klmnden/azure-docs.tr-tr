---
title: Azure Otomasyonu ile sunucuları istenen duruma göre yapılandırma ve kaymaları yönetme
description: Öğretici - Azure Otomasyonu durum yapılandırması ile sunucu yapılandırmalarını yönetme
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
manager: carmonm
ms.topic: conceptual
ms.date: 08/08/2018
ms.openlocfilehash: 3bcdb667ee649b9bbf32ad33e74e876cdd2b5cbf
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144187"
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>Sunucuları istenen duruma göre yapılandırma ve kaymaları yönetme

Azure Otomasyonu durumu yapılandırması, sunucularınız için yapılandırmaları belirtin ve bu sunucular, zaman içinde belirtilen durumda olduğundan emin olun olanak sağlar.

> [!div class="checklist"]
> - Yerleşik Azure Automation DSC tarafından yönetilecek bir VM
> - Azure Otomasyonu bir yapılandırmayı karşıya yükleyin
> - Düğüm yapılandırması içine yapılandırma derleme
> - Yönetilen bir düğüme bir düğüm yapılandırması Ata
> - Bir yönetilen düğümü uyumluluk durumunu denetleyin

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Azure Otomasyonu hesabı. Bir Azure Otomasyonu Garklı Çalıştır hesabı oluşturma yönergeleri için bkz. [Azure Farklı Çalıştır Hesabı](automation-sec-configure-azure-runas-account.md).
- Azure Resource Manager VM (Klasik değil) Windows Server 2008 R2 çalıştıran veya üzeri. VM oluşturma yönergeleri için bkz. [Azure portalında ilk Windows sanal makinenizi oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
- Azure PowerShell modülü 3.6 veya sonraki bir sürümü. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/azurerm/install-azurerm-ps).
- Desired State Configuration (DSC) ile aşinalık. DSC hakkında daha fazla bilgi için bkz: [Windows PowerShell Desired State Configuration ' ne genel bakış](https://docs.microsoft.com/powershell/dsc/overview)

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Connect-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzureRmAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>Oluşturma ve yapılandırma, Azure Otomasyonu karşıya yükleme

Bu öğreticide, IIS sanal makinede yüklü olduğunu sağlayan basit bir DSC yapılandırması kullanacağız.

DSC yapılandırmaları hakkında bilgi edinmek için bkz. [DSC yapılandırmaları](/powershell/dsc/configurations).

Bir metin düzenleyicisine aşağıdakileri yazıp `TestConfig.ps1` adıyla yerel ortamda kaydedin.

```powershell
configuration TestConfig {
   Node WebServer {
      WindowsFeature IIS {
         Ensure               = 'Present'
         Name                 = 'Web-Server'
         IncludeAllSubFeature = $true
      }
   }
}
```

Çağrı `Import-AzureRmAutomationDscConfiguration` cmdlet'ini Otomasyon hesabınızda yapılandırmayı karşıya yükleyin:

```powershell
 Import-AzureRmAutomationDscConfiguration -SourcePath 'C:\DscConfigs\TestConfig.ps1' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Published
```

## <a name="compile-a-configuration-into-a-node-configuration"></a>Düğüm yapılandırması içine yapılandırma derleme

Bir düğüme atanabilmesi için önce bir DSC yapılandırması düğüm yapılandırması derlenmiş olmalıdır.

Derleme yapılandırmaları hakkında daha fazla bilgi için bkz: [DSC yapılandırmaları](/powershell/dsc/configurations).

Çağrı `Start-AzureRmAutomationDscCompilationJob` derlemek için cmdlet `TestConfig` düğüm yapılandırması ile yapılandırma:

```powershell
Start-AzureRmAutomationDscCompilationJob -ConfigurationName 'TestConfig' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount'
```

Bu adlı bir düğüm yapılandırması oluşturur `TestConfig.WebServer` Otomasyon hesabınızdaki.

## <a name="register-a-vm-to-be-managed-by-state-configuration"></a>Durum yapılandırması tarafından yönetilecek bir VM'yi kaydedin

Azure Vm'leri (Klasik ve Resource Manager), şirket içi Vm'leri, Linux makineler, AWS VM ve şirket içi fiziksel makineleri yönetmek için Azure Otomasyon durum Yapılandırması'nı kullanabilirsiniz. Bu konuda, yalnızca Azure Resource Manager Vm'lerinde kaydetme biz karşılarız. Diğer tür kaydetme hakkında daha fazla bilgi için bkz: [makineleri Azure Otomasyon durum yapılandırması tarafından Yönetim için hazırlama](automation-dsc-onboarding.md).

Çağrı `Register-AzureRmAutomationDscNode` cmdlet'ini Azure Otomasyon durum yapılandırması ile sanal makinenize kaydedin.

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm'
```

Bu durum yapılandırması yönetilen bir düğüm olarak belirtilen VM kaydeder.

### <a name="specify-configuration-mode-settings"></a>Yapılandırma modu ayarlarını belirtin

Bir yönetilen düğümü olarak bir VM kaydolduğunuzda yapılandırmasının özelliklerini de belirtebilirsiniz. Örneğin, yalnızca bir kez uygulanacak olan makinenin durumu belirtebilirsiniz (DSC denemez ilk onay sonra yapılandırmayı uygulamak) belirterek `ApplyOnly` değeri olarak **ConfigurationMode** özelliği :

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationMode 'ApplyOnly'
```

DSC kullanarak yapılandırma durumu ne sıklıkta denetleyeceğini de belirtebilirsiniz **ConfigurationModeFrequencyMins** özelliği:

```powershell
# Run a DSC check every 60 minutes
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationModeFrequencyMins 60
```

Yönetilen bir düğüm için yapılandırma özellikleri ayarlama hakkında daha fazla bilgi için bkz. [Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode).

DSC yapılandırma ayarları hakkında daha fazla bilgi için bkz. [yerel Configuration Manager Yapılandırma](/powershell/dsc/metaconfig).

## <a name="assign-a-node-configuration-to-a-managed-node"></a>Yönetilen bir düğüme bir düğüm yapılandırması Ata

Şimdi biz yapılandırmak istediğiniz VM için derlenmiş düğüm yapılandırmasının atayabilirsiniz.

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -NodeId $node.Id
```

Bu düğüm yapılandırmasının adı atar `TestConfig.WebServer` adlı kayıtlı DSC düğümü `DscVm`.
Varsayılan olarak, DSC düğümü, 30 dakikada bir düğüm yapılandırması ile uyumluluk için denetlenir.
Uyumluluğu denetleme aralığı değiştirme hakkında daha fazla bilgi için bkz: [yerel Configuration Manager Yapılandırma](/PowerShell/DSC/metaConfig).

## <a name="check-the-compliance-status-of-a-managed-node"></a>Bir yönetilen düğümü uyumluluk durumunu denetleyin

Yönetilen bir düğümün uyumluluk durumu raporları çağırarak alabilirsiniz `Get-AzureRmAutomationDscNodeReport` cmdlet:

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Get an array of status reports for the DSC node
$reports = Get-AzureRmAutomationDscNodeReport -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeId $node.Id

# Display the most recent report
$reports[0]
```

## <a name="removing-nodes-from-service"></a>Düğümleri Hizmeti'nden kaldırılıyor

Azure Otomasyonu durum yapılandırması için bir düğüm eklediğinizde, hizmet ve çekme yapılandırmaları ve makineyi yapılandırmak için gerekli modülleri ile kaydetmek için yerel Configuration Manager ayarlarını ayarlayın.
Düğümü hizmetten kaldırmak isterseniz, Azure portal'ı veya Az cmdlet'lerini kullanarak bunu yapabilirsiniz.

> [!NOTE]
> Düğüm artık hizmetine bağlanıyor şekilde hizmetinden yalnızca bir düğümün kaydı yerel Configuration Manager ayarlarını ayarlar.
> Bu düğüme uygulanmış yapılandırmasını etkilemez.
> Geçerli yapılandırmayı kaldırmak üzere kullanımı [PowerShell](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/remove-dscconfigurationdocument?view=powershell-5.1) veya (Linux düğümleri için tek seçenek budur) yerel yapılandırma dosyasını silin.

### <a name="azure-portal"></a>Azure portal

Azure Otomasyonu, tıklayarak **durum yapılandırması (DSC)** İçindekiler.
İleri'yi **düğümleri** hizmete kayıtlı olan düğüm listesini görüntülemek için.
Kaldırmak istediğiniz düğümün adına tıklayın.
Açılır düğümün görünümüne tıklayın **Unregister**.

### <a name="powershell"></a>PowerShell

PowerShell kullanarak Azure Otomasyonu durumu Configuration hizmetinden bir düğüm kaydını kaldırmak için cmdlet belgelerini izleyin [Unregister-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/az.automation/unregister-azautomationdscnode?view=azps-2.0.0).

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için bkz: [Azure Otomasyon durum yapılandırması ile çalışmaya başlama](automation-dsc-getting-started.md)
- Bilgi edinmek için nasıl yerleşik düğümlerine bkz [makineleri Azure Otomasyon durum yapılandırması tarafından Yönetim için hazırlama](automation-dsc-onboarding.md)
- Hedef düğümleri atayabilirsiniz böylece, DSC yapılandırmaları derleme hakkında bilgi edinmek için [yapılandırmaları Azure Automation durumu yapılandırma derleme](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için bkz. [Azure Otomasyonu durumu yapılandırma cmdlet'leri](/powershell/module/azurerm.automation/#automation)
- Fiyatlandırma bilgileri için bkz: [Azure Otomasyon durum yapılandırması için fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/)
- Bir sürekli dağıtım işlem hattı, Azure Otomasyonu durum yapılandırmasını kullanarak bir örnek görmek için bkz: [sürekli dağıtımı kullanarak Azure Otomasyon durum yapılandırması ve Chocolatey](automation-dsc-cd-chocolatey.md)
