---
title: "Sanal makineleri Azure VM ölçek kümesi için örnek kimlikleri anlama | Microsoft Docs"
description: "VM'ler örneği kimlikleri için Azure VM ölçek kümesi anlama"
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
ms.date: 02/22/2018
ms.author: negat
ms.openlocfilehash: 3a43dc86f1fb53dfde4bce3938faaa30e18f5a6d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="understand-instance-ids-for-azure-vm-scale-set-vms"></a>VM'ler örneği kimlikleri için Azure VM ölçek kümesi anlama
Bu makalede örnek kimlikleri için ölçek kümeleri ve bunlar ortaya çeşitli yolları açıklanmaktadır.

## <a name="scale-set-instance-ids"></a>Ölçek kümesi örnek kimlikleri

Her VM ölçek kümesindeki benzersiz olarak tanımlayan bir örnek kimliği alır. Bu örnek kimliği API'leri ölçek kümesindeki belirli bir üzerinde işlem yapmak için kullanılan VM ölçek ayarlayın. Örneğin, yeniden görüntü oluşturma API kullanırken, yeniden görüntü oluşturma için belirli örnek kimliği belirtebilirsiniz:

REST API: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` (daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage))

PowerShell: `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` (daha fazla bilgi için bkz: [Powershell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm))

CLI: `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` (daha fazla bilgi için bkz: [CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage)).

Ölçek kümesindeki tüm örneklerini listeleyerek örneği kimlikleri listesini elde edebilirsiniz:

REST API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualMachines?api-version={apiVersion}` (daha fazla bilgi için bkz: [REST API belgeleri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/list))

PowerShell: `Get-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}` (daha fazla bilgi için bkz: [Powershell belgelerine](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

CLI: `az vmss list-instances -g {resourceGroupName} -n {vmScaleSetName}` (daha fazla bilgi için bkz: [CLI belgelerine](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_list_instances)).

Aynı zamanda [resources.azure.com](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/) ölçek kümesindeki Vm'leri listelemek için.

Komutu sağladığınız seçenekleri çıkış tam sunu bağlıdır, ancak CLI bazı örnek çıktısı şöyledir:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
[
  {
    "instanceId": "85",
    "latestModelApplied": true,
    "location": "westus",
    "name": "nsgvmss_85",
    .
    .
    .
```

Gördüğünüz gibi "InstanceId" yalnızca ondalık bir sayı özelliğidir. Eski örnekleri silindikten sonra kimlikleri örneği için yeni örnekleri yeniden kullanılabilir.

>[!NOTE]
> Yoktur **garanti** şekilde örneğinde kimlikleri ölçek kümesindeki sanal makineleri atanır. Bunlar zaman zaman sıralı olarak artan görünebilir, ancak bu her zaman geçerli değildir. Bir bağımlılık örneği kimlikleri Vm'lere atanan belirli şekilde kazanmaz.

## <a name="scale-set-vm-names"></a>Ölçek kümesi VM adları

Yukarıdaki örnek çıktıda ayrıca VM için "name" vardır. Bu ad "{ölçek kümesi adı} _ {kimliği örneği}" biçimini alır. Ölçek kümesindeki örnekleri listelediğinizde Azure portalında gördüğünüz bir addır:

![](./media/virtual-machine-scale-sets-instance-ids/vmssInstances.png)

{Kimliği örneği} adı daha önce ele alınan "InstanceId" özelliği olarak aynı ondalık sayı parçasıdır.

## <a name="instance-metadata-vm-name"></a>Örnek meta verileri VM adı

Query [örneği meta veri](../virtual-machines/windows/instance-metadata-service.md) gelen bir ölçek kümesindeki VM çıktıda bir "name" görürsünüz:

```
{
  "compute": {
    "location": "westus",
    "name": "nsgvmss_85",
    .
    .
    .
```

Bu adı daha önce ele alınan adıyla aynıdır.

## <a name="scale-set-vm-computer-name"></a>Ölçek kümesi VM bilgisayar adı

Ayrıca bir ölçek her VM kendisine atanmış bir bilgisayar adı alır. VM ana bilgisayar adı adıdır [Azure tarafından sağlanan DNS ad çözümlemesi sanal ağ içinde](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Bu bilgisayar adı "{computer-name-prefix}{base-36-instance-id}" biçimidir.

{Base-36-örnek-ID} yer [36 temel](https://en.wikipedia.org/wiki/Base36) ve her zaman uzunluğu altı basamaktan oluşur. Altıdan basamak sayısı temel 36 gösterimini aldığı durumlarda {base-36-örnek-ID} uzunluğu altı basamak olun sıfırlarla sıfır eklenir. Örneğin, {bilgisayar adı öneki} "nsgvmss" örneğiyle ve örnek kimliği 85 bilgisayar adı "nsgvmss00002D" sahip olur.

>[!NOTE]
> Ölçek kümesi adından kendisini farklı olabilir bilgisayar adı öneki ayarlayabileceğiniz, Ölçek kümesi modelinin özelliğidir.