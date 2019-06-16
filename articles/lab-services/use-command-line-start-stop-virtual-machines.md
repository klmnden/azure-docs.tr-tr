---
title: Azure DevTest Labs sanal makineleri durdurmak ve başlatmak komut satırı araçlarını kullanma | Microsoft Docs
description: Azure DevTest Labs'de sanal makineler durdurmak ve başlatmak komut satırı araçlarını kullanmayı öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: a8132735d1af08055e9341608dcac0564ed4b927
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60236675"
---
# <a name="use-command-line-tools-to-start-and-stop-azure-devtest-labs-virtual-machines"></a>Azure DevTest Labs sanal makineleri durdurmak ve başlatmak için komut satırı araçlarını kullanma
Bu makalede başlatmak veya sanal makineler'de Azure DevTest labs'deki bir laboratuvara durdurmak için Azure PowerShell veya Azure CLI kullanma işlemini gösterir. Bu işlemleri otomatikleştirmek için PowerShell/CLI betiklerini oluşturabilirsiniz. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
Azure DevTest Labs, hızlı, kolay ve yalın geliştirme ve test ortamları oluşturmak için kullanılan bir yoldur. Bu maliyet yönetmek, hızlı bir şekilde Vm'leri sağlayın ve en aza indirmek, atık sağlar.  Azure portalında Vm'leri otomatik olarak belirli zamanlarda durdurmak ve başlatmak Laboratuvar yapılandırma olanak tanıyan yerleşik özellikler mevcuttur. 

Ancak, bazı senaryolarda, sanal makinelerin PowerShell/CLI betiklerin durdurmayla ve otomatik hale getirmek isteyebilirsiniz. Bu, belirli zamanlarda yerine istediğiniz zaman tek tek makineler durdurmayla ve ile bazı esnekliği sağlar. Bu görevler betikler kullanarak hangi çalışıyor yardımcı olabilir durumlarda bazıları aşağıda verilmiştir.

- 3 katmanlı uygulama test ortamının bir parçası kullanırken, katmanları sıralı olarak başlatılması gerekir. 
- Özel ölçütler paradan tasarruf etmek için karşılandığında bir VM devre dışı bırakın. 
- Bu bir CI/CD iş akışı içinde bir görev olarak akışın başından itibaren başlatmak, makineler oluşturdukça Vm'leri kullanın, makineleri veya altyapıyı test edin ve ardından işlemi tamamlandığında, sanal makineleri durdurmak için kullanın. Buna örnek olarak Azure DevTest Labs ile özel görüntü Fabrika olacaktır.  

## <a name="azure-powershell"></a>Azure PowerShell
Aşağıdaki PowerShell betiğini bir laboratuvarda VM başlatır. [Çağırma AzResourceAction](/powershell/module/az.resources/invoke-azresourceaction?view=azps-1.7.0) birincil odak noktası için bu betiği. **ResourceId** parametredir Laboratuvar içinde VM için tam kaynak kimliği. **Eylem** parametredir nerede **Başlat** veya **Durdur** seçenekleri, ihtiyacınız olan şey bağlı olarak ayarlanır.

```powershell
# The id of the subscription
$subscriptionId = "111111-11111-11111-1111111"

# The name of the lab
$devTestLabName = "yourlabname"

# The name of the virtual machine to be started
$vMToStart = "vmname"

# The action on the virtual machine (Start or Stop)
$vmAction = "Start"

# Select the Azure subscription
Select-AzSubscription -SubscriptionId $subscriptionId

# Get the lab information
if ($(Get-Module -Name AzureRM).Version.Major -eq 6) {
    $devTestLab = Get-AzResource -ResourceType 'Microsoft.DevTestLab/labs' -Name $devTestLabName
} else {
    $devTestLab = Find-AzResource -ResourceType 'Microsoft.DevTestLab/labs' -ResourceNameEquals $devTestLabName
}

# Start the VM and return a succeeded or failed status
$returnStatus = Invoke-AzResourceAction `
                    -ResourceId "$($devTestLab.ResourceId)/virtualmachines/$vMToStart" `
                    -Action $vmAction `
                    -Force

if ($returnStatus.Status -eq 'Succeeded') {
    Write-Output "##[section] Successfully updated DTL machine: $vMToStart, Action: $vmAction"
}
else {
    Write-Error "##[error]Failed to update DTL machine: $vMToStart, Action: $vmAction"
}
```


## <a name="azure-cli"></a>Azure CLI
[Azure CLI](/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) başlatma ve durdurma DevTest Labs sanal makinelerin otomatik hale getirmek için başka bir yoludur. Azure CLI olabilir [yüklü](/cli/azure/install-azure-cli?view=azure-cli-latest) farklı işletim sistemlerinde. Aşağıdaki betiği başlatma ve durdurma laboratuvarda bir VM için komutlar sağlar. 

```azurecli
# Sign in to Azure
az login 

## Get the name of the resource group that contains the lab
az resource list --resource-type "Microsoft.DevTestLab/labs" --name "yourlabname" --query "[0].resourceGroup" 

## Start the VM
az lab vm start --lab-name yourlabname --name vmname --resource-group labResourceGroupName

## Stop the VM
az lab vm stop --lab-name yourlabname --name vmname --resource-group labResourceGroupName
```


## <a name="next-steps"></a>Sonraki adımlar
Bu işlemleri yapmak için Azure portalını kullanarak için şu makaleye bakın: [Bir VM'yi yeniden başlatma](devtest-lab-restart-vm.md).
