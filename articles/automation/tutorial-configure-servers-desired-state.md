---
title: "İstenilen durumu sunucularına yapılandırma ve Azure Automation kayması yönetme | Microsoft Docs"
description: "Öğretici - Azure Otomasyonu DSC sunucu yapılandırmalarını yönetme"
services: automation
documentationcenter: automation
author: eslesar
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: automation
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: eslesar
ms.custom: 
ms.openlocfilehash: 9c0a44b37ac303f3e93c54e3bf691f14ba1d65e9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>İstenilen durumu sunucularına yapılandırma ve kayması yönetme

Azure Otomasyonu istenen durum yapılandırması (DSC) sunucularınız için yapılandırmaları belirtin ve bu sunucular zaman içinde belirtilen durumda olduğundan emin olmak sağlar.



> [!div class="checklist"]
> * Yerleşik Azure Otomasyonu DSC tarafından yönetilmek üzere bir VM
> * Bir yapılandırma için Azure Otomasyonu karşıya yükle
> * Düğüm yapılandırması içine bir yapılandırma derleme
> * Yönetilen bir düğüme bir düğüm yapılandırması atayın
> * Bir yönetilen düğümü uyumluluk durumunu denetleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için gerekir:

* Azure Otomasyonu hesabı. Bir Azure Otomasyonu Garklı Çalıştır hesabı oluşturma yönergeleri için bkz. [Azure Farklı Çalıştır Hesabı](automation-sec-configure-azure-runas-account.md).
* Bir Azure Kaynak Yöneticisi'ni VM (Klasik değil) Windows Server 2008 R2 çalıştıran veya sonraki bir sürümü. VM oluşturma yönergeleri için bkz. [Azure portalında ilk Windows sanal makinenizi oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
* Azure PowerShell modülü 3,6 veya sonraki bir sürümü. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).
* DSC hakkında bilgi. DSC hakkında daha fazla bilgi için bkz: [Windows PowerShell istenen durum yapılandırması genel bakış](https://docs.microsoft.com/powershell/dsc/overview)

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzureRmAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>Oluşturma ve yapılandırma için Azure Automation yükleme

Bu öğretici için IIS VM yüklendiğini sağlayan basit bir DSC yapılandırma kullanacağız.

DSC yapılandırmaları hakkında daha fazla bilgi için bkz: [DSC yapılandırmaları](https://docs.microsoft.com/powershell/dsc/configurations).

Bir metin düzenleyicisinde, aşağıdaki komutu yazın ve yerel olarak kaydedin `TestConfig.ps1`.

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

Çağrı `Import-AzureRmAutomationDscConfiguration` Otomasyon hesabınızda yapılandırma karşıya yüklemek için cmdlet:

```powershell
 Import-AzureRmAutomationDscConfiguration -SourcePath 'C:\DscConfigs\TestConfig.ps1' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Published
```

## <a name="compile-a-configuration-into-a-node-configuration"></a>Düğüm yapılandırması içine bir yapılandırma derleme

Bir düğüme atanabilmesi için önce bir DSC yapılandırma düğüm yapılandırması derlenmiş olmalıdır.

Derleme yapılandırmaları hakkında daha fazla bilgi için bkz: [DSC yapılandırmaları](https://docs.microsoft.com/powershell/dsc/configurations).

Çağrı `Start-AzureRmAutomationDscCompilationJob` derlemek için cmdlet `TestConfig` yapılandırmalarına düğüm yapılandırması ayrılır:

```powershell
Start-AzureRmAutomationDscCompilationJob -ConfigurationName 'TestConfig' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount'
```

Bu adlı bir düğüm yapılandırması oluşturur `TestConfig.WebServer` Otomasyon hesabınızda.

## <a name="register-a-vm-to-be-managed-by-dsc"></a>VM DSC tarafından yönetilmek üzere kaydetme

Azure Otomasyonu DSC, Azure Vm'leri (Klasik ve Resource Manager), şirket içi sanal makineleri, Linux makineler, AWS VM'ler ve şirket içi fiziksel makineleri yönetmek için kullanabilirsiniz. Bu konuda, yalnızca Azure Kaynak Yöneticisi Vm'leri kaydetme kapsar.
Diğer tür kaydetme hakkında daha fazla bilgi için bkz: [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md).

Çağrı `Register-AzureRmAutomationDscNode` VM Azure Otomasyonu DSC'ye kaydetmek için cmdlet.

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName "MyResourceGroup" -AutomationAccountName "myAutomationAccount" -AzureVMName "DscVm"
```

Bu, belirtilen sanal Makineyi bir Azure Otomasyonu hesabınızı DSC düğüm olarak kaydeder.

### <a name="specify-configuration-mode-settings"></a>Yapılandırma modu ayarlarını belirtin

Yönetilen bir düğüm olarak bir VM kaydederken yapılandırmasının özelliklerini de belirtebilirsiniz.
Örneğin, makinenin durumunu yalnızca bir kez uygulanmasına olduğunu belirtebilirsiniz (DSC denemez ilk onay sonra yapılandırmayı uygulamak) belirterek `ApplyOnly` değeri olarak **ConfigurationMode** özelliği :

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName "DscVm" -ConfigurationMode 'ApplyOnly'
```

Kullanarak DSC yapılandırma durumunu ne sıklıkta denetleyeceğini belirtebilirsiniz **ConfigurationModeFrequencyMins** özelliği:

```powershell
# Run a DSC check every 60 minutes
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName "DscVm" -ConfigurationModeFrequencyMins 60
```

Yönetilen bir düğüm için yapılandırma özellikleri ayarlama hakkında daha fazla bilgi için bkz: [Register-AzureRmAutomationDscNode](https://docs.microsoft.com/powershell/module/azurerm.automation/register-azurermautomationdscnode?view=azurermps-4.3.1&viewFallbackFrom=azurermps-4.2.0).

DSC yapılandırma ayarları hakkında daha fazla bilgi için bkz: [yerel Configuration Manager Yapılandırma](https://docs.microsoft.com/powershell/dsc/metaconfig).

## <a name="assign-a-node-configuration-to-a-managed-node"></a>Yönetilen bir düğüme bir düğüm yapılandırması atayın

Şimdi biz biz yapılandırmak istediğiniz VM derlenmiş düğüm yapılandırması atayabilirsiniz.

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -Id $node.Id
```

Bu düğüm yapılandırmasını adlı atar `TestConfig.WebServer` adlı kayıtlı DSC düğümü `DscVm`.
Varsayılan olarak, DSC düğümü, her 30 dakikada düğüm yapılandırması ile uyumluluk açısından denetlenir.
Uyumluluk denetimi aralığını değiştirme hakkında daha fazla bilgi için bkz: [yerel Configuration Manager'ı yapılandırma](https://docs.microsoft.com/PowerShell/DSC/metaConfig)

## <a name="check-the-compliance-status-of-a-managed-node"></a>Bir yönetilen düğümü uyumluluk durumunu denetleme

DSC düğümü uyumluluk durumu raporları çağırarak alabilirsiniz `Get-AzureRmAutomationDscNodeReport` cmdlet:

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Get an array of status reports for the DSC node
$reports = Get-AzureRmAutomationDscNodeReport -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Id $node.Id

# Display the most recent report
$reports[0]
```

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl Azure Otomasyonu DSC'ye yönetilecek yerleşik düğümleri için bkz: [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md)
* Automation DSC kullanmak için Azure portalını kullanmayı öğrenmek için bkz: [Azure Otomasyonu DSC ile çalışmaya başlama](automation-dsc-getting-started.md)
* Böylece hedef düğümleri atayabilirsiniz DSC yapılandırmaları derleme hakkında bilgi edinmek için [Azure Otomasyonu DSC yapılandırmalarında derleme](automation-dsc-compile.md)
* Azure Otomasyonu DSC için PowerShell cmdlet başvuru için bkz: [Azure Otomasyonu DSC cmdlet'leri](/powershell/module/azurerm.automation/#automation)
* Fiyatlandırma bilgileri için bkz: [Azure Otomasyonu DSC fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/)
* Sürekli dağıtım ardışık düzeninde Azure Automation DSC kullanmaya ilişkin bir örnek görmek için bkz: [Iaas VM'ler kullanarak Azure Otomasyonu DSC ve Chocolatey sürekli dağıtım](automation-dsc-cd-chocolatey.md)
