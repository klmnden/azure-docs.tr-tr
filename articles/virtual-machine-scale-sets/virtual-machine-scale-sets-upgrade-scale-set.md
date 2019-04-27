---
title: Bir Azure sanal makine ölçek kümesini değiştirme | Microsoft Docs
description: Değiştirme ve REST API'leri, Azure PowerShell ve Azure CLI ile bir Azure sanal makine ölçek güncelleştirme hakkında bilgi edinin
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: manayar
ms.openlocfilehash: 71899a9d6782c4700c287458c85ec83bd1516a4b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60803143"
---
# <a name="modify-a-virtual-machine-scale-set"></a>Sanal makine ölçek kümesini değiştirme

Uygulamalarınızın yaşam döngüsü boyunca, değiştirmek veya sanal makine ölçek kümenizi güncelleştirirseniz gerekebilir. Bu güncelleştirmeler, Ölçek kümesi yapılandırmasını güncelleştirme veya uygulama yapılandırmasını değiştirmek nasıl içerebilir. Bu makale, mevcut bir ölçek REST API'leri, Azure PowerShell veya Azure CLI ile kümesini değiştirmek açıklamaktadır.

## <a name="fundamental-concepts"></a>Temel kavramlar

### <a name="the-scale-set-model"></a>Ölçek kümesi modeline
Bir ölçek kümesi "yakalayan bir ölçek kümesi modeline" sahip *istenen* ölçeğin durum bir bütün olarak ayarlayın. Bir ölçek kümesi modelini sorgulamak için kullanabilirsiniz 

- REST API ile [virtualmachinescalesets/işlem/get](/rest/api/compute/virtualmachinescalesets/get) gibi:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version={apiVersion}
    ```

- Azure PowerShell ile [Get-AzVmss](/powershell/module/az.compute/get-azvmss):

    ```powershell
    Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
    ```

- Azure CLI ile [az vmss show](/cli/azure/vmss):

    ```azurecli
    az vmss show --resource-group myResourceGroup --name myScaleSet
    ```

- Ayrıca [resources.azure.com](https://resources.azure.com) veya dile özgü [Azure SDK'ları](https://azure.microsoft.com/downloads/).

Çıkış tam sunumunu komutu sağlayan seçenekler bağlıdır. Aşağıdaki örnekte Azure CLI üzerinden sıkıştırılmış örnek çıktı gösterilmektedir:

```azurecli
az vmss show --resource-group myResourceGroup --name myScaleSet
{
  "location": "westus",
  "overprovision": true,
  "plan": null,
  "singlePlacementGroup": true,
  "sku": {
    "additionalProperties": {},
    "capacity": 1,
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
}
```

Bu özellikler, Ölçek kümesindeki bir bütün olarak için geçerlidir.


### <a name="the-scale-set-instance-view"></a>Ölçek kümesi örnek görünümü
Bir ölçek kümesi de "görüntülemek bir ölçek kümesi örneği" geçerli yakalayan sahip *çalışma zamanı* ölçeğin durum bir bütün olarak ayarlayın. Bir ölçek kümesi örnek görünümünü sorgulamak için kullanabilirsiniz:

- REST API ile [virtualmachinescalesets/işlem/getinstanceview](/rest/api/compute/virtualmachinescalesets/getinstanceview) gibi:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/instanceView?api-version={apiVersion}
    ```

- Azure PowerShell ile [Get-AzVmss](/powershell/module/az.compute/get-azvmss):

    ```powershell
    Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceView
    ```

- Azure CLI ile [az vmss get-instance-view](/cli/azure/vmss):

    ```azurecli
    az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet
    ```

- Ayrıca [resources.azure.com](https://resources.azure.com) veya dile özgü [Azure SDK'ları](https://azure.microsoft.com/downloads/)

Çıkış tam sunumunu komutu sağlayan seçenekler bağlıdır. Aşağıdaki örnekte Azure CLI üzerinden sıkıştırılmış örnek çıktı gösterilmektedir:

```azurecli
$ az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet
{
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "level": "Info",
      "message": null,
      "time": "{time}"
    }
  ],
  "virtualMachine": {
    "additionalProperties": {},
    "statusesSummary": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "count": 1
      }
    ]
  }
}
```

Bu özellikler, geçerli çalışma zamanı durumu ölçek kümesine uygulanan uzantıları durumu gibi ölçek kümesindeki sanal makinelerin bir özetini sağlar.


### <a name="the-scale-set-vm-model-view"></a>Ölçek kümesi VM model görünümü
Benzer şekilde nasıl bir model görünümü bir ölçek kümesi vardır, Ölçek kümesindeki her sanal makine örneği kendi modeli görünüme sahiptir. Bir ölçek kümesindeki belirli bir sanal makine örneği için model görünümü sorgulamak için kullanabilirsiniz:

- REST API ile [virtualmachinescalesetvms/işlem/get](/rest/api/compute/virtualmachinescalesetvms/get) gibi:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualmachines/instanceId?api-version={apiVersion}
    ```

- Azure PowerShell ile [Get-AzVmssVm](/powershell/module/az.compute/get-azvmssvm):

    ```powershell
    Get-AzVmssVm -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId
    ```

- Azure CLI ile [az vmss show](/cli/azure/vmss):

    ```azurecli
    az vmss show --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- Ayrıca [resources.azure.com](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/).

Çıkış tam sunumunu komutu sağlayan seçenekler bağlıdır. Aşağıdaki örnekte Azure CLI üzerinden sıkıştırılmış örnek çıktı gösterilmektedir:

```azurecli
$ az vmss show --resource-group myResourceGroup --name myScaleSet
{
  "location": "westus",
  "name": "{name}",
  "sku": {
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
}
```

Bu özellikler yapılandırma, Ölçek kümesindeki bir bütün olarak bir ölçek kümesindeki bir sanal makine örneği yapılandırma açıklanmaktadır. Örneğin, Ölçek kümesi modeline sahip `overprovision` değilken bir ölçek kümesindeki bir sanal makine örneği için model bir özelliği olarak. Açıdan ölçek kümesindeki tüm, bireysel bir sanal makine örnekleri olarak ölçek için bir özellik olduğundan bu farktır (açıdan hakkında daha fazla bilgi için bkz. [tasarım konuları ölçek kümeleri için](virtual-machine-scale-sets-design-overview.md#overprovisioning)).


### <a name="the-scale-set-vm-instance-view"></a>Ölçek kümesi sanal makine örnek görünümü
Benzer şekilde, bir ölçek kümesi örnek görünümünü nasıl sahip, Ölçek kümesindeki her sanal makine örneği kendi örneği görünüme sahiptir. Bir ölçek kümesi içinde belirli bir VM örneği için örnek görünümü sorgulamak için kullanabilirsiniz:

- REST API ile [virtualmachinescalesetvms/işlem/getinstanceview](/rest/api/compute/virtualmachinescalesetvms/getinstanceview) gibi:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualmachines/instanceId/instanceView?api-version={apiVersion}
    ```

- Azure PowerShell ile [Get-AzVmssVm](/powershell/module/az.compute/get-azvmssvm):

    ```powershell
    Get-AzVmssVm -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId -InstanceView
    ```

- Azure CLI ile [az vmss get-instance-view](/cli/azure/vmss)

    ```azurecli
    az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- Ayrıca [resources.azure.com](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/)

Çıkış tam sunumunu komutu sağlayan seçenekler bağlıdır. Aşağıdaki örnekte Azure CLI üzerinden sıkıştırılmış örnek çıktı gösterilmektedir:

```azurecli
$ az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
{
  "additionalProperties": {
    "osName": "ubuntu",
    "osVersion": "16.04"
  },
  "disks": [
    {
      "name": "{name}",
      "statuses": [
        {
          "additionalProperties": {},
          "code": "ProvisioningState/succeeded",
          "displayStatus": "Provisioning succeeded",
          "time": "{time}"
        }
      ]
    }
  ],
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "time": "{time}"
    },
    {
      "additionalProperties": {},
      "code": "PowerState/running",
      "displayStatus": "VM running"
    }
  ],
  "vmAgent": {
    "statuses": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "displayStatus": "Ready",
        "level": "Info",
        "message": "Guest Agent is running",
        "time": "{time}"
      }
    ],
    "vmAgentVersion": "{version}"
  },
}
```

Bu özellikler ölçek kümesine uygulanan tüm uzantıları içeren bir ölçek kümesindeki bir sanal makine örneği geçerli çalışma zamanı durumunu açıklar.


## <a name="how-to-update-global-scale-set-properties"></a>Küresel ölçek güncelleştirme özelliklerini ayarlama
Küresel ölçek kümesi özelliği güncelleştirmek için ölçek kümesi modeline özelliğinde güncelleştirmeniz gerekir. Bu güncelleştirme aracılığıyla yapabilirsiniz:

- REST API ile [virtualmachinescalesets/işlem/createorupdate](/rest/api/compute/virtualmachinescalesets/createorupdate) gibi:

    ```rest
    PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version={apiVersion}
    ```

- Küresel ölçek kümesi özelliklerini güncelleştirmek için REST API özellikleri ile Resource Manager şablonu dağıtabilirsiniz.

- Azure PowerShell ile [güncelleştirme AzVmss](/powershell/module/az.compute/update-azvmss):

    ```powershell
    Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -VirtualMachineScaleSet {scaleSetConfigPowershellObject}
    ```

- Azure CLI ile [az vmss update](/cli/azure/vmss):
    - Bir özelliği değiştirmek için:

        ```azurecli
        az vmss update --set {propertyPath}={value}
        ```

    - Bir ölçek kümesindeki bir liste özelliği bir nesne eklemek için: 

        ```azurecli
        az vmss update --add {propertyPath} {JSONObjectToAdd}
        ```

    - Bir nesne bir ölçek kümesindeki bir liste özelliği kaldırmak için: 

        ```azurecli
        az vmss update --remove {propertyPath} {indexToRemove}
        ```

    - Ölçek kümesi daha önce dağıttıysanız `az vmss create` komutunu çalıştırabilirsiniz `az vmss create` yeniden ölçek kümesini güncelleştirmek için komutu. Emin olun tüm özellikleri `az vmss create` komutu, aynı önce olduğu gibi değiştirmek istediğiniz özellikleri dışında.

- Ayrıca [resources.azure.com](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/).

Ölçek kümesi modeline güncelleştirildikten sonra yeni yapılandırmayı ölçek kümesinde oluşturulan tüm yeni vm'lere uygulanır. Ancak, modelleri ölçek kümesindeki var olan VM'ler için hala son genel ölçek kümesi modeli ile güncel sunulmalıdır. Modeldeki her bir VM için bir boolean özelliği çağrılır `latestModelApplied` VM son genel ölçek kümesi modeli ile güncel olup olmadığını gösterir (`true` VM en son modeliyle güncel olduğu anlamına gelir).


## <a name="how-to-bring-vms-up-to-date-with-the-latest-scale-set-model"></a>VM'ler ile en son ölçek kümesi modeline güncel hale getirme
Ölçek kümesine sahip bir "Yükseltme İlkesi" belirleyen nasıl Vm'leri son ölçek kümesi modeline güncel duruma getirilir. Yükseltme ilkesi için üç mod şunlardır:

- **Otomatik** -bu modda, Ölçek kümesi Vm'leri getirilen sırasını tutarlılık garantisi sağlar. Ölçek kümesi tüm VM'ler aynı anda sürebilir. 
- **Sıralı** -bu modda, Ölçek kümesi isteğe bağlı duraklatma süresi toplu güncelleştirmesi kullanıma arasında toplu işlemler halinde dökümünü yapan.
- **El ile** - bu modda, Ölçek kümesi modeline mevcut Vm'lere herhangi işlem gerçekleşmediyse güncelleştirdiğinizde.
 
Var olan sanal makineleri güncelleştirmek için var olan her VM'nin "el ile yükseltme" yapmanız gerekir. Bu el ile yükseltme ile bunu yapabilirsiniz:

- REST API ile [virtualmachinescalesets/işlem/updateinstances](/rest/api/compute/virtualmachinescalesets/updateinstances) gibi:

    ```rest
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/manualupgrade?api-version={apiVersion}
    ```

- Azure PowerShell ile [güncelleştirme AzVmssInstance](/powershell/module/az.compute/update-azvmssinstance):
    
    ```powershell
    Update-AzVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId
    ```

- Azure CLI ile [az vmss update-instances](/cli/azure/vmss)

    ```azurecli
    az vmss update-instances --resource-group myResourceGroup --name myScaleSet --instance-ids {instanceIds}
    ```

- Dile özgü kullanabilirsiniz [Azure SDK'ları](https://azure.microsoft.com/downloads/).

>[!NOTE]
> Service Fabric kümeleri yalnızca kullanma *otomatik* modu, ancak güncelleştirme farklı şekilde ele alınır. Daha fazla bilgi için [Service Fabric uygulama yükseltme](../service-fabric/service-fabric-application-upgrade.md).

Yükseltme İlkesi izlemez değişikliği küresel ölçek özellikleri ayarlamak için bir tür yoktur. İşletim sistemi profili (örneğin, yönetici kullanıcı adı ve parola) API sürümünde yalnızca değiştirilebilir bir ölçek kümesi değişiklikleri *2017-12-01* veya üzeri. Bu değişiklikler, yalnızca değişikliği ölçek kümesi modelinde sonra oluşturulan Vm'lere uygulanır. Varolan Vm'leri güncel duruma getirmek için var olan her VM'nin "reimage" yapmanız gerekir. Bu reimage aracılığıyla yapabilirsiniz:

- REST API ile [virtualmachinescalesets/işlem/reimage](/rest/api/compute/virtualmachinescalesets/reimage) gibi:

    ```rest
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/reimage?api-version={apiVersion}
    ```

- Azure PowerShell ile [kümesi AzVmssVm](https://docs.microsoft.com/powershell/module/az.compute/set-azvmssvm):

    ```powershell
    Set-AzVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId -Reimage
    ```

- Azure CLI ile [az vmss reimage](https://docs.microsoft.com/cli/azure/vmss):

    ```azurecli
    az vmss reimage --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- Dile özgü kullanabilirsiniz [Azure SDK'ları](https://azure.microsoft.com/downloads/).


## <a name="properties-with-restrictions-on-modification"></a>Değişiklik kısıtlamalar ile özellikleri

### <a name="create-time-properties"></a>Oluşturma zamanı özellikleri
Ölçek kümesi oluşturduğunuzda, bazı özellikler yalnızca ayarlanabilir. Bu özellikler şunları içerir:

- Kullanılabilirlik Alanları
- Görüntü başvurusu yayımcısı
- Görüntü başvurusu teklifi
- Yönetilen işletim sistemi disk depolama hesabı türü

### <a name="properties-that-can-only-be-changed-based-on-the-current-value"></a>Geçerli değere göre yalnızca değiştirilen Özellikler
İstisnalar geçerli değerine bağlı olarak bazı özellikleri değiştirilebilir. Bu özellikler şunları içerir:

- **singlePlacementGroup** - singlePlacementGroup doğruysa, değiştirilebilir false. Ancak, singlePlacementGroup yanlış ise, **olmayabilir** değiştirilmesine true.
- **alt ağ** - alt ağ, bir ölçek kümesinin olduğu sürece özgün alt değiştirilebilir ve yeni alt ağ olan aynı sanal ağ içinde.

### <a name="properties-that-require-deallocation-to-change"></a>Ayırmayı kaldırma değiştirmek için gereken özellikleri
Ölçek kümesindeki sanal makineler serbest bırakılır, bazı özellikler yalnızca belirli değerleri için değiştirilebilir. Bu özellikler şunları içerir:

- **SKU adı**- yeni sanal makine SKU'su, Ölçek kümesinde şu anda, Vm'leri ölçek SKU adı değiştirmeden önce serbest gereken donanım üzerinde desteklenmiyor. Daha fazla bilgi için [bir Azure VM'yi yeniden boyutlandırma](../virtual-machines/windows/resize-vm.md).


## <a name="vm-specific-updates"></a>Sanal Makineye özgü güncelleştirmeleri
Küresel ölçek kümesi özellikleri yerine belirli VM'ler için bazı değişiklikleri uygulanabilir. Şu anda, yalnızca desteklenen sanal Makineye özgü güncelleştirme İliştir/Ayır ölçek kümesine veri diski/vm'lerden etmektir. Bu özellik önizlemede. Daha fazla bilgi için [Önizleme belgeleri](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).


## <a name="scenarios"></a>Senaryolar

### <a name="application-updates"></a>Uygulama güncelleştirmeleri
Bir uygulama için bir ölçek kümesi uzantıları dağıtılırsa, uzantı yapılandırması için bir güncelleştirme yükseltme ilkesiyle uygun şekilde güncelleştirmek uygulama neden olur. Bir özel betik uzantısı'nda çalışacak bir komut dosyası yeni bir sürümü varsa, örneğin, güncelleştiremedi *fileUris* yeni komut dosyasına işaret edecek şekilde özelliği. Bazı durumlarda, uzantı yapılandırması değiştirilmemiş olsa bile bir güncelleştirmeyi zorlamak isteyebilirsiniz (örneğin, betik URI'si için bir değişiklik yapmadan betik güncelleştirilmiş). Bu durumlarda, değiştirebileceğiniz *forceUpdateTag* bir güncelleştirmeyi zorlamak için. Azure platformunda bu özellik yorumlamaz. Değeri değiştirirseniz, uzantıyı nasıl çalıştığı üzerinde hiçbir etkisi yoktur. Bir değişiklik, yalnızca yeniden çalıştırmak için uzantı zorlar. Daha fazla bilgi için *forceUpdateTag*, bkz: [uzantıları için REST API belgelerini](/rest/api/compute/virtualmachineextensions/createorupdate). Unutmayın *forceUpdateTag* tüm uzantılar, yalnızca özel betik uzantısı ile kullanılabilir.

Özel bir görüntü dağıtılan uygulamalar için de yaygındır. Bu senaryo, aşağıdaki bölümde ele alınmıştır.

### <a name="os-updates"></a>İşletim sistemi güncelleştirmeleri
Azure platform görüntüleri kullanıyorsanız değiştirerek görüntüyü güncelleştirebilirsiniz *Imagereference* (daha fazla bilgi için bkz: [REST API belgelerini](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate)).

>[!NOTE]
> Platform görüntüleri ile görüntü başvurusu sürümü için "son" belirtmek yaygındır. Oluşturduğunuzda, ölçeği genişletme ve yeniden görüntü oluşturma, Vm'leri ile kullanılabilir en son sürüme oluşturulur. Ancak, bunu **yok** yeni görüntü sürümleri çıktıkça işletim sistemi görüntüsünü otomatik olarak zaman içinde güncelleştirilir anlamına gelir. Ayrı bir özellik, şu anda otomatik işletim sistemi yükseltmelerini sağlayan Önizleme aşamasındadır. Daha fazla bilgi için [otomatik işletim sistemi yükseltmelerini belgeleri](virtual-machine-scale-sets-automatic-upgrade.md).

Özel görüntüler kullanıyorsanız, görüntüyü güncelleştirerek güncelleştirebilirsiniz *Imagereference* kimliği (daha fazla bilgi için bkz: [REST API belgelerini](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate)).

## <a name="examples"></a>Örnekler

### <a name="update-the-os-image-for-your-scale-set"></a>Ölçek kümeniz için işletim sistemi görüntüsü güncelleştirme
Ubuntu LTS, 16.04 eski bir sürümünü çalıştıran bir ölçek kümesi olabilir. Ubuntu LTS 16.04 sürümü gibi daha yeni bir sürüme güncelleştirmek istediğiniz *16.04.201801090*. Bu özellikler aşağıdaki komutlardan birini ile doğrudan değiştirebilmesi görüntü başvurusu version özelliği bir listenin bir parçası değil:

- Azure PowerShell ile [güncelleştirme AzVmss](/powershell/module/az.compute/update-azvmss) gibi:

    ```powershell
    Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -ImageReferenceVersion 16.04.201801090
    ```

- Azure CLI ile [az vmss update](/cli/azure/vmss):

    ```azurecli
    az vmss update --resource-group myResourceGroup --name myScaleSet --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090
    ```

Alternatif olarak, Ölçek kümenizi kullanan resmi değiştirmek isteyebilirsiniz. Örneğin, güncelleştirmek veya ölçek kümeniz tarafından kullanılan özel bir görüntü değiştirmek isteyebilirsiniz. Görüntünün görüntü başvuru kimliği özelliği güncelleştirerek ölçek kümenizi kullanan değiştirebilirsiniz. Bu özellik aşağıdaki komutlardan birini ile doğrudan değiştirebilmesi görüntü başvuru kimliği özelliği bir listenin bir parçası değil:

- Azure PowerShell ile [güncelleştirme AzVmss](/powershell/module/az.compute/update-azvmss) gibi:

    ```powershell
    Update-AzVmss `
        -ResourceGroupName "myResourceGroup" `
        -VMScaleSetName "myScaleSet" `
        -ImageReferenceId /subscriptions/{subscriptionID}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myNewImage
    ```

- Azure CLI ile [az vmss update](/cli/azure/vmss):

    ```azurecli
    az vmss update \
        --resource-group myResourceGroup \
        --name myScaleSet \
        --set virtualMachineProfile.storageProfile.imageReference.id=/subscriptions/{subscriptionID}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myNewImage
    ```


### <a name="update-the-load-balancer-for-your-scale-set"></a>Ölçek kümeniz için Yük Dengeleyiciyi güncelleştirin
Bir ölçek kümesi Azure Load Balancer ile sahip olduğunuz ve Azure Application Gateway ile Azure Load Balancer değiştirmek istediğiniz varsayalım. Bir ölçek kümesi için uygulama ağ geçidi özellikleri ve yük dengeleyici özellikleri doğrudan değiştirme yerine liste öğeleri eklemek veya kaldırmak için komutları kullanabilirsiniz bir listenin bir parçası olduğundan:

- Azure Powershell:

    ```powershell
    # Get the current model of the scale set and store it in a local PowerShell object named $vmss
    $vmss=Get-AzVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet"
    
    # Create a local PowerShell object for the new desired IP configuration, which includes the reference to the application gateway
    $ipconf = New-AzVmssIPConfig "myNic" -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name
    
    # Replace the existing IP configuration in the local PowerShell object (which contains the references to the current Azure Load Balancer) with the new IP configuration
    $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf
    
    # Update the model of the scale set with the new configuration in the local PowerShell object
    Update-AzVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -virtualMachineScaleSet $vmss
    ```

- Azure CLI:

    ```azurecli
    # Remove the load balancer backend pool from the scale set model
    az vmss update --resource-group myResourceGroup --name myScaleSet --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0
    
    # Remove the load balancer backend pool from the scale set model; only necessary if you have NAT pools configured on the scale set
    az vmss update --resource-group myResourceGroup --name myScaleSet --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0
    
    # Add the application gateway backend pool to the scale set model
    az vmss update --resource-group myResourceGroup --name myScaleSet --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}'
    ```

>[!NOTE]
> Bu komutlar, Ölçek kümesinde yalnızca bir IP yapılandırması ve yük dengeleyici olduğu kabul edilmektedir. Varsa birden çok, bir liste dizini dışındaki kullanmanız gerekebilir *0*.


## <a name="next-steps"></a>Sonraki adımlar
Ölçek kümeleriyle üzerinde genel yönetim görevlerini gerçekleştirebilirsiniz [Azure CLI](virtual-machine-scale-sets-manage-cli.md) veya [Azure PowerShell](virtual-machine-scale-sets-manage-powershell.md).
