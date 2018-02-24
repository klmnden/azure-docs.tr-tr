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
ms.openlocfilehash: 836d56012afa9e5d5bdec35d85c37dd4b0b788ce
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="modify-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesini değiştirme
Bu makalede, varolan bir ölçek kümesini değiştirmek açıklar. Bu ölçeğin yapılandırmasını değiştirmek nasıl içerir, ölçeğini çalışan uygulamalara yapılandırmasını değiştirmek nasıl set, kullanılabilirlik ve daha fazlasını nasıl yönetileceği.

## <a name="fundamental-concepts"></a>temel kavramlar

### <a name="the-scale-set-model"></a>Ölçek kümesi modeli

Bir ölçek "yakalayan bir ölçek kümesi modeli" olarak ayarlanmış *istenen* ölçek durumunu bir bütün olarak ayarlayın. Modelin bir ölçek kümesi için sorgu için kullanabilirsiniz:

REST API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` (daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/get))

PowerShell: `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}` (daha fazla bilgi için bkz: [Powershell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss))

CLI: `az vmss show -g {resourceGroupName} -n {vmSaleSetName}` (daha fazla bilgi için bkz: [CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show))

De kullanabilirsiniz [resources.azure.com](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/) modeli ölçek kümesi için sorgulanamıyor.

Komutu sağladığınız seçenekleri çıkış tam sunu bağlıdır, ancak CLI bazı örnek çıktısı şöyledir:

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



### <a name="the-scale-set-instance-view"></a>Ölçek kümesi örnek görünümü

Ayrıca bir ölçek "görüntülemek bir ölçek kümesi örnek" geçerli yakalayan sahip *çalışma zamanı* ölçek durumunu bir bütün olarak ayarlayın. Ölçek kümesi örnek görünümünü sorgulamak için aşağıdaki komutu kullanabilirsiniz:

REST API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/instanceView?api-version={apiVersion}` (daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getinstanceview))

PowerShell: `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceView` (daha fazla bilgi için bkz: [Powershell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss))

CLI: `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName}` (daha fazla bilgi için bkz: [CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view))

Aynı zamanda [resources.azure.com](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/) ölçek kümesi örnek görünümünü sorgulanamıyor.

Komutu sağladığınız seçenekleri çıkış tam sunu bağlıdır, ancak CLI örnek çıktısı şöyledir:

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

Gördüğünüz gibi bu özellikleri ölçeğinde VM'ler geçerli çalışma zamanı durumu özetini ayarla, içerir (verilmemiştir) ölçek kümesine uygulanan uzantıları durumu da dahil olmak üzere sağlar.



### <a name="the-scale-set-vm-model-view"></a>Ölçek kümesi VM model görünümü

Benzer şekilde nasıl bir model görünümü ölçek kümesi vardır, her bir VM ölçek kümesindeki kendi modeli görünüme sahiptir. Ölçek kümesi için model view sorgulamak için aşağıdaki komutu kullanabilirsiniz:

REST API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}?api-version={apiVersion}` (daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/get))

PowerShell: `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` (daha fazla bilgi için bkz: [Powershell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

CLI: `az vmss show -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` (daha fazla bilgi için bkz: [CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show))

De kullanabilirsiniz [resources.azure.com](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/) bir VM ölçek kümesindeki için model sorgulanamıyor.

Komutu sağladığınız seçenekleri çıkış tam sunu bağlıdır, ancak CLI bazı örnek çıktısı şöyledir:

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

Gördüğünüz gibi bu özellikleri VM kendisi yapılandırma bir bütün olarak ayarlayın ve ölçeğin yapılandırmasını açıklar. Örneğin, Ölçek kümesi modeli vardır `overprovision` model için bir VM ölçek kümesindeki çalışmazken bir özellik olarak. İşleminin tamamı, bireysel VM ölçek kümesindeki yap ölçek için bir özellik olduğundan bu farktır (işleminin hakkında daha fazla bilgi için bkz: [bu belgeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning)).



### <a name="the-scale-set-vm-instance-view"></a>Ölçek kümesi VM örnek görünümü

Ölçek kümesindeki her bir VM ölçek kümesi örnek görünümünü nasıl sahip benzer, kendi örneği görünüme sahiptir. Ölçek kümesi örnek görünümünü sorgulamak için aşağıdaki komutu kullanabilirsiniz:

REST API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}/instanceView?api-version={apiVersion}` (daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/getinstanceview))

PowerShell: `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -InstanceView` (daha fazla bilgi için bkz: [Powershell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

CLI: `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` (daha fazla bilgi için bkz: [CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view))

Aynı zamanda [resources.azure.com](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/) sorgu ölçek kümesindeki VM örneği görünümüne.

Komutu sağladığınız seçenekleri çıkış tam sunu bağlıdır, ancak CLI bazı örnek çıktısı şöyledir:

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

Gördüğünüz gibi bu özellikleri (verilmemiştir) ölçek kümesine uygulanan uzantıları dahil olmak üzere VM kendisi, geçerli çalışma zamanı durumunu açıklar.




## <a name="how-to-update-global-scale-set-properties"></a>Genel ölçeği güncelleştirme özelliklerini ayarlama

Bir genel ölçek kümesi özelliği güncelleştirmek için ölçek kümesi modelinde özelliğinde güncelleştirmeniz gerekir. Bu güncelleştirme aracılığıyla yapabilirsiniz:

REST API: `PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` (daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate))

Resource Manager şablonları: Genel ölçek kümesi özelliklerini güncelleştirmek için REST API özelliklerinden kullanarak bir Resource Manager şablonu alternatif olarak dağıtabilirsiniz.

PowerShell: `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -VirtualMachineScaleSet {scaleSetConfigPowershellObject}` (daha fazla bilgi için bkz: [Powershell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss))

CLI. Bir özelliği değiştirmek için: `az vmss update --set {propertyPath}={value}`. Bir liste özelliği ölçek kümesindeki bir nesne eklemek için: `az vmss update --add {propertyPath} {JSONObjectToAdd}`. Bir nesne ölçek kümesindeki bir liste özelliği kaldırmak için: `az vmss update --remove {propertyPath} {indexToRemove}`. (daha fazla bilgi için bkz: [CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update)). Alternatif olarak, ölçeği kullanarak Ayarla daha önce dağıttıysanız `az vmss create` komutunu çalıştırabilirsiniz `az vmss create` yeniden ölçek kümesini Güncelleştir komutu. Bunu yapmak için emin olmak gereken tüm özellikleri `az vmss create` komutu aynıdır önceki gibi değiştirmek istediğiniz özellikleri dışında.



Aynı zamanda [resources.azure.com](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/) ölçek güncellemek için model ayarlayın.

Ölçek kümesi modeli güncelleştirildikten sonra yeni yapılandırmayı ölçek kümesinde oluşturulan yeni Vm'leri uygular. Ancak, modelleri ölçek kümesindeki var olan VM'ler için hala son genel ölçek kümesi modeli ile güncel duruma getirilmesi gerekir. Modeldeki her bir VM için bir boolean özelliği adlı `latestModelApplied` VM son genel ölçek kümesi modelle güncel olup olmadığını gösterir (`true` VM son modeliyle güncel olduğu anlamına gelir).




## <a name="how-to-bring-vms-up-to-date-with-the-latest-scale-set-model"></a>Sanal makineleri son ölçek kümesi modeliyle güncel duruma getirmek nasıl

Ölçek kümesine sahip bir "Yükseltme İlkesi" belirleyen nasıl VM'ler son ölçek kümesi modeli ile güncel duruma getirilir. Yükseltme ilkesi için üç mod şunlardır:

- Otomatik: Bu modda, garanti duruma VM'ler sırası hakkında ölçek kümesi sağlar. Ölçek kümesi aynı anda tüm sanal makineleri sürebilir. 
- Alınıyor: Bu modda, Ölçek kümesini bir isteğe bağlı duraklatma süresiyle toplu güncelleştirme arasında toplu ortalarında dağıtır.
- El ile: ölçek kümesi modeli güncelleştirmek bu modda, hiçbir şey mevcut Vm'lere olur. Var olan sanal makineleri güncelleştirmek için varolan her VM "el ile yükseltme" yapmanız gerekir. Aracılığıyla el ile bu yükseltme yapabilirsiniz:

REST API: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/manualupgrade?api-version={apiVersion}` (daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/updateinstances))

PowerShell: `Update-AzureRmVmssInstance -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` (daha fazla bilgi için bkz: [Powershell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance))

CLI: `az vmss update-instances -g {resourceGroupName} -n {vmScaleSetName} --instance-ids {instanceIds}` (daha fazla bilgi için bkz: [CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update_instances)).

Aynı zamanda [Azure SDK'ları](https://azure.microsoft.com/downloads/) ölçek kümesindeki bir VM'de el ile yükseltme yapmak için.

>[!NOTE]
> Service Fabric kümeleri yalnızca otomatik modunu kullanabilirsiniz, ancak güncelleştirme farklı şekilde ele alınır. Service fabric güncelleştirmeleri hakkında daha fazla bilgi için bkz: [Service Fabric belgelerine](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade).

>[!NOTE]
> Bir tür Özellikleri Genel ölçeği ayarlamak için yükseltme İlkesi izlemez değişiklik yoktur. Değişiklikleri bunlar işletim sistemi profili (örneğin, yönetici kullanıcı adı ve parola) için ölçek kümesi. Bu değişiklikler yalnızca model ölçek değişikliği ayarladıktan sonra oluşturulan sanal makineleri için geçerlidir. Var olan VM'ler güncel duruma getirmek için bir "yeniden görüntü oluşturma" var olan her VM yapmanız gerekir. Bu yeniden görüntü oluşturma aracılığıyla yapabilirsiniz:

REST API: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` (daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage))

PowerShell: `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` (daha fazla bilgi için bkz: [Powershell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm))

CLI: `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` (daha fazla bilgi için bkz: [CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage)).

Aynı zamanda [Azure SDK'ları](https://azure.microsoft.com/downloads/) ölçek kümesindeki VM görüntüsünü yeniden oluşturmak için.




## <a name="properties-with-restrictions-on-modification"></a>Özellikleri değiştirme kısıtlamalar ile

### <a name="create-time-properties"></a>Oluşturma zamanı özellikleri

Bazı özellikler yalnızca başlangıçta ölçek oluşturma kümesi ayarlayabilirsiniz. Bu özellikler şunları içerir:

- Bölgeleri
- image reference publisher
- Yansıma başvurusu teklif

### <a name="properties-that-can-only-be-changed-based-on-the-current-value"></a>Geçerli değere göre yalnızca değiştirilebilir özellikleri

Özel durumlar dışında geçerli değerine bağlı olarak bazı özellikleri değiştirilebilir. Bu özellikler şunları içerir:

- singlePlacementGroup: singlePlacementGroup true ise, onu false olarak değiştirilebilir. Ancak, singlePlacementGroup yanlışsa, onu **olmayabilir** değiştirilmesi true.
- alt ağ: özgün alt ağ ve yeni alt ağı aynı sanal ağ içinde olduğu sürece bir ölçek kümesinin alt değiştirilebilir.

### <a name="properties-that-require-deallocation-to-change"></a>Ayırmayı kaldırma değiştirmek için gereken özellikleri

Ölçek kümesindeki sanal makineleri serbest varsa bazı özellikler yalnızca belirli değerler değiştirilebilir. Bu özellikler şunları içerir:

- SKU adı: yeni VM SKU desteklenmiyorsa donanımda ölçek kümesi şu anda etkin, ölçeği sku adı değiştirmeden önce Ayarla içindeki VM'ler ayırması gerekir. Sanal makineleri yeniden boyutlandırma ile ilgili daha fazla bilgi için bkz: [bu Azure blog gönderisine](https://azure.microsoft.com/blog/resize-virtual-machines/).


## <a name="vm-specific-updates"></a>VM özgü güncelleştirmeleri

Bazı değişiklikleri Özellikleri Genel ölçeği ayarlamak yerine belirli VM'ler için uygulanabilir. Şu anda, yalnızca desteklenen VM yönelik bir güncelleştirme ekleme/ayırma ölçek kümesi'ndeki veri disklerinin/VMs. Bu özelliğin önizlemede değil. Daha fazla bilgi için bkz: [Önizleme belgelerine](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).

## <a name="scenarios-application-updates-os-updates-etc"></a>Senaryolar: Uygulama güncelleştirmeleri, işletim sistemi güncelleştirmeleri, vs.

### <a name="application-updates"></a>Uygulama güncelleştirmeleri

Bir uygulama ölçeği uzantıları Ayarla dağıtılırsa, uzantı yapılandırmasını güncelleştirme yükseltme ilkesiyle uygun şekilde güncelleştirmek uygulamanın neden olur. Örneğin, bir özel betik uzantısı'nda çalıştırılacak bir komut dosyası yeni bir sürümü varsa, yeni komut dosyasına işaret edecek şekilde fileUris özelliği güncelleştirebilir. Bazı durumlarda, ancak uzantı yapılandırması değişmeden olsa bile bir güncelleştirmeyi uygulamak istediğiniz (örneğin, komut dosyası komut dosyası URI'sini değiştirmeden güncelleştirilmiş size). Bu durumlarda, bir güncelleştirmeyi uygulamak için forceUpdateTag değiştirebilirsiniz. Değeri değiştirilerek uzantısı nasıl çalışacağını üzerinde hiçbir etkisi olmaz şekilde Azure platformu bu özellik, Yorumlar değil. Değiştirmeye yalnızca yeniden çalıştırmak için uzantı zorlar. ForceUpdateTag hakkında daha fazla bilgi için bkz: [uzantıları için REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachineextensions/createorupdate).

Ayrıca özel bir görüntü dağıtılacak uygulamalar yaygındır. Bu senaryo aşağıdaki "İşletim sistemi güncelleştirmeleri" bölümünde ele alınmıştır

### <a name="os-updates"></a>İşletim sistemi güncelleştirmeleri

Platform görüntüleri kullanıyorsanız, Imagereference değiştirerek görüntü güncelleştirebilir (daha fazla bilgi için [REST API belgeleri](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

>[!NOTE]
> Platform görüntülerle "son" için resim başvurusu sürümünü belirtmek için yaygın bir sorundur. Sırasında ölçek kümesi oluşturma anlamına gelir, ölçeklendirme ve yeniden görüntü oluşturma, VM'ler ile en son sürüm oluşturulur. Ancak, bu **yok** yeni görüntü sürümleri yayımlanan gibi işletim sistemi görüntüsü otomatik olarak zaman içinde güncelleştirileceği olduğunu anlamına gelir. Şu anda önizlemede ayrı bir özellik budur. Daha fazla bilgi için bkz: [otomatik işletim sistemi yükseltme belgelerine](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade).

Özel resimler kullanıyorsanız, Imagereference kimliği güncelleştirerek görüntü güncelleştirebilirsiniz (daha fazla bilgi için [REST API belgeleri](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

## <a name="examples"></a>Örnekler

### <a name="updating-the-os-image-for-your-scale-set"></a>İşletim sistemi görüntüsünü ölçek kümesi için güncelleştirme

Bir ölçeği Ubuntu LTS 16.04 eski bir sürümünü çalıştıran Ayarla varsa ve Ubuntu LTS 16.04 (örneğin, 16.04.201801090 sürüm) daha yeni bir sürüme güncelleştirmek istediğiniz varsayalım. Böylece bu komutları ile bu özellikleri doğrudan değiştirebilirsiniz görüntü başvurusu version özelliği listesinin bir parçası değil:

PowerShell: `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -ImageReferenceVersion 16.04.201801090`

CLI: `az vmss update -g {resourceGroupName} -n {vmScaleSetName} --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090`


### <a name="updating-the-load-balancer-for-your-scale-set"></a>Yük Dengeleyici, Ölçek kümesi için güncelleştirme

Bir Azure yük dengeleyicisi ile ölçek varsa ve Azure yük dengeleyici Azure uygulama ağ geçidi ile değiştirmek istediğiniz varsayalım. Özellikleri doğrudan değiştirme yerine liste öğeleri ekleme ve kaldırma için komutları kullanabilmek için bir ölçek kümesi için yük dengeleyici ve uygulama ağ geçidi özelliklerini listesinin bir parçası şunlardır:

PowerShell: 
```
# get the current model of the scale set and store it in a local powershell object named $vmss
> $vmss=Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName}

# create a local powershell object for the new desired IP configuration, which includes the referencerence to the application gateway
> $ipconf = New-AzureRmVmssIPConfig myNic -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name

# replace the existing IP configuration in the local powershell object (which contains the references to the current Azure Load Balancer) with the new IP configuration
> $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf

# Update the model of the scale set with the new configuration in the local powershell object
> Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName} -virtualMachineScaleSet $vmss

```

CLI:
```
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0 # remove the load balancer backend pool from the scale set model
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0 # remove the load balancer backend pool from the scale set model; only necessary if you have NAT pools configured on the scale set
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}' # add the application gateway backend pool to the scale set model
```

>[!NOTE]
> Bu komutlar, Ölçek kümesinde yalnızca bir IP yapılandırması ve yük dengeleyici olduğunu varsayalım. Varsa birden çok, bir listeyi dizin 0 dışında kullanmanız gerekebilir.