---
title: Düşük öncelikli VM'ler (Önizleme) kullanan bir Azure ölçek kümesi oluşturma | Microsoft Docs
description: Düşük öncelikli VM'ler maliyet tasarrufu için kullandığınız bir Azure sanal makine ölçek kümeleri oluşturmayı öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: mmccrory
manager: rajraj
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: memccror
ms.openlocfilehash: 861c68ae8163e0ba8c2af2a3d96153ac3e84855f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60803206"
---
# <a name="low-priority-vms-on-scale-sets-preview"></a>Düşük öncelikli VM'ler, Ölçek kümesi (Önizleme)

Düşük öncelikli VM'ler, Ölçek kümelerinde kullanarak, bir önemli maliyet tasarrufları bizim unutilized kapasite yararlanmak sağlar. Herhangi bir noktada Azure kapasitesi geri gerektiğinde, Azure altyapısının düşük öncelikli VM'ler çıkarırsınız. Bu nedenle, düşük öncelikli VM'ler toplu işler, geliştirme ve test ortamları, büyük işlem iş yükleri ve daha fazla işleme gibi kesintiler işleyebileceği iş yükleri için mükemmeldir.

Unutilized kullanılabilir kapasite miktarı, boyut, bölge, günün saati ve daha fazla göre değişebilir. Düşük öncelikli VM'ler ölçekte dağıtma ayarlar, Azure Vm'leri kullanılabilir kapasite yoktur, ancak bu sanal makineler için SLA yoktur ayırır. Düşük öncelikli ölçek kümesi, bir tek hata etki alanında dağıtılan ve yüksek kullanılabilirlik garantileri sunar.

## <a name="eviction-policy"></a>Çıkarma İlkesi

Düşük öncelikli ölçek kümesi oluştururken, çıkarma ilkesini ayarlayın *serbest* (varsayılan) veya *Sil*. 

*Serbest* ilke çıkarılan örnekleri yeniden olanak tanıyan durduruldu-serbest durumu çıkarılan Vm'lerinizi taşır. Ancak, bu ayırma başarılı olur garantisi yoktur. Serbest VM ölçek kümesi örneği kotanız sayılır ve temel alınan diskleriniz için ücretlendirilirsiniz. 

Bunlar çıkarılacak zaman silinmesi, düşük öncelikli ölçek kümesinde sanal makinelerinizin esnetmek istiyorsanız, çıkarma ilkesini ayarlayın *Sil*. Silmek üzere ayarlanmış çıkarma İlkesi ile ölçek kümesi örneği sayısı özelliği artırarak yeni VM'ler oluşturabilirsiniz. Çıkarılan sanal makineleri, temel alınan diskleriyle birlikte silinir ve bu nedenle, depolama alanı için ücretlendirilmez. Otomatik olarak deneyin ve çıkarılan VM'ler için dengelemek için ölçek kümelerini otomatik ölçeklendirme özelliğini de kullanabilirsiniz, ancak ayırma başarılı bir garanti yoktur. Disklerinizi ve kota sınırlarını ulaşma maliyetini önlemek için silmek için çıkarma İlkesi ayarladığınızda, yalnızca düşük öncelikli ölçek kümelerinde otomatik ölçeklendirme özelliğini kullanmanız önerilir. 

> [!NOTE]
> Önizleme süresince, kullanarak, çıkarma İlkesi ayarlamak mümkün olmayacak [Azure portalında](#use-the-azure-portal) ve [Azure Resource Manager şablonları](#use-azure-resource-manager-templates). 

## <a name="deploying-low-priority-vms-on-scale-sets"></a>Düşük öncelikli VM'ler ölçekte dağıtma ayarlar

Düşük öncelikli VM'ler, Ölçek kümesi dağıtmak için yeni ayarlayabileceğiniz *öncelik* bayrak *düşük*. Ölçek kümesindeki tüm sanal makineler, düşük öncelikli olacak şekilde ayarlanacaktır. Düşük öncelikli VM ile bir ölçek kümesi oluşturmak için aşağıdaki yöntemlerden birini kullanın:
- [Azure portal](#use-the-azure-portal)
- Azure CLI
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager şablonları](#use-azure-resource-manager-templates)

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Düşük öncelikli VM'ler kullanan bir ölçek kümesi oluşturma işlemi ayrıntılı olarak aynıdır [makale Başlarken](quick-create-portal.md). Bir ölçek kümesi dağıtırken, düşük öncelik bayrağını ve çıkarma ilkesini ayarlamayı da seçebilirsiniz: ![Düşük öncelikli VM ile bir ölçek kümesi oluşturma](media/virtual-machine-scale-sets-use-low-priority/vmss-low-priority-portal.png)

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Düşük öncelikli VM ile bir ölçek kümesi oluşturma işlemi ayrıntılı olarak aynıdır [makale Başlarken](quick-create-cli.md). Yalnızca Ekle '--öncelik ' parametresi için CLI'yı arayın ve ayarlamak *düşük* aşağıdaki örnekte gösterildiği gibi:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Low
```

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Düşük öncelikli VM ile bir ölçek kümesi oluşturma işlemi ayrıntılı olarak aynıdır [makale Başlarken](quick-create-powershell.md).
Yalnızca Ekle '-öncelik ' parametresi [yeni AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) ve ayarlamak *düşük* aşağıdaki örnekte gösterildiği gibi:

```powershell
$vmssConfig = New-AzVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Priority "Low"
```

## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma

Düşük öncelikli VM'ler kullanan bir ölçek kümesi oluşturma işlemi başlangıç makalesinde için ayrıntılı aynıdır [Linux](quick-create-template-linux.md) veya [Windows](quick-create-template-windows.md). 'Priority' özelliği için ekleme *Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile* kaynak türü, şablonunuzda ve belirtin *düşük* değeri. Kullandığınızdan emin olun *2018-03-01* API sürümü veya üzeri. 

Silme işlemini çıkarma ilkesini ayarlamak için 'evictionPolicy' parametresini ekleyin ve değerini *Sil*.

Aşağıdaki örnekte adlı bir Linux düşük öncelikli ölçek kümesi oluşturur *myScaleSet* içinde *Batı Orta ABD*, hangilerinin *Sil* VM ölçek kümesindeki çıkarma üzerinde:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2018-03-01",
  "sku": {
    "name": "Standard_DS2_v2",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
       "priority": "Low",
       "evictionPolicy": "delete",
       "storageProfile": {
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage"
        },
        "imageReference":  {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "16.04-LTS",
          "version": "latest"
        }
      },
      "osProfile": {
        "computerNamePrefix": "myvmss",
        "adminUsername": "azureuser",
        "adminPassword": "P@ssw0rd!"
      }
    }
  }
}
```
## <a name="faq"></a>SSS

### <a name="can-i-convert-existing-scale-sets-to-low-priority-scale-sets"></a>Düşük öncelikli ölçek kümeleri için var olan ölçek kümeleri dönüştürebilir miyim?
Hayır, düşük öncelik bayrağını ayarlama yalnızca oluşturma sırasında desteklenir.

### <a name="can-i-create-a-scale-set-with-both-regular-vms-and-low-priority-vms"></a>Normal Vm'lere ve düşük öncelikli VM'ler ile ölçek oluşturabilir miyim?
Hayır, bir ölçek kümesi birden fazla öncelik türünü desteklemiyor.

### <a name="how-is-quota-managed-for-low-priority-vms"></a>Kota, düşük öncelikli VM'ler için nasıl yönetilir?
Düşük öncelikli VM'ler ve normal Vm'leri aynı kota havuzu paylaşır. 

### <a name="can-i-use-autoscale-with-low-priority-scale-sets"></a>Düşük öncelikli ölçek kümeleriyle otomatik ölçeklendirme kullanabilir miyim?
Evet, düşük öncelikli ölçek kümenizi otomatik ölçeklendirme kuralları ayarlayabilirsiniz. Sanal makinelerinizin çıkarılan, yeni düşük öncelikli VM'ler oluşturmak otomatik ölçeklendirme deneyebilirsiniz. Unutmayın, bu kapasiteyi yine de garanti edilmez. 

### <a name="does-autoscale-work-with-both-eviction-policies-deallocate-and-delete"></a>Her iki çıkarma ilkeleri ile otomatik ölçeklendirme işi yapar (serbest bırakma ve silme)?
Otomatik ölçeklendirme kullanırken silmek için çıkarma ilkesini ayarlamanız önerilir. Serbest örnekleri, Ölçek kümesinde kapasite sayınız karşı sayılan olmasıdır. Otomatik ölçeklendirme kullanırken, büyük olasılıkla hedef örnek sayınız kaldırıldığından, çıkarılan örnekleri nedeniyle hızlı bir şekilde ulaşırsınız. 

## <a name="next-steps"></a>Sonraki adımlar
Düşük öncelikli VM ile bir ölçek kümesi oluşturduğunuza göre dağıtmayı deneyin. bizim [düşük öncelikli kullanarak otomatik ölçek şablon](https://github.com/Azure/vm-scale-sets/tree/master/preview/lowpri).

Kullanıma [Fiyatlandırma sayfasında sanal makine ölçek kümesi](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) fiyatlandırma ayrıntıları için.
