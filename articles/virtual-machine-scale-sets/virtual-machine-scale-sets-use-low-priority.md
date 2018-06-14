---
title: Düşük öncelikli sanal makineleri (Önizleme) kullanan bir Azure ölçek kümesi oluşturma | Microsoft Docs
description: Düşük öncelikli sanal makineleri giderlerinden tasarruf edersiniz kullanmasını Azure sanal makine ölçek kümeleri oluşturma hakkında bilgi edinin
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
ms.openlocfilehash: 5c0726ea0da288d5306e28b101e4d3b59605b443
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33894919"
---
# <a name="low-priority-vms-on-scale-sets-preview"></a>Düşük öncelikli sanal makine ölçek kümeleri (Önizleme)

Düşük öncelikli sanal makineleri ölçek kümeleri kullanarak önemli maliyet tasarrufları bizim unutilized kapasite yararlanmak sağlar. Azure kapasite geri gerektiği zaman zaman içinde herhangi bir noktada, Azure altyapı düşük öncelikli sanal makineleri çıkarırsınız. Bu nedenle, düşük öncelikli sanal makineleri toplu işleri, geliştirme ve test ortamları, büyük hesaplama iş yüklerini ve daha fazla işleme gibi kesintilerine işleyebilir iş yükleri için mükemmeldir.

Kullanılabilir unutilized kapasite miktarı boyutu, bölge, günün saatini ve daha fazla göre değişebilir. Düşük öncelikli sanal makine ölçek dağıtma ayarladığında Azure VM'ler kullanılabilir kapasite, ancak bu VM'ler için hiçbir SLA ayırır. Düşük öncelikli ölçek kümesini tek hata etki alanında dağıtılır ve yüksek kullanılabilirliği garanti sunar.

## <a name="eviction-policy"></a>Çıkarma İlkesi

Düşük öncelikli ölçek kümeleri oluştururken, çıkarma ilkesi ayarlayabilirsiniz *Deallocate* (varsayılan) veya *silmek*. 

*Deallocate* İlkesi çıkarılan örnekleri yeniden dağıtmak sağlayarak durduruldu-serbest durumu çıkarılan Vm'leriniz taşır. Ancak, ayırmayı başarılı olur garanti yoktur. Deallocated VM ölçek kümesi örnek kotanızı karşı sayılacaktır ve temel diskleriniz için sizden ücret alınır. 

Düşük öncelikli ölçeği çıkarılacak zaman silinecek Ayarla Vm'leriniz kullanmak isterseniz, çıkarma İlkesi ayarlayabileceğiniz *silmek*. Silmek üzere ayarlanmış çıkarma İlkesi ile ölçek kümesi örnek sayısı özelliği artırarak yeni VM'ler oluşturabilirsiniz. Çıkarılan VM'ler kendi temel diskleri birlikte silinir ve bu nedenle, depolama alanı için ücret alınmaz. Otomatik olarak deneyin ve çıkarılan VM'ler için dengelemek için ölçek kümeleri otomatik ölçeklendirme özelliğini de kullanabilirsiniz, ancak ayırma başarılı olur garanti yoktur. Disklerinizi ve kota sınırları basarsa maliyetini önlemek için silmek için çıkarma İlkesi ayarladığınızda, yalnızca düşük öncelikli ölçek kümeleri üzerinde otomatik ölçeklendirmeye özelliği kullanmanız önerilir. 

> [!NOTE]
> Önizleme sırasında kullanarak çıkarma ilkenizi ayarlamak kullanamazsınız [Azure portal](#use-the-azure-portal) ve [Azure Resource Manager şablonları](#use-azure-resource-manager-templates). 

## <a name="deploying-low-priority-vms-on-scale-sets"></a>Düşük öncelikli sanal makine ölçek dağıtma ayarlar

Düşük öncelikli VM ölçek kümesi üzerinde dağıtmak için yeni ayarlayabileceğiniz *öncelik* bayrağını *düşük*. Düşük öncelikli ölçek kümenizdeki tüm VM'ler ayarlanır. Düşük öncelikli sanal makineleri ile ayarlamak ölçeği oluşturmak için aşağıdaki yöntemlerden birini kullanın:
- [Azure Portal](#use-the-azure-portal)
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager şablonları](#use-azure-resource-manager-templates)

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Düşük öncelikli sanal makineleri kullanan bir ölçek kümesi oluşturmak için işlem içinde ayrıntılı olarak aynıdır [makale Başlarken](quick-create-portal.md). Ölçek kümesini dağıtırken, düşük öncelik bayrağını ve çıkarma ilkesini ayarlamayı da seçebilirsiniz: ![ölçeği olan düşük öncelikli VM'ler Ayarla oluşturma](media/virtual-machine-scale-sets-use-low-priority/vmss-low-priority-portal.png)

## <a name="use-the-azure-cli-20"></a>Azure CLI 2.0 kullanın

Düşük öncelikli sanal makineleri ile ayarlamak ölçek oluşturma işlemi içinde ayrıntılı olarak aynıdır [makale Başlarken](quick-create-cli.md). Yalnızca Ekle '--öncelik ' CLI parametresi çağırın ve ayarlamak *düşük* aşağıdaki örnekte gösterildiği gibi:

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

Düşük öncelikli sanal makineleri ile ayarlamak ölçek oluşturma işlemi içinde ayrıntılı olarak aynıdır [makale Başlarken](quick-create-powershell.md).
Yalnızca Ekle '-öncelik ' parametresi [yeni AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) ve ayarlamak *düşük* aşağıdaki örnekte gösterildiği gibi:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Priority "Low"
```

## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kullanın

Düşük öncelikli sanal makineleri kullanan bir ölçek kümesi oluşturma işlemi için alma başlatılan makalesinde ayrıntılı olarak aynıdır [Linux](quick-create-template-linux.md) veya [Windows](quick-create-template-windows.md). 'Priority' özelliği Ekle *Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile* kaynak şablonunuzda ve *düşük* değeri olarak. Kullandığınızdan emin olun *2018-03-01* API sürümü veya daha yüksek. 

Silme için çıkarma İlkesi'ni ayarlamak için 'evictionPolicy' parametresini ekleyin ve ayarlamak *silmek*.

Aşağıdaki örnek, bir Linux düşük öncelikli ölçeği adlandırılmış Ayarla oluşturur *myScaleSet* içinde *Batı Orta ABD*, hangilerinin *silmek* Vm'lerde ölçeği çıkarma üzerinde ayarla:

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
Hayır, düşük öncelik bayrağını ayarı yalnızca oluşturma sırasında desteklenir.

### <a name="can-i-create-a-scale-set-with-both-regular-vms-and-low-priority-vms"></a>Normal VM'ler ve düşük öncelikli sanal makineleri ile ayarlanmış bir ölçek oluşturabilir miyim?
Hayır, Ölçek kümesini birden fazla öncelik türünü desteklemiyor.

### <a name="how-is-quota-managed-for-low-priority-vms"></a>Kota düşük öncelikli VM'ler için nasıl yönetilir?
Düşük öncelikli sanal makineleri ve normal sanal makineleri aynı kota havuzu paylaşır. 

### <a name="can-i-use-autoscale-with-low-priority-scale-sets"></a>Düşük öncelikli ölçek kümeleri otomatik ölçeklendirme kullanabilir miyim?
Evet, düşük öncelikli ölçek kümesinde otomatik ölçeklendirmeyi kurallar ayarlayabilirsiniz. Vm'leriniz çıkarılacak varsa, otomatik ölçeklendirme yeni düşük öncelikli sanal makineleri oluşturmayı deneyebilirsiniz. Unutmayın, bu kapasite ancak garanti edilmez. 

### <a name="does-autoscale-work-with-both-eviction-policies-deallocate-and-delete"></a>Mu otomatik ölçeklendirme iş hem çıkarma ilkeleriyle (serbest bırakma ve silme)?
Otomatik ölçeklendirme kullanırken silmek için çıkarma ilkesi ayarlamanız önerilir. Deallocated örnekleri kapasite sayınız karşı ölçek kümesinde sayılır olmasıdır. Otomatik ölçeklendirme kullanırken, büyük olasılıkla hedef örnek sayınız kaldırıldığından, çıkarılan örnekleri nedeniyle hızlı bir şekilde karşılaşır. 

## <a name="next-steps"></a>Sonraki adımlar
Düşük öncelikli sanal makineleri ile ayarlamak ölçek oluşturduğunuza göre dağıtmayı deneyin bizim [düşük öncelikli kullanarak otomatik ölçek şablonunu](https://github.com/Azure/vm-scale-sets/tree/master/preview/lowpri).

Kullanıma [fiyatlandırma sayfası sanal makine ölçek kümesi](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) fiyatlandırma ayrıntıları için.