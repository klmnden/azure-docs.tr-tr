---
title: Bir Azure sanal makine ölçek kümesini değiştirme | Microsoft Docs
description: Değiştirme ve REST API'leri, Azure PowerShell ve Azure CLI 2.0 ayarlayın bir Azure sanal makine ölçek güncelleştirmek hakkında bilgi edinin
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
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
ms.author: negat
ms.openlocfilehash: bfbcf8ff3f24b69b49b9a2bd5d567e1ead57d974
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="modify-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesini değiştirme
Uygulamalarınızı yaşam döngüsü boyunca değiştirmeniz veya sanal makine ölçek kümesi güncelleştirmeniz gerekebilir. Bu güncelleştirmeler ölçek kümesi yapılandırmasını güncelleştirmek veya uygulama yapılandırmasını değiştirmek nasıl içerebilir. Bu makalede, varolan bir ölçeği REST API'leri, Azure PowerShell veya Azure CLI 2.0 ayarla değiştirmek açıklar.

## <a name="fundamental-concepts"></a>temel kavramlar

### <a name="the-scale-set-model"></a>Ölçek kümesi modeli
Bir ölçek "yakalayan bir ölçek kümesi modeli" olarak ayarlanmış *istenen* ölçek durumunu bir bütün olarak ayarlayın. Modelin bir ölçek kümesi için sorgu için kullanabilirsiniz 

- REST API'si ile [virtualmachinescalesets/işlem/get](/rest/api/compute/virtualmachinescalesets/get) gibi:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version={apiVersion}
    ```

- Azure PowerShell ile [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss):

    ```powershell
    Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
    ```

- Azure CLI 2.0 ile [az vmss Göster](/cli/azure/vmss#az_vmss_show):

    ```azurecli
    az vmss show --resource-group myResourceGroup --name myScaleSet
    ```

- Aynı zamanda [resources.azure.com](https://resources.azure.com) veya dile özgü [Azure SDK'ları](https://azure.microsoft.com/downloads/).

Sağladığınız komut için seçenekleri çıkış tam sunu bağlıdır. Aşağıdaki örnek Azure CLI 2. 0 sıkıştırılmış örnek çıkış şunları gösterir:

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

Bu özellikleri ölçeğin bir bütün olarak ayarlamak için geçerlidir.


### <a name="the-scale-set-instance-view"></a>Ölçek kümesi örnek görünümü
Ayrıca bir ölçek "görüntülemek bir ölçek kümesi örnek" geçerli yakalayan sahip *çalışma zamanı* ölçek durumunu bir bütün olarak ayarlayın. Ölçek kümesi örnek görünümünü sorgulamak için aşağıdaki komutu kullanabilirsiniz:

- REST API'si ile [virtualmachinescalesets/işlem/getinstanceview](/rest/api/compute/virtualmachinescalesets/getinstanceview) gibi:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/instanceView?api-version={apiVersion}
    ```

- Azure PowerShell ile [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss):

    ```powershell
    Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceView
    ```

- Azure CLI 2.0 ile [az vmss get-örnek-görünümü](/cli/azure/vmss#az_vmss_get_instance_view):

    ```azurecli
    az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet
    ```

- Aynı zamanda [resources.azure.com](https://resources.azure.com) veya dile özgü [Azure SDK'ları](https://azure.microsoft.com/downloads/)

Sağladığınız komut için seçenekleri çıkış tam sunu bağlıdır. Aşağıdaki örnek Azure CLI 2. 0 sıkıştırılmış örnek çıkış şunları gösterir:

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

Bu özellikler, Ölçek kümesine uygulanan uzantıları durumu gibi ölçek kümesindeki sanal makineleri geçerli çalışma zamanı durumunun bir özetini sağlar.


### <a name="the-scale-set-vm-model-view"></a>Ölçek kümesi VM model görünümü
Benzer şekilde nasıl bir model görünümü ölçek kümesi vardır, her bir VM ölçek kümesindeki kendi modeli görünüme sahiptir. Ölçek kümesi için model view sorgulamak için aşağıdaki komutu kullanabilirsiniz:

- REST API'si ile [virtualmachinescalesetvms/işlem/get](/rest/api/compute/virtualmachinescalesetvms/get) gibi:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualmachines/instanceId?api-version={apiVersion}
    ```

- Azure PowerShell ile [Get-AzureRmVmssVm](/powershell/module/azurerm.compute/get-azurermvmssvm):

    ```powershell
    Get-AzureRmVmssVm -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId
    ```

- Azure CLI 2.0 ile [az vmss Göster](/cli/azure/vmss#az_vmss_show):

    ```azurecli
    az vmss show --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- Aynı zamanda [resources.azure.com](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/).

Sağladığınız komut için seçenekleri çıkış tam sunu bağlıdır. Aşağıdaki örnek Azure CLI 2. 0 sıkıştırılmış örnek çıkış şunları gösterir:

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

Bu özellikleri VM kendisi yapılandırma bir bütün olarak ayarlayın ve ölçeğin yapılandırmasını açıklar. Örneğin, Ölçek kümesi modeli vardır `overprovision` model için bir VM ölçek kümesindeki çalışmazken bir özellik olarak. İşleminin tamamı, bireysel VM ölçek kümesindeki yap ölçek için bir özellik olduğundan bu farktır (işleminin hakkında daha fazla bilgi için bkz: [tasarım konuları ölçek kümeleri için](virtual-machine-scale-sets-design-overview.md#overprovisioning)).


### <a name="the-scale-set-vm-instance-view"></a>Ölçek kümesi VM örnek görünümü
Ölçek kümesindeki her bir VM ölçek kümesi örnek görünümünü nasıl sahip benzer, kendi örneği görünüme sahiptir. Ölçek kümesi örnek görünümünü sorgulamak için aşağıdaki komutu kullanabilirsiniz:

- REST API'si ile [virtualmachinescalesetvms/işlem/getinstanceview](/rest/api/compute/virtualmachinescalesetvms/getinstanceview) gibi:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualmachines/instanceId/instanceView?api-version={apiVersion}
    ```

- Azure PowerShell ile [Get-AzureRmVmssVm](/powershell/module/azurerm.compute/get-azurermvmssvm):

    ```powershell
    Get-AzureRmVmssVm -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId -InstanceView
    ```

- Azure CLI 2.0 ile [az vmss get-örnek-görünümü](/cli/azure/vmss#az_vmss_get_instance_view)

    ```azurecli
    az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- Aynı zamanda [resources.azure.com](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/)

Sağladığınız komut için seçenekleri çıkış tam sunu bağlıdır. Aşağıdaki örnek Azure CLI 2. 0 sıkıştırılmış örnek çıkış şunları gösterir:

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

Bu özellikler ölçek kümesine uygulanan tüm uzantılar içeren VM kendisi, geçerli çalışma zamanı durumunu tanımlar.


## <a name="how-to-update-global-scale-set-properties"></a>Genel ölçeği güncelleştirme özelliklerini ayarlama
Bir genel ölçek kümesi özelliği güncelleştirmek için ölçek kümesi modelinde özelliğinde güncelleştirmeniz gerekir. Bu güncelleştirme aracılığıyla yapabilirsiniz:

- REST API'si ile [virtualmachinescalesets/işlem/createorupdate](/rest/api/compute/virtualmachinescalesets/createorupdate) gibi:

    ```rest
    PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version={apiVersion}
    ```

- Resource Manager şablonu genel ölçek kümesi özelliklerini güncelleştirmek için REST API özelliklerinden ile dağıtabilirsiniz.

- Azure PowerShell ile [güncelleştirme AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss):

    ```powershell
    Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -VirtualMachineScaleSet {scaleSetConfigPowershellObject}
    ```

- Azure CLI 2.0 ile [az vmss güncelleştirme](/cli/azure/vmss#az_vmss_update):
    - Bir özelliği değiştirmek için:

        ```azurecli
        az vmss update --set {propertyPath}={value}
        ```

    - Bir liste özelliği ölçek kümesindeki bir nesne eklemek için: 

        ```azurecli
        az vmss update --add {propertyPath} {JSONObjectToAdd}
        ```

    - Bir nesne ölçek kümesindeki bir liste özelliği kaldırmak için: 

        ```azurecli
        az vmss update --remove {propertyPath} {indexToRemove}
        ```

    - Ölçeği ile Ayarla daha önce dağıttıysanız `az vmss create` komutunu çalıştırabilirsiniz `az vmss create` yeniden ölçek kümesini Güncelleştir komutu. Olduğundan emin olun tüm özellikleri `az vmss create` komutu aynıdır önceki gibi değiştirmek istediğiniz özellikleri dışında.

- Aynı zamanda [resources.azure.com](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/).

Ölçek kümesi modeli güncelleştirildikten sonra yeni yapılandırmayı ölçek kümesinde oluşturulan yeni Vm'leri uygular. Ancak, modelleri ölçek kümesindeki var olan VM'ler için hala son genel ölçek kümesi modeli ile güncel duruma getirilmesi gerekir. Modeldeki her bir VM için bir boolean özelliği adlı `latestModelApplied` VM son genel ölçek kümesi modelle güncel olup olmadığını gösterir (`true` VM son modeliyle güncel olduğu anlamına gelir).


## <a name="how-to-bring-vms-up-to-date-with-the-latest-scale-set-model"></a>Sanal makineleri son ölçek kümesi modeliyle güncel duruma getirmek nasıl
Ölçek kümesine sahip bir "Yükseltme İlkesi" belirleyen nasıl VM'ler son ölçek kümesi modeli ile güncel duruma getirilir. Yükseltme ilkesi için üç mod şunlardır:

- **Otomatik** -bu modda, Ölçek kümesini garanti duruma VM'ler sırası hakkında yapar. Ölçek kümesi aynı anda tüm sanal makineleri sürebilir. 
- **Çalışırken** -bu modda, Ölçek kümesini bir isteğe bağlı duraklatma süresiyle toplu güncelleştirme arasında toplu ortalarında dağıtır.
- **El ile** - bu modda, var olan VM'ler için hiçbir şey olmaz ölçek kümesi modelinde güncelleştirdiğinizde.
 
Var olan sanal makineleri güncelleştirmek için varolan her VM "el ile yükseltme" yapmanız gerekir. İle bu el ile yükseltme yapabilirsiniz:

- REST API'si ile [virtualmachinescalesets/işlem/updateinstances](/rest/api/compute/virtualmachinescalesets/updateinstances) gibi:

    ```rest
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/manualupgrade?api-version={apiVersion}
    ```

- Azure PowerShell ile [güncelleştirme AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance):
    
    ```powershell
    Update-AzureRmVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId
    ```

- Azure CLI 2.0 ile [az vmss güncelleştirme-örnekleri](/cli/azure/vmss#az_vmss_update_instances)

    ```azurecli
    az vmss update-instances --resource-group myResourceGroup --name myScaleSet --instance-ids {instanceIds}
    ```

- Dile özgü de kullanabilirsiniz [Azure SDK'ları](https://azure.microsoft.com/downloads/).

>[!NOTE]
> Service Fabric kümeleri yalnızca kullanabilir *otomatik* mod, ancak güncelleştirme farklı şekilde ele alınır. Daha fazla bilgi için bkz: [ Service Fabric uygulama yükseltme](../service-fabric/service-fabric-application-upgrade.md).

Bir tür Özellikleri Genel ölçeği ayarlamak için yükseltme İlkesi izlemez değişiklik yoktur. İşletim sistemi profili (örneğin, yönetici kullanıcı adı ve parola) API sürümünde yalnızca değiştirilebilir ölçek kümesini değişiklikler *2017-12-01* veya sonraki bir sürümü. Bu değişiklikler yalnızca model ölçek değişikliği ayarladıktan sonra oluşturulan sanal makineleri için geçerlidir. Var olan VM'ler güncel duruma getirmek için bir "yeniden görüntü oluşturma" var olan her VM yapmanız gerekir. Bu yeniden görüntü oluşturma aracılığıyla yapabilirsiniz:

- REST API'si ile [işlem/virtualmachinescalesets/yeniden görüntü oluşturma](/rest/api/compute/virtualmachinescalesets/reimage) gibi:

    ```rest
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/reimage?api-version={apiVersion}
    ```

- Azure PowerShell ile [kümesi AzureRmVmssVm](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm):

    ```powershell
    Set-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId -Reimage
    ```

- Azure CLI 2.0 ile [az vmss yeniden görüntü oluşturma](https://docs.microsoft.com/cli/azure/vmss#az_vmss_reimage):

    ```azurecli
    az vmss reimage --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- Dile özgü de kullanabilirsiniz [Azure SDK'ları](https://azure.microsoft.com/downloads/).


## <a name="properties-with-restrictions-on-modification"></a>Özellikleri değiştirme kısıtlamalar ile

### <a name="create-time-properties"></a>Oluşturma zamanı özellikleri
Bazı özellikler yalnızca ölçek kümesi oluşturduğunuzda ayarlayabilirsiniz. Bu özellikler şunları içerir:

- Kullanılabilirlik Alanları
- Yansıma başvurusu yayımcı
- Yansıma başvurusu teklif
- Yönetilen işletim sistemi disk depolama hesabı türü

### <a name="properties-that-can-only-be-changed-based-on-the-current-value"></a>Geçerli değere göre yalnızca değiştirilebilir özellikleri
Özel durumlar dışında geçerli değerine bağlı olarak bazı özellikleri değiştirilebilir. Bu özellikler şunları içerir:

- **singlePlacementGroup** - singlePlacementGroup doğruysa, değiştirilmesi false. Ancak, singlePlacementGroup yanlışsa, onu **olmayabilir** değiştirilmesi true.
- **alt ağ** - ölçek kümesini alt sürece özgün alt ağ olarak değiştirilebilir ve yeni bir alt ağ olan aynı sanal ağ içinde.

### <a name="properties-that-require-deallocation-to-change"></a>Ayırmayı kaldırma değiştirmek için gereken özellikleri
Ölçek kümesindeki sanal makineleri serbest varsa bazı özellikler yalnızca belirli değerler değiştirilebilir. Bu özellikler şunları içerir:

- **SKU adı**- yeni VM SKU ölçek kümesi şu anda, ölçeği SKU adı değiştirmeden önce Ayarla içindeki VM'ler ayırması gereken donanım desteklenmiyor. Daha fazla bilgi için bkz: [bir Azure VM yeniden boyutlandırmak nasıl](../virtual-machines/windows/resize-vm.md).


## <a name="vm-specific-updates"></a>VM özgü güncelleştirmeleri
Bazı değişiklikleri Özellikleri Genel ölçeği ayarlamak yerine belirli VM'ler için uygulanabilir. Şu anda, yalnızca desteklenen VM özgü güncelleştirme ölçek kümesi'ndeki veri disklerinin VM'ler/ekleme/ayırma etmektir. Bu özelliğin önizlemede değil. Daha fazla bilgi için bkz: [Önizleme belgelerine](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).


## <a name="scenarios"></a>Senaryolar

### <a name="application-updates"></a>Uygulama güncelleştirmeleri
Bir uygulama ölçeği uzantıları Ayarla dağıtılırsa, uzantı yapılandırması için bir güncelleştirme yükseltme ilkesiyle uygun şekilde güncelleştirmek uygulamanın neden olur. Örneğin, bir özel betik uzantısı'nda çalıştırılacak bir komut dosyası yeni bir sürümü varsa, güncelleştirme *fileUris* yeni komut dosyasına işaret edecek şekilde özelliği. Bazı durumlarda, uzantı yapılandırması değişmeden olsa bile bir güncelleştirmeyi uygulamak isteyebilir (örneğin, betik URI'si için bir değişiklik olmadan betik güncelleştirilir). Bu durumlarda, değiştirebileceğiniz *forceUpdateTag* bir güncelleştirmeyi uygulamak için. Azure platformu, bu özellik yorumlar değil. Değeri değiştirirseniz, uzantının nasıl çalışacağını üzerinde hiçbir etkisi yoktur. Bir değişiklik, yalnızca yeniden çalıştırmak için uzantı zorlar. Daha fazla bilgi için *forceUpdateTag*, bkz: [uzantıları için REST API belgeleri](/rest/api/compute/virtualmachineextensions/createorupdate). Unutmayın *forceUpdateTag* tüm uzantılar, yalnızca özel betik uzantısı ile kullanılabilir.

Ayrıca özel bir görüntü dağıtılacak uygulamalar yaygındır. Bu senaryo aşağıdaki bölümde ele alınmıştır.

### <a name="os-updates"></a>İşletim sistemi güncelleştirmeleri
Azure platformu görüntüleri kullanırsanız değiştirerek görüntü güncelleştirebilirsiniz *Imagereference* (daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

>[!NOTE]
> Platform görüntülerle "son" için resim başvurusu sürümünü belirtmek için yaygın bir sorundur. Oluşturduğunuzda, ölçeğini ve yeniden görüntü oluşturma, VM'ler ile en son sürüm oluşturulur. Ancak, bu **yok** yeni görüntü sürümleri yayımlanan gibi işletim sistemi görüntüsü zaman içinde otomatik olarak güncelleştirilen anlamına gelir. Ayrı bir özellik şu anda otomatik işletim sistemi yükseltme sağlar önizlemede değil. Daha fazla bilgi için bkz: [otomatik işletim sistemi yükseltme belgelerine](virtual-machine-scale-sets-automatic-upgrade.md).

Özel resimler kullanırsanız, görüntüyü güncelleştirerek güncelleştirebilirsiniz *Imagereference* kimliği (daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

## <a name="examples"></a>Örnekler

### <a name="update-the-os-image-for-your-scale-set"></a>İşletim sistemi görüntüsü, Ölçek kümesi için güncelleştirme
Ubuntu LTS 16.04 eski bir sürümünü çalıştıran bir ölçek kümesine sahip. Ubuntu LTS 16.04 sürümü gibi daha yeni bir sürüme güncelleştirmek istediğiniz *16.04.201801090*. Bu özellikler aşağıdaki komutlardan birini ile doğrudan değiştirebileceğiniz şekilde görüntü başvurusu version özelliği listesinin bir parçası değil:

- Azure PowerShell ile [güncelleştirme AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) gibi:

    ```powershell
    Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -ImageReferenceVersion 16.04.201801090
    ```

- Azure CLI 2.0 ile [az vmss güncelleştirme](/cli/azure/vmss#az_vmss_update_instances):

    ```azurecli
    az vmss update --resource-group myResourceGroup --name myScaleSet --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090
    ```


### <a name="update-the-load-balancer-for-your-scale-set"></a>Yük Dengeleyici, Ölçek kümesi için güncelleştirme
Bir Azure yük dengeleyicisi ile ölçek varsa ve Azure yük dengeleyici Azure uygulama ağ geçidi ile değiştirmek istediğiniz varsayalım. Kaldırın veya özellikleri doğrudan değiştirme yerine liste öğelerini eklemek için komutları kullanabilmeniz için yük dengeleyici ve ölçek kümesi için uygulama ağ geçidi özellikleri listesinin bir parçası şunlardır:

- Azure Powershell:

    ```powershell
    # Get the current model of the scale set and store it in a local PowerShell object named $vmss
    $vmss=Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet"
    
    # Create a local PowerShell object for the new desired IP configuration, which includes the referencerence to the application gateway
    $ipconf = New-AzureRmVmssIPConfig "myNic" -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name
    
    # Replace the existing IP configuration in the local PowerShell object (which contains the references to the current Azure Load Balancer) with the new IP configuration
    $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf
    
    # Update the model of the scale set with the new configuration in the local PowerShell object
    Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -virtualMachineScaleSet $vmss
    ```

- Azure CLI 2.0:

    ```azurecli
    # Remove the load balancer backend pool from the scale set model
    az vmss update --resource-group myResourceGroup --name myScaleSet --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0
    
    # Remove the load balancer backend pool from the scale set model; only necessary if you have NAT pools configured on the scale set
    az vmss update --resource-group myResourceGroup --name myScaleSet --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0
    
    # Add the application gateway backend pool to the scale set model
    az vmss update --resource-group myResourceGroup --name myScaleSet --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}'
    ```

>[!NOTE]
> Bu komutlar, Ölçek kümesinde yalnızca bir IP yapılandırması ve yük dengeleyici olduğunu varsayalım. Varsa birden çok, bir liste dizini dışındaki kullanmanız gerekebilir *0*.


## <a name="next-steps"></a>Sonraki adımlar
Ölçek kümeleri üzerinde genel yönetim görevleri gerçekleştirebilir [Azure CLI 2.0](virtual-machine-scale-sets-manage-cli.md) veya [Azure PowerShell](virtual-machine-scale-sets-manage-powershell.md).
