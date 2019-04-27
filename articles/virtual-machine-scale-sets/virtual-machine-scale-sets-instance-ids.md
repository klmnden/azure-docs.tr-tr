---
title: Azure VM ölçek kümesi Vm'lerine örnek kimlikleri anlama | Microsoft Docs
description: Örnek kimlikleri için Azure VM ölçek kümesi Vm'leri anlama
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
ms.date: 02/22/2018
ms.author: manayar
ms.openlocfilehash: 6aeba722a0661979664f8d61efdb9b2bf47ad801
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60618515"
---
# <a name="understand-instance-ids-for-azure-vm-scale-set-vms"></a>Örnek kimlikleri için Azure VM ölçek kümesi Vm'leri anlama
Bu makalede örnek kimlikleri ölçek kümeleri ve bunlar yüzey çeşitli yolları açıklar.

## <a name="scale-set-instance-ids"></a>Ölçek kümesi örnek kimlikleri

Her bir VM ölçek kümesindeki benzersiz olarak tanımlayan bir örnek kimliği alır. VM ölçek kümesi kimliği API'leri ölçek kümesindeki belirli bir üzerinde işlem yapmak için kullanılan bu örneği. Örneğin, yeniden görüntü oluşturma API'si kullanılırken bir belirli bir örnek kimliği için yeniden görüntü oluşturma belirtebilirsiniz:

REST API: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` (daha fazla bilgi için [REST API belgelerini](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage))

PowerShell: `Set-AzVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` (daha fazla bilgi için [Powershell belgeleri](https://docs.microsoft.com/powershell/module/az.compute/set-azvmssvm))

CLI: `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` (daha fazla bilgi için [CLI belgeleri](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest)).

Bir ölçek kümesindeki tüm örnekleri listeleyerek örnek kimlikleri listesini alabilirsiniz:

REST API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualMachines?api-version={apiVersion}` (daha fazla bilgi için [REST API belgelerini](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/list))

PowerShell: `Get-AzVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}` (daha fazla bilgi için [Powershell belgeleri](https://docs.microsoft.com/powershell/module/az.compute/get-azvmssvm))

CLI: `az vmss list-instances -g {resourceGroupName} -n {vmScaleSetName}` (daha fazla bilgi için [CLI belgeleri](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest)).

Ayrıca [resources.azure.com](https://resources.azure.com) veya [Azure SDK'ları](https://azure.microsoft.com/downloads/) bir ölçek kümesindeki Vm'leri listelemek için.

Çıkış tam sunumu için sağladığınız seçenekleri bağlıdır, ancak clı'dan bazı örnek çıktı aşağıdaki gibidir:

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

Gördüğünüz gibi "InstanceId" özelliği yalnızca ondalık bir sayı olabilir. Eski örnekleri silindikten sonra örnek kimlikleri yeni örnekleri için yeniden kullanılabilir.

>[!NOTE]
> Var. **garanti** şekilde örneğinde ölçek kümesindeki vm'lere kimlikleri atanır. Bunlar bazen sıralı olarak artan görünebilir, ancak bu her zaman böyle değildir. Bir bağımlılık örnek kimlikleri Vm'lere atanmış olan belirli şekilde almaz.

## <a name="scale-set-vm-names"></a>Ölçek kümesi sanal makine adı

Yukarıdaki örnek çıktıda da VM için bir "name" yoktur. Bu adı "{ölçek-kümesi-adı} _ {örnek-kimliği}" biçimini alır. Bir ölçek kümesindeki örneklerin listelediğinizde Azure portalında gördüğünüz bir addır:

![](./media/virtual-machine-scale-sets-instance-ids/vmssInstances.png)

{Örnek-kimliği} adının bir parçası olarak daha önce açıklanan "InstanceId" özelliği aynı ondalık sayıdır.

## <a name="instance-metadata-vm-name"></a>Örnek meta veri VM adı

Sorgu, [örnek meta veri](../virtual-machines/windows/instance-metadata-service.md) gelen bir VM ölçek kümesi içinde çıktıda bir "name" görürsünüz:

```
{
  "compute": {
    "location": "westus",
    "name": "nsgvmss_85",
    .
    .
    .
```

Bu adı daha önce açıklanan adı ile aynıdır.

## <a name="scale-set-vm-computer-name"></a>Ölçek kümesi VM bilgisayar adı

Bir ölçek kümesi aynı zamanda her bir sanal Makineye atanmış bir bilgisayar adı alır. Bu bilgisayar adı VM'nin adıdır [Azure tarafından sağlanan DNS ad çözümlemesi sanal ağ içindeki](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Bu bilgisayar adı "{computer-name-prefix}{base-36-instance-id}" biçimidir.

{Base-36-örnek-kimliği} yer [36 temel](https://en.wikipedia.org/wiki/Base36) ve her zaman altı basamak uzunluğunda. Altıdan az basamak sayısı 36 gösterimini temel aldığı durumlarda {base-36-örnek-kimliği} uzunluğu altı basamaklı yapmak için önüne sıfır eklenir. Örneğin, bilgisayar adı "nsgvmss00002D" {bilgisayar adı öneki} "nsgvmss" ile bir örnek ve örnek kimliği 85 olacaktır.

>[!NOTE]
> Bilgisayar adı ön eki ayarlayabilirsiniz, Ölçek kümesi modelinde bir özelliği olduğundan ölçek kümesi adı kendisini farklı olabilir.
