---
title: Windows Server için Azure hibrit avantajı | Microsoft Docs
description: Şirket içi lisanslarını Azure'a taşımalarına olanak, Windows Yazılım Güvencesi avantajları en üst düzeye öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: xujing
manager: gwallace
editor: ''
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 4/22/2018
ms.author: xujing-ms
ms.openlocfilehash: 739c867171d7b59a68f7e4d11bbf50a189568ce7
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722755"
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure Hibrit Teklifi
Yazılım Güvencesi olan müşteriler için Windows Server için Azure hibrit avantajı, şirket içi Windows Server lisanslarınızı kullanın ve Windows sanal makineler, düşük bir maliyet karşılığında Azure üzerinde çalıştırmak sağlar. Windows işletim sistemi ile yeni sanal makineleri dağıtmak için Windows Server için Azure hibrit Avantajı'nı kullanabilirsiniz. Bu makale Windows Server için Azure hibrit avantajı ile yeni VM'ler dağıtmayı ve varolan nasıl güncelleştirebilirsiniz adımları üzerinden Vm'leri çalıştıran gider. Windows Server için Azure hibrit Avantajı hakkında daha fazla bilgi için bkz: Lisans ve maliyet tasarrufu [Windows Server için Azure hibrit avantajı lisans sayfası](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

> [!Important]
> 2 işlemcili her lisans veya 16 çekirdekli lisans kümelerinin her biri, 8 çekirdekli iki örnek veya 16 çekirdekli bir örnek kullanma hakkına sahiptir. Standard Sürüm için Azure Hibrit Teklifi lisansları, şirket içinde ya da Azure’da yalnızca bir kez kullanılabilir. Datacenter Sürümü avantajları, aynı anda hem şirket içinde hem de Azure’da kullanım olanağı sunar.
>

> [!Important]
> Windows Server için Azure hibrit avantajı ile Windows Server işletim sistemi çalıştıran tüm Vm'leri kullanarak artık desteklenen SQL Server gibi ek yazılımlar veya üçüncü taraf Market yazılımları ile Vm'leri de dahil olmak üzere tüm bölgelerde. 
>

> [!NOTE]
> Klasik VM'ler için yalnızca dağıtma yeni VM'den şirket içi özel görüntüleri üzerinde desteklenir. Bu makalede desteklenen özelliklerden yararlanmak için Klasik VM'ler için Resource Manager modeli geçirmeniz gerekir.
>

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="ways-to-use-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure hibrit Avantajı'nı kullanma yolları
Windows sanal makinelerini, Azure karma avantajı ile kullanmak için birkaç yolu vardır:

1. Sağlanan Windows Server görüntüleri Azure Market'te birinden Vm'leri dağıtabilirsiniz.
2. Özel bir VM'yi karşıya yükleme ve bir Resource Manager şablonu veya Azure PowerShell kullanarak dağıtma
3. Geçiş ve Azure karma avantajı ile çalışan arasında varolan bir VM'yi dönüştürebilir veya Windows Server için isteğe bağlı ücreti ödersiniz
4. Windows Server için Azure hibrit avantajı üzerinde sanal makine ölçek kümesi de uygulayabilirsiniz


## <a name="create-a-vm-with-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure hibrit avantajı ile VM oluşturma
Tüm Windows Server tabanlı işletim sistemi görüntüleri Windows Server için Azure hibrit avantajı için desteklenir. Azure platformu desteği görüntüleri kullanmayı veya kendi özel Windows Server görüntülerini karşıya yükleme. 

### <a name="portal"></a>Portal
Windows Server için Azure hibrit avantajı ile bir VM oluşturmak için "para tasarrufu" bölümü altında iki durumlu düğmeyi kullanın.

### <a name="powershell"></a>PowerShell


```powershell
New-AzVm `
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

## <a name="convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure hibrit Avantajı'nı kullanarak mevcut bir VM'ye dönüştürme
Windows Server için Azure hibrit avantajı yararlanmak için dönüştürmek istediğiniz varolan bir VM'yi varsa, aşağıdaki yönergeleri izleyerek sanal makinenin lisans türü güncelleştirebilirsiniz.

> [!NOTE]
> VM'de lisans türünü değiştirerek sistemin yeniden başlatma veya hizmet interuption neden neden olmaz.  Bu yalnızca bir meta veri bayrak bir güncelleştirmedir.
> 

### <a name="portal"></a>Portal
VM dikey portalından Azure hibrit avantajı "Yapılandırma" seçeneğini belirleyerek kullanmak için VM'yi güncelleştirin ve "Azure hibrit avantajı" seçeneğini değiştirir

### <a name="powershell"></a>PowerShell
- Windows Server için Azure hibrit avantajı için mevcut Windows Server Vm'lerinin Dönüştür

    ```powershell
    $vm = Get-AzVM -ResourceGroup "rg-name" -Name "vm-name"
    $vm.LicenseType = "Windows_Server"
    Update-AzVM -ResourceGroupName rg-name -VM $vm
    ```
    
- Kullandıkça Öde dön teklifi'nden yararlanarak Windows Server Vm'lerinin Dönüştür

    ```powershell
    $vm = Get-AzVM -ResourceGroup "rg-name" -Name "vm-name"
    $vm.LicenseType = "None"
    Update-AzVM -ResourceGroupName rg-name -VM $vm
    ```
    
### <a name="cli"></a>CLI
- Windows Server için Azure hibrit avantajı için mevcut Windows Server Vm'lerinin Dönüştür

    ```azurecli
    az vm update --resource-group myResourceGroup --name myVM --set licenseType=Windows_Server
    ```

### <a name="how-to-verify-your-vm-is-utilizing-the-licensing-benefit"></a>VM'nize nasıl, lisans avantajından yararlanarak
PowerShell, Resource Manager şablonu veya portal üzerinden sanal makinenizin dağıttıktan sonra aşağıdaki yöntemlerden ayarında doğrulayabilirsiniz.

### <a name="portal"></a>Portal
VM dikey portaldan aç/kapa Windows Server için Azure hibrit avantajı için "Yapılandırma" sekmesini seçerek görüntüleyebilirsiniz.

### <a name="powershell"></a>PowerShell
Aşağıdaki örnek, tek bir VM için lisans türünü gösterir.
```powershell
Get-AzVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Çıktı:
```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Bu çıktı karşılaştırır Windows Server için Azure hibrit avantajı lisanslama olmadan dağıtılan aşağıdaki VM ile:
```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

### <a name="cli"></a>CLI
```azurecli
az vm get-instance-view -g MyResourceGroup -n MyVM --query "[?licenseType=='Windows_Server']" -o table
```

> [!NOTE]
> VM'de lisans türünü değiştirerek sistemin yeniden başlatma veya hizmet interuption neden neden olmaz. Bu bayrağı yalnızca lisans meta veri olur.
>

## <a name="list-all-vms-with-azure-hybrid-benefit-for-windows-server-in-a-subscription"></a>Windows Server için Azure hibrit avantajı ile tüm VM'lerin bir abonelikte listesi
Windows Server için Azure hibrit avantajı ile dağıtılan tüm sanal makinelerin sayısı ve görmek için aboneliğinizden aşağıdaki komutu çalıştırabilirsiniz:

### <a name="portal"></a>Portal
Sanal makine veya sanal makine ölçek kümesi kaynak dikey penceresinden tablo sütununda "Azure hibrit avantajı" içerecek şekilde yapılandırarak tüm VM ve lisans türü listesini görüntüleyebilirsiniz. VM ayarı 'Enabled "'olabilir,"Etkin değil"veya"Desteklenmiyor"durumu.

### <a name="powershell"></a>PowerShell
```powershell
$vms = Get-AzVM
$vms | ?{$_.LicenseType -like "Windows_Server"} | select ResourceGroupName, Name, LicenseType
```

### <a name="cli"></a>CLI
```azurecli
az vm list --query "[?licenseType=='Windows_Server']" -o table
```

## <a name="deploy-a-virtual-machine-scale-set-with-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure hibrit avantajı ile bir sanal makine ölçek kümesini dağıtma
İçinde sanal makine ölçek kümesi Resource Manager şablonları, ek bir parametre `licenseType` , VirtualMachineProfile özelliği içinde belirtilmelidir. Oluşturma sırasında bunu veya ARM şablonu aracılığıyla, PowerShell, Azure CLI veya REST ölçek kümenizi için güncelleştirin.

Aşağıdaki örnek, bir Windows Server 2016 Datacenter görüntüsü ile ARM şablonu kullanır:
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
Ayrıca kullanma hakkında daha fazla bilgi edinebilirsiniz [değiştirme bir sanal makine ölçek kümesi](../../virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set.md) ölçek kümenizi güncelleştirmek daha yollarını ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Azure karma avantajı ile tasarruf etme](https://azure.microsoft.com/pricing/hybrid-use-benefit/)
- Daha fazla bilgi edinin [Azure hibrit avantajı için sık sorulan sorular](https://azure.microsoft.com/pricing/hybrid-use-benefit/faq/)
- Daha fazla bilgi edinin [ayrıntılı kılavuz lisanslama Windows Server için Azure hibrit avantajı](https://docs.microsoft.com/windows-server/get-started/azure-hybrid-benefit)
- Daha fazla bilgi edinin [Windows Server için Azure hibrit avantajı ve Azure Site Recovery olun Azure uygulamalarını geçirme daha uygun maliyetli](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/)
- Daha fazla bilgi edinin [çok Kiracılı barındırma sağ ile azure'da Windows 10](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment)
- Daha fazla bilgi edinin [kullanarak Resource Manager şablonları](../../azure-resource-manager/resource-group-overview.md)
