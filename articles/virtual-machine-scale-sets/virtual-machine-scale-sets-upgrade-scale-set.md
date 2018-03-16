---
title: "Bir Azure sanal makine ölçek kümesini değiştirme | Microsoft Docs"
description: "Bir Azure sanal makine ölçek kümesini değiştirme"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: negat
ms.openlocfilehash: fcca912a8120a51d2f0a454ef0a6341cd5882015
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="modify-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesini değiştirme
Bu makalede, varolan bir sanal makine ölçek kümesini değiştirmek açıklar. Görevler, Ölçek yapılandırmasını değiştirmek nasıl içerir, ölçeğini çalışan uygulamalara yapılandırmasını değiştirmek nasıl set, kullanılabilirlik ve daha fazlasını nasıl yönetileceği.

## <a name="fundamental-concepts"></a>temel kavramlar

### <a name="scale-set-model"></a>Model ölçek kümesi

Bir ölçek yakalayan bir model olarak ayarlanmış *istenen* ölçek durumunu bir bütün olarak ayarlayın. Modelin bir ölçek kümesi için sorgu için kullanabilirsiniz:

* REST API: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` 
   
  Daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/get).

* PowerShell:

  `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}`
   
  Daha fazla bilgi için bkz: [PowerShell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss).

* Azure CLI: 

  `az vmss show -g {resourceGroupName} -n {vmSaleSetName}` 
   
  Daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show).

De kullanabilirsiniz [Azure kaynak Gezgini (Önizleme)](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/) modeli ölçek kümesi için sorgulanamıyor.

Çıkış tam sunu komutu sağlayan seçenekleri bağlıdır. Azure CLI örnek çıktısı şöyledir:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
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
  .
  .
  .
}
```

Gördüğünüz gibi bu özellikleri ölçeğin bir bütün olarak ayarlamak için geçerlidir.



### <a name="scale-set-instance-view"></a>Ölçek kümesi örnek görünümü

Ayrıca bir ölçek geçerli yakalayan bir örnek görünümünü sahip *çalışma zamanı* ölçek durumunu bir bütün olarak ayarlayın. Ölçek kümesi örnek görünümünü sorgulamak için aşağıdaki komutu kullanabilirsiniz:

* REST API: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/instanceView?api-version={apiVersion}` 
   
  Daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getinstanceview).

* PowerShell: 

  `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceView` 
  
  Daha fazla bilgi için bkz: [PowerShell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss).

* Azure CLI: 

  `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName}` 
   
  Daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view).

Aynı zamanda [Azure kaynak Gezgini (Önizleme)](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/) ölçek kümesi örnek görünümünü sorgulanamıyor.

Çıkış tam sunu komutu sağlayan seçenekleri bağlıdır. Azure CLI örnek çıktısı şöyledir:

```
$ az vmss get-instance-view -g {resourceGroupName} -n {virtualMachineScaleSetName}
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
  .
  .
  .
}
```

Gördüğünüz gibi bu özellikleri ölçek kümesindeki sanal makineleri geçerli çalışma zamanı durumunun bir özetini sağlar. Özet (okumanızdır belirtilmemiş) kümesini uygulanan uzantıları durumunu içerir.



### <a name="scale-set-vm-model-view"></a>Ölçek kümesi VM model görünümü

Benzer şekilde nasıl bir model görünümü ölçek kümesi vardır, her bir VM ölçek kümesindeki kendi modeli görünüme sahiptir. Ölçek kümesi için model view sorgulamak için aşağıdaki komutu kullanabilirsiniz:

* REST API: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}?api-version={apiVersion}` 
  
  Daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/get).

* PowerShell: 

  `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` 
  
  Daha fazla bilgi için bkz: [PowerShell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm).

* Azure CLI: 

  `az vmss show -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` 
  
  Daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show).

De kullanabilirsiniz [Azure kaynak Gezgini (Önizleme)](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/) bir VM ölçek kümesindeki için model sorgulanamıyor.

Çıkış tam sunu komutu sağlayan seçenekleri bağlıdır. Azure CLI örnek çıktısı şöyledir:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
{
  "location": "westus",
  "name": "{name}",
  "sku": {
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
  .
  .
  .
}
```

Gördüğünüz gibi bu özellikleri VM kendisi yapılandırma bir bütün olarak ayarlayın ve ölçeğin yapılandırmasını açıklar. Örneğin, Ölçek kümesi modeli vardır `overprovision` model için bir VM ölçek kümesi ise bir özellik olarak desteklemez. Bu farkı nedeni işleminin tamamı, bireysel VM ölçek kümesindeki yap ölçek için bir özellik olmasıdır. (İşleminin hakkında daha fazla bilgi için bkz: [tasarım konuları ölçek kümeleri için](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning).)



### <a name="scale-set-vm-instance-view"></a>Ölçek kümesi VM örnek görünümü

Ölçek kümesindeki her bir VM ölçek kümesi örnek görünümünü nasıl sahip benzer, kendi örneği görünüme sahiptir. Ölçek kümesi örnek görünümünü sorgulamak için aşağıdaki komutu kullanabilirsiniz:

* REST API: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}/instanceView?api-version={apiVersion}` 
 
  Daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/getinstanceview).

* PowerShell: 

  `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -InstanceView` 
  
  Daha fazla bilgi için bkz: [PowerShell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm).

* Azure CLI: 

  `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` 
  
  Daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view).

Aynı zamanda [Azure kaynak Gezgini (Önizleme)](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/) sorgu ölçek kümesindeki VM örneği görünümüne.

Çıkış tam sunu komutu sağlayan seçenekleri bağlıdır. Azure CLI örnek çıktısı şöyledir:

```
$ az vmss get-instance-view -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}
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
  .
  .
  .
}
```

Gördüğünüz gibi bu özellikleri VM geçerli çalışma zamanı durumunu açıklar. Durumu (okumanızdır belirtilmemiş) kümesini uygulanan tüm uzantılar içerir.




## <a name="techniques-for-updating-global-scale-set-properties"></a>Genel ölçeği güncelleştirme teknikleri özelliklerini ayarlama

Bir genel ölçek kümesi özelliği güncelleştirmek için ölçek kümesi modelinde özelliğinde güncelleştirmeniz gerekir. Bu güncelleştirme aracılığıyla yapabilirsiniz:

* REST API: 

  `PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` 
  
  Daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate).

  Alternatif olarak, genel ölçek kümesi özelliklerini güncelleştirmek için REST API özelliklerinden kullanarak bir Azure Resource Manager şablonu dağıtabilirsiniz.

* PowerShell: 

  `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -VirtualMachineScaleSet {scaleSetConfigPowershellObject}` 
  
  Daha fazla bilgi için bkz: [PowerShell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss).

* Azure CLI:

  * Bir özelliği değiştirmek için: `az vmss update --set {propertyPath}={value}` 
  
  * Bir liste özelliği ölçek kümesindeki bir nesne eklemek için: `az vmss update --add {propertyPath} {JSONObjectToAdd}` 
  
  * Bir nesne ölçek kümesindeki bir liste özelliği kaldırmak için: `az vmss update --remove {propertyPath} {indexToRemove}` 
  
  Daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update). 
  
  Alternatif olarak, ölçeği kullanarak Ayarla daha önce dağıttıysanız `az vmss create` komutunu çalıştırabilirsiniz `az vmss create` yeniden ölçek kümesini Güncelleştir komutu. Bunu yapmak için emin olun tüm özellikleri `az vmss create` komutu aynıdır önceki gibi değiştirmek istediğiniz özellikleri dışında.



Aynı zamanda [Azure kaynak Gezgini (Önizleme)](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/) ölçek güncellemek için model ayarlayın.

Ölçek kümesi modeli güncelleştirildikten sonra yeni yapılandırmayı ölçek kümesinde oluşturulan yeni Vm'leri uygular. Ancak, modelleri ölçek kümesindeki var olan VM'ler için hala son genel ölçek kümesi modeli ile güncel duruma getirilmesi gerekir. Her VM için modelde adlı bir Boolean özelliği `latestModelApplied` VM son genel ölçek kümesi modelle güncel olup olmadığını gösterir. (Değerini `true` VM son modeliyle güncel olduğu anlamına gelir.)




## <a name="techniques-for-bringing-vms-up-to-date-with-the-latest-scale-set-model"></a>VM'ler son ölçek kümesi modeliyle güncel hale getirme yöntemleri

Ölçek kümesine sahip bir *yükseltme İlkesi* nasıl VM'ler son ölçek kümesi modeli ile güncel duruma getirilene belirler. Yükseltme ilkesi için üç mod şunlardır:

- **Otomatik**: Bu modda, Ölçek kümesini garanti yapılmadan aşağı VM'ler sırası hakkında yapar. Ölçek kümesi aynı anda tüm sanal makineleri sürebilir. 
- **Çalışırken**: Bu modda, Ölçek kümesini toplu işlemleri arasında isteğe bağlı duraklatma süresi ile bir toplu güncelleştirmeyi ortalarında dağıtır.
- **El ile**: ölçek kümesi modeli güncelleştirmek bu modda, hiçbir şey mevcut Vm'lere olur. Var olan sanal makineleri güncelleştirmek için her biri el ile yükseltmeniz gerekir. Aracılığıyla el ile bu yükseltme yapabilirsiniz:

  - REST API: 
  
    `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/manualupgrade?api-version={apiVersion}` 
    
    Daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/updateinstances).

  - PowerShell: 
  
    `Update-AzureRmVmssInstance -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` 
    
    Daha fazla bilgi için bkz: [PowerShell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance).

  - Azure CLI: 
  
    `az vmss update-instances -g {resourceGroupName} -n {vmScaleSetName} --instance-ids {instanceIds}` 
    
    Daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update_instances).

  Aynı zamanda [Azure SDK'ları](https://azure.microsoft.com/downloads/) el ile bir VM ölçek kümesindeki yükseltmek için.

>[!NOTE]
> Azure Service Fabric kümeleri yalnızca otomatik modunu kullanabilirsiniz, ancak güncelleştirme farklı şekilde ele alınır. Service Fabric güncelleştirmeleri hakkında daha fazla bilgi için bkz: [Service Fabric belgelerine](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade).

Değişiklik genel ölçek özelliklerini ayarlamak için bir tür yükseltme İlkesi izleyin değil: değişiklikler ölçeğe ayarlar işletim sistemi profili. (Yönetici kullanıcı adı ve parola örnektir.) Bu özellikler yalnızca'API sürümünde 2017-12-01 veya daha sonra değiştirilebilir. Bu değişiklikler yalnızca model ölçek değişikliği ayarladıktan sonra oluşturulan VM'ler için geçerlidir. Var olan VM'ler güncel duruma getirmek için her mevcut VM'yi yeniden görüntü oluşturma gerekir. Bir VM aracılığıyla yeniden görüntü oluşturma:

* REST API: 

  `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` 
  
  Daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage).

* PowerShell: 

  `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` 
  
  Daha fazla bilgi için bkz: [PowerShell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm).

* Azure CLI: 

  `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` 
  
  Daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage).

Aynı zamanda [Azure SDK'ları](https://azure.microsoft.com/downloads/) ölçek kümesindeki VM görüntüsünü yeniden oluşturmak için.




## <a name="properties-with-restrictions-on-modification"></a>Özellikleri değiştirme kısıtlamalar ile

### <a name="create-time-properties"></a>Oluşturma zamanı özellikleri

Yalnızca ölçek kümesini başlangıçta oluştururken bazı özellikleri ayarlayabilirsiniz. Bu özellikler şunları içerir:

- Bölgeler
- Yansıma başvurusu yayımcı
- Yansıma başvurusu teklif

### <a name="properties-that-can-be-changed-based-on-the-current-value-only"></a>Yalnızca geçerli değere göre değiştirilebilir özellikleri

Özel durum dışında geçerli değerine bağlı olarak bazı özellikleri değiştirilebilir. Bu özellikler şunları içerir:

- `singlePlacementGroup`: İf `singlePlacementGroup` onu değiştirilebilir false olarak doğrudur. Ancak, varsa `singlePlacementGroup` false, onu *olamaz* değiştirilmesi true.
- `subnet`: Özgün alt ağ ve yeni alt ağı aynı sanal ağ içinde olduğu sürece ölçek kümesini alt değiştirilebilir.

### <a name="properties-that-require-deallocation-to-change"></a>Ayırmayı kaldırma değiştirmek için gereken özellikleri

Ölçek kümesindeki sanal makineleri yalnızca serbest varsa bazı özellikler için bazı değerler değiştirilebilir. Bu özellikler şunları içerir:

- `sku name`: Yeni VM SKU ölçek kümesi şu anda açık olan bir donanımda desteklenmiyorsa, sanal makineleri değiştirmeden önce ayarlamak ölçeğinde ayırması gerekir `sku name`. Sanal makineleri yeniden boyutlandırma ile ilgili daha fazla bilgi için bkz: [bu Azure blog gönderisine](https://azure.microsoft.com/blog/resize-virtual-machines/).


## <a name="vm-specific-updates"></a>VM özgü güncelleştirmeleri

Bazı değişiklikleri Özellikleri Genel ölçeği ayarlamak yerine belirli VM'ler uygulanabilir. Şu anda, yalnızca desteklenen VM yönelik bir güncelleştirme ekleme/ayırma ölçek kümesi'ndeki veri disklerinin/VMs. Bu özelliğin önizlemede değil. Daha fazla bilgi için bkz: [Önizleme belgelerine](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).

## <a name="scenarios"></a>Senaryolar

### <a name="application-updates"></a>Uygulama güncelleştirmeleri

Bir uygulama ölçeği uzantıları Ayarla dağıtılırsa, uzantı yapılandırmasını güncelleştirme yükseltme ilkesiyle uygun şekilde güncelleştirilmesi uygulamanın neden olur. Örneğin, bir özel betik uzantısı'nda çalıştırılacak bir komut dosyası yeni bir sürümü varsa, güncelleştirme `fileUris` yeni komut dosyasına işaret edecek şekilde özelliği. 

Bazı durumlarda, uzantı yapılandırması değişmeden olsa bile bir güncelleştirmeyi uygulamak isteyebilirsiniz. (Örneğin, komut dosyası komut dosyası URI'sini değiştirmeden güncelleştirildi.) Bu durumlarda, değiştirebileceğiniz `forceUpdateTag` bir güncelleştirmeyi uygulamak için. Değeri değiştirilerek uzantısı nasıl çalışacağını üzerinde hiçbir etkisi olmaz şekilde Azure platformu bu özellik, Yorumlar değil. Değiştirmeye yalnızca yeniden çalıştırmak için uzantı zorlar. 

Daha fazla bilgi için `forceUpdateTag`, bkz: [uzantıları için REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachineextensions/createorupdate).

Ayrıca özel bir görüntü dağıtılacak uygulamalar yaygındır. Bu senaryo aşağıdaki bölümde ele alınmıştır.

### <a name="os-updates"></a>İşletim sistemi güncelleştirmeleri

Platform görüntüleri kullanıyorsanız değiştirerek görüntüleri güncelleştirebilirsiniz `imageReference`. Daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate).

>[!NOTE]
> Platform görüntülerle "son" için resim başvurusu sürümünü belirtmek için yaygın bir sorundur. Bu, Ölçek kümeleri oluşturduğunuzda, genişletilmiş ve görüntülenen, VM'ler ile en son sürüm oluşturulduğunu anlamına gelir. Ancak, bu *yok* yeni görüntü sürümleri yayımlanan gibi işletim sistemi görüntüsü otomatik olarak zaman içinde güncelleştirileceği olduğunu anlamına gelir. Şu anda önizlemede ayrı bir özellik budur. Daha fazla bilgi için bkz: [otomatik işletim sistemi yükseltmeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade).

Özel resimler kullanıyorsanız, görüntüleri güncelleştirerek güncelleştirebilirsiniz `imageReference` kimliği Daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate).

## <a name="examples"></a>Örnekler

### <a name="update-the-os-image-for-your-scale-set"></a>İşletim sistemi görüntüsü, Ölçek kümesi için güncelleştirme

Bir ölçeği Ubuntu LTS 16.04 eski bir sürümünü çalıştıran Ayarla sahip varsayalım. Ubuntu LTS 16.04 (örneğin, 16.04.201801090 sürüm) daha yeni bir sürüme güncelleştirmek istediğiniz. Böylece bu komutları kullanarak doğrudan bu özellikleri değiştirebilirsiniz görüntü başvurusu version özelliği listesinin bir parçası değil:

* PowerShell: 

  `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -ImageReferenceVersion 16.04.201801090`

* Azure CLI: 

  `az vmss update -g {resourceGroupName} -n {vmScaleSetName} --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090`


### <a name="update-the-load-balancer-for-your-scale-set"></a>Yük Dengeleyici, Ölçek kümesi için güncelleştirme

Azure yük dengeleyicisi ile ölçek varsa ve yük dengeleyici bir Azure uygulama ağ geçidi ile değiştirmek istediğiniz varsayalım. Yük Dengeleyici ve uygulama ağ geçidi özelliklerini ölçek kümesi için bir liste bir parçasıdır. Bu nedenle, liste öğelerini özellikleri doğrudan değiştirme yerine ekleme ve kaldırma için komutlarını kullanabilirsiniz.

PowerShell:
```
# Get the current model of the scale set and store it in a local PowerShell object named $vmss
> $vmss=Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName}

# Create a local PowerShell object for the new desired IP configuration, which includes the reference to the application gateway
> $ipconf = New-AzureRmVmssIPConfig myNic -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name

# Replace the existing IP configuration in the local PowerShell object (which contains the references to the current Azure load balancer) with the new IP configuration
> $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf

# Update the model of the scale set with the new configuration in the local PowerShell object
> Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName} -virtualMachineScaleSet $vmss

```

Azure CLI:
```
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0 # Remove the load balancer back-end pool from the scale set model
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0 # Remove the load balancer back-end pool from the scale set model; only necessary if you have NAT pools configured on the scale set
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}' # Add the application gateway back-end pool to the scale set model
```

>[!NOTE]
> Bu komutlar, Ölçek kümesinde yalnızca bir IP yapılandırması ve yük dengeleyici olduğunu varsayalım. Varsa birden çok, bir listeyi dizin 0 dışında kullanmanız gerekebilir.