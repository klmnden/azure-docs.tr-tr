---
title: Windows Server için Azure karma avantajı | Microsoft Docs
description: Azure için şirket içi lisansları getirmek için Windows Yazılım Güvencesi Avantajlarınızı en üst düzeye öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: xujing
manager: jeconnoc
editor: ''
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 4/22/2018
ms.author: xujing-ms
ms.openlocfilehash: a4b0baefc8c3c839a06d6540e57b34657138c8ff
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure Hibrit Teklifi
Yazılım Güvencesi ile müşteriler için Windows Server için Azure karma avantajı, şirket içi Windows Server lisanslarınızı kullanın ve Windows sanal makineleri Azure üzerinde sınırlı bir ücret çalıştırmak olanak sağlar. Windows Server için Azure karma avantajı, Windows işletim sistemi ile yeni sanal makineleri dağıtmak için kullanabilirsiniz. Bu makalede Windows Server için Azure karma avantajı olan yeni VM'ler dağıtma ve nasıl varolan güncelleştirebilirsiniz adımları üzerinden VM'ler çalıştıran gider. Windows Server için Azure karma Avantajı hakkında daha fazla bilgi için bkz: Lisans ve maliyet tasarrufu [Windows Server için Azure karma avantajı lisans sayfası](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

> [!Important]
> 2 işlemcili her lisans veya 16 çekirdekli lisans kümelerinin her biri, 8 çekirdekli iki örnek veya 16 çekirdekli bir örnek kullanma hakkına sahiptir. Standard Sürüm için Azure Hibrit Teklifi lisansları, şirket içinde ya da Azure’da yalnızca bir kez kullanılabilir. Datacenter Sürümü avantajları, aynı anda hem şirket içinde hem de Azure’da kullanım olanağı sunar.
>

> [!Important]
> Windows Server işletim sistemi çalıştıran herhangi bir VM ile Windows Server için Azure karma avantajı kullanarak şimdi desteklenen SQL Server gibi ek yazılımı veya üçüncü taraf Market yazılımı ile sanal makineleri de dahil olmak üzere tüm bölgelerde. 
>

> [!NOTE]
> Klasik VM'ler için yalnızca şirket içi özel resimler yeni VM'den dağıtma desteklenir. Bu makalede desteklenen özelliklerinden yararlanmak için Klasik sanal makineleri için Resource Manager modeli geçirmeniz gerekir.
>


## <a name="ways-to-use-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure karma avantajı kullanmanın yolları
Azure karma avantajı ile Windows sanal makineleri kullanmak için birkaç yolu vardır:

1. Sağlanan birinden VM'ler dağıtabilirsiniz [Azure Marketi Windows Server görüntülerinde](#https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview)
2. Özel bir VM karşıya yükleme ve Resource Manager şablonu ya da Azure PowerShell kullanarak dağıtın
3. Geçiş ve Azure karma avantajı ile çalışan arasında var olan VM dönüştürebilir veya Windows Server için isteğe bağlı maliyet ödeme
4. Windows Server için Azure karma avantajı da ayarlamak sanal makine ölçek üzerinde uygulayabilirsiniz


## <a name="create-a-vm-with-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure karma avantajı ile bir VM oluşturma
Tüm Windows Server tabanlı işletim sistemi görüntüleri Windows Server için Azure karma avantajı için desteklenir. Azure platformu desteği görüntüleri veya kendi özel Windows Server görüntülerini kullanabilirsiniz. 

### <a name="portal"></a>Portal
Windows Server için Azure karma avantajı ile bir VM oluşturmak için iki durumlu altında "paradan tasarruf" bölümü kullanın.

### <a name="powershell"></a>PowerShell
```powershell
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -ImageName "Win2016Datacenter" `
    -LicenseType "Windows_Server"
```

### <a name="cli"></a>CLI
```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --license-type Windows_Server
```

### <a name="template"></a>Şablon
Resource Manager şablonlarınızı, ek bir parametre içinde `licenseType` belirtilmesi gerekir. Daha fazla bilgi edinebilirsiniz [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md)
```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure karma avantajı kullanarak mevcut bir VM'yi Dönüştür
Windows Server için Azure karma avantajı yararlanmak için dönüştürmek istediğiniz mevcut bir VM'yi varsa, VM lisans türü aşağıdaki gibi güncelleştirebilirsiniz:

### <a name="portal"></a>Portal
VM dikey portaldan Azure karma fayda "Yapılandırma" seçeneğini belirleyerek kullanmak için VM güncelleştirin ve "Azure karma fayda" seçeneğini Değiştir

### <a name="powershell"></a>PowerShell
- Windows Server için Azure karma avantajı için var olan Windows Server Vm'lerinin Dönüştür

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
    $vm.LicenseType = "Windows_Server"
    Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
    ```
    
- Kullandıkça Öde dön avantajı ile Windows Server Vm'lerinin Dönüştür

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
    $vm.LicenseType = "None"
    Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
    ```
    
### <a name="cli"></a>CLI
- Windows Server için Azure karma avantajı için var olan Windows Server Vm'lerinin Dönüştür

    ```azurecli
    az vm update --resource-group myResourceGroup --name myVM --set licenseType=Windows_Server
    ```
    
### <a name="how-to-verify-your-vm-is-utilizing-the-licensing-benefit"></a>VM'nize nasıl lisans avantajı kullanma
PowerShell, Resource Manager şablonu veya portal aracılığıyla, VM dağıttıktan sonra aşağıdaki yöntemlerden ayarında doğrulayabilirsiniz.

### <a name="portal"></a>Portal
VM dikey portalından "Yapılandırması" sekmesinde seçerek Windows Server için Azure karma avantajı için iki durumlu görüntüleyebilirsiniz.

### <a name="powershell"></a>PowerShell
Aşağıdaki örnek, lisans türü için tek bir VM gösterir.
```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Çıktı:
```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Bu çelişir çıktı Windows Server için Azure karma avantajı lisans olmadan dağıtılan aşağıdaki VM ile:
```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

### <a name="cli"></a>CLI
```azurecli
az vm get-instance-view -g MyResourceGroup -n MyVM --query '[?licenseType==Windows_Server]' -o table
```

## <a name="list-all-vms-with-azure-hybrid-benefit-for-windows-server-in-a-subscription"></a>Windows Server için Azure karma avantajı ile tüm VM'lerin bir abonelikte listesi
Windows Server için Azure karma avantajı ile dağıtılan tüm sanal makinelerin sayısı ve görmek için aboneliğinizden aşağıdaki komutu çalıştırabilirsiniz:

### <a name="portal"></a>Portal
Sanal makine veya sanal makine ölçek kümeleri kaynak dikey penceresinden, "Azure karma avantajı" içerecek şekilde tablo sütunu yapılandırarak tüm sanal makine ve lisans türü listesini görüntüleyebilirsiniz. VM ayarı ya da 'Enabled "'olabilir,"Etkin"veya"Desteklenmiyor"durumu.

### <a name="powershell"></a>PowerShell
```powershell
$vms = Get-AzureRMVM 
$vms | ?{$_.LicenseType -like "Windows_Server"} | select ResourceGroupName, Name, LicenseType
```

### <a name="cli"></a>CLI
```azurecli
az vm list --query '[?licenseType==Windows_Server]' -o table
```

## <a name="deploy-a-virtual-machine-scale-set-with-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure karma avantajı ile bir sanal makine ölçek kümesini dağıtma
İçinde sanal makine ölçek kümesi Resource Manager şablonları, ek bir parametre `licenseType` VirtualMachineProfile özelliğinizi içinde belirtilmesi gerekir. Oluşturma sırasında bunu veya Powershell, Azure CLI veya REST gibi ARM şablonu aracılığıyla ayarlamak, Ölçek için güncelleştirin.

Aşağıdaki örnek, bir Windows Server 2016 Datacenter görüntüsüyle ARM şablonu kullanır:
```json
"virtualMachineProfile": {
    "storageProfile": {
        "osDisk": {
            "createOption": "FromImage"
        },
        "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2016-Datacenter",
            "version": "latest"
        }
    },
    "licenseType": "Windows_Server",
    "osProfile": {
            "computerNamePrefix": "[parameters('vmssName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
    }
```
Da hakkında daha fazla bilgi alabilirsiniz [değiştirme bir sanal makine ölçek kümesi](../../virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set.md) diğer yolları, ölçeği güncelleştirme ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Azure karma avantajı paradan tasarruf etme](https://azure.microsoft.com/pricing/hybrid-use-benefit/)
- Daha fazla bilgi edinin [Azure karma avantajı için sık sorulan sorular](https://azure.microsoft.com/pricing/hybrid-use-benefit/faq/)
- Daha fazla bilgi edinmek [ayrıntılı kılavuz lisans Windows Server için Azure karma avantajı](https://docs.microsoft.com/windows-server/get-started/azure-hybrid-benefit)
- Daha fazla bilgi edinmek [Windows Server için Azure karma avantajı Azure Site Recovery yapıp Azure uygulamaları geçirme daha uygun maliyetli](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/)
- Daha fazla bilgi edinmek [çok kullanıcılı barındırma hakkına sahip Azure üzerinde Windows 10](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment)
- Daha fazla bilgi edinmek [kullanarak Resource Manager şablonları](../../azure-resource-manager/resource-group-overview.md)
