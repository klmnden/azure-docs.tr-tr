---
title: Örneği koruması için Azure sanal makine ölçek kümesi örneklerine | Microsoft Docs
description: Azure sanal makine ölçek kümesi örneklerine ölçeklendirme ve ölçek kümesi işlemlerinden korumayı öğrenin.
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2019
ms.author: manayar
ms.openlocfilehash: 6c271c2c9feb1520951b2a8e301da4878970d60a
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66259430"
---
# <a name="instance-protection-for-azure-virtual-machine-scale-set-instances-preview"></a>Örneği koruması için Azure sanal makine ölçek kümesi örnekleri (Önizleme)
Azure sanal makine ölçek kümeleri iş yükleriniz için daha iyi esneklik etkinleştirme [otomatik ölçeklendirme](virtual-machine-scale-sets-autoscale-overview.md)altyapınızı ölçeği daraltır, yapılandırabilmek için ve bu ölçek yaptığında. Ölçek kümeleri de etkinleştirmeniz merkezi olarak yönetin, yapılandırın ve sanal makinelerin çok sayıda farklı aracılığıyla güncelleştirme [yükseltme ilkesini](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) ayarlar. Bir güncelleştirme ölçek kümesi modeline yapılandırabilirsiniz ve yükseltme ilkesi otomatik veya çalışırken ayarladıysanız yeni yapılandırma için her bir ölçek kümesi örnek otomatik olarak uygulanır.

Olabilir, uygulamanızın trafiği işler ölçek geri kalanından farklı kabul edilmesi için belirli örnekler istediğiniz durumlarda örneği ayarlayın. Örneğin, uzun süre çalışan işlemleri ölçek kümesindeki belirli örnekleri gerçekleştiriliyor ve bu örnekler, ölçeği işlemleri tamamlanana kadar açma olmasını istemediğiniz. Ölçek kümesindeki bir ölçek kümesi üyeleri ek veya bunlardan farklı görevleri gerçekleştirmek için birkaç örneği de özelleştirilmiş. Bu 'özel' sanal makineler ölçek kümesindeki diğer örneklerle değiştirilmeyen gerekir. Bunlar ve diğer senaryolar için uygulamanızı etkinleştirmek için ek denetimler örneği koruması sağlar.

Bu makalede, uygulamak ve farklı örneği koruma özellikleri ile ölçek kümesi örneklerine kullanmak nasıl açıklanmaktadır.

> [!NOTE]
>Örnek koruma, şu anda genel Önizleme aşamasındadır. Hiçbir katılımı yordam, aşağıda açıklanan genel Önizleme işlevselliği kullanmak için gereklidir. Örnek koruma önizlemesi yalnızca ve üstü API sürümü 2019-03-01 ile desteklenir.

## <a name="types-of-instance-protection"></a>Örnek koruma türü
Ölçek kümeleri, iki tür örneği koruma özellikleri sağlar:

-   **Ölçek bileşeninden koruyun**
    - Aracılığıyla etkinleştirilen **protectFromScaleIn** ölçekte özelliğini ayarlama örneği
    - Örneği tarafından başlatılan bir otomatik ölçeklendirme ölçek bileşeninden korur
    - (Örnek silme dahil olmak üzere) örneği kullanıcı tarafından başlatılan işlemleri **engellenmedi**
    - Ölçek kümesinde başlatılan işlemleri (yükseltme, görüntüsünü yeniden oluşturma, serbest bırakın, vb.) olan **engellenmedi**

-   **Ölçek kümesi eylemlerden koruyun**
    - Aracılığıyla etkinleştirilen **protectFromScaleSetActions** ölçekte özelliğini ayarlama örneği
    - Örneği tarafından başlatılan bir otomatik ölçeklendirme ölçek bileşeninden korur
    - Örnek, Ölçek kümesinde başlatılan işlemleri önler (yükseltme gibi görüntüsünü yeniden oluşturma, serbest bırakın, vb.)
    - (Örnek silme dahil olmak üzere) örneği kullanıcı tarafından başlatılan işlemleri **engellenmedi**
    - Tam bir ölçek kümesinin Sil **engellenmedi**

## <a name="protect-from-scale-in"></a>Ölçek bileşeninden koruyun
Ölçek kümesi örneklerine örneği oluşturulduktan sonra örneği koruması uygulanabilir. Koruma uygulanır ve yalnızca değiştirilen [örnek modeli](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-vm-model-view) değil [ölçek kümesi modeline](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-model).

Aşağıdaki örneklerde açıklandığı ölçek kümesi örneklerinde ölçeğini koruma uygulayarak birden çok yolu vardır.

### <a name="rest-api"></a>REST API

Aşağıdaki örnek ölçek kümesindeki bir örneği ölçeğini koruma uygular.

```
PUT on `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualMachines/{instance-id}?api-version=2019-03-01`
```

```json
{
  "properties": {
    "protectionPolicy": {
      "protectFromScaleIn": true
    }
  }        
}

```

> [!NOTE]
>Örneği koruması yalnızca API'siyle 2019-03-01 sürümü ve üzeri desteklenir

### <a name="azure-powershell"></a>Azure PowerShell

Kullanım [güncelleştirme AzVmssVM](/powershell/module/az.compute/update-azvmssvm) cmdlet'i için ölçek kümenizi ölçeğini koruma uygulayacak şekilde ayarlama örneği.

Aşağıdaki örnek ölçek kümesindeki örnek kimliği 0 olan bir örneği ölçeğini koruma uygular.

```azurepowershell-interactive
Update-AzVmssVM `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myVMScaleSet" `
  -InstanceId 0 `
  -ProtectFromScaleIn $true
```

### <a name="azure-cli-20"></a>Azure CLI 2.0

Kullanım [az vmss update](/cli/azure/vmss#az-vmss-update) ölçek kümesi örneğinizin ölçeğini koruma uygulamak için.

Aşağıdaki örnek ölçek kümesindeki örnek kimliği 0 olan bir örneği ölçeğini koruma uygular.

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --instance-id 0 \
  --protect-from-scale-in true
```

## <a name="protect-from-scale-set-actions"></a>Ölçek kümesi eylemlerden koruyun
Ölçek kümesi örneklerine örneği oluşturulduktan sonra örneği koruması uygulanabilir. Koruma uygulanır ve yalnızca değiştirilen [örnek modeli](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-vm-model-view) değil [ölçek kümesi modeline](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-model).

Ölçek kümesi eylemlerden örneği koruma da örneği tarafından başlatılan bir otomatik ölçeklendirme ölçek bileşeninden korur.

Ölçek uygulayarak birden çok yol kümesi Eylemler koruma ölçek kümesi örneklerine aşağıdaki örneklerde açıklandığı vardır.

### <a name="rest-api"></a>REST API

Aşağıdaki örnek, bir örnek ölçek kümesindeki ölçek kümesi eylemlerine koruma uygular.

```
PUT on `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vMScaleSetName}/virtualMachines/{instance-id}?api-version=2019-03-01`
```

```json
{
  "properties": {
    "protectionPolicy": {
      "protectFromScaleIn": true,
      "protectFromScaleSetActions": true
    }
  }        
}

```

> [!NOTE]
>Örneği koruması yalnızca ve üstü API sürümü 2019-03-01 ile desteklenir.</br>
Ölçek kümesi eylemlerden örneği koruma da örneği tarafından başlatılan bir otomatik ölçeklendirme ölçek bileşeninden korur. "ProtectFromScaleIn" belirtilemez: "protectFromScaleSetActions" ayarlarken false: true

### <a name="azure-powershell"></a>Azure PowerShell

Kullanım [güncelleştirme AzVmssVM](/powershell/module/az.compute/update-azvmssvm) ölçek koruma uygulamak için cmdlet'i, Ölçek kümesi Örneğinize eylemleri ayarlayın.

Aşağıdaki örnek, Ölçek kümesi eylemleri korumadan ölçek kümesindeki örnek kimliği 0 olan bir örneği için geçerlidir.

```azurepowershell-interactive
Update-AzVmssVM `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myVMScaleSet" `
  -InstanceId 0 `
  -ProtectFromScaleIn $true `
  -ProtectFromScaleSetAction $true
```

### <a name="azure-cli-20"></a>Azure CLI 2.0

Kullanım [az vmss update](/cli/azure/vmss#az-vmss-update) ölçek kümesi Eylemlerdeki ölçek kümesi örneğinizin koruması uygulamak için.

Aşağıdaki örnek, Ölçek kümesi eylemleri korumadan ölçek kümesindeki örnek kimliği 0 olan bir örneği için geçerlidir.

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --instance-id 0 \
  --protect-from-scale-in true \
  --protect-from-scale-set-actions true
```

## <a name="troubleshoot"></a>Sorun giderme
### <a name="no-protectionpolicy-on-scale-set-model"></a>Ölçek kümesi modelinde hiçbir protectionPolicy
Örneği koruması ölçek kümesi örneklerine ve ölçek kümesi modeline göre değil yalnızca geçerlidir.

### <a name="no-protectionpolicy-on-scale-set-instance-model"></a>Hiçbir protectionPolicy ölçek kümesi örnek modeli
Oluşturulduğunda varsayılan olarak, bir örneğine koruma İlkesi uygulanmaz.

Ölçek kümesi örneklerine örneği oluşturulduktan sonra örneği koruma uygulayabilirsiniz.

### <a name="not-able-to-apply-instance-protection"></a>Örneği koruması uygulamak karşılaştırılamıyor
Örneği koruması yalnızca ve üstü API sürümü 2019-03-01 ile desteklenir. Kullanılan API sürümünü denetleyin ve gereken şekilde güncelleştirin. PowerShell veya CLI en son sürüme güncelleştirmeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [uygulamanızı dağıtmak](virtual-machine-scale-sets-deploy-app.md) üzerinde sanal makine ölçek kümeleri.
