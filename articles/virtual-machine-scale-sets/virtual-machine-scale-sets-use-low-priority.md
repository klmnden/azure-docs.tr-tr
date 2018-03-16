---
title: "Düşük öncelikli sanal makineleri (Önizleme) kullanan bir Azure ölçek kümesi oluşturma | Microsoft Docs"
description: "Düşük öncelikli sanal makineleri giderlerinden tasarruf edersiniz kullanmasını Azure sanal makine ölçek kümeleri oluşturma hakkında bilgi edinin"
services: virtual-machine-scale-sets
documentationcenter: 
author: mmccrory
manager: rajraj
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: memccror
ms.openlocfilehash: 9e4970ecc538caab537281931b89bfd57d994cfa
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="low-priority-vms-on-scale-sets-preview"></a>Düşük öncelikli sanal makine ölçek kümeleri (Önizleme)

Düşük öncelikli sanal makineleri ölçek kümeleri kullanarak önemli maliyet tasarrufları bizim unutilized kapasite yararlanmak sağlar. Azure kapasite geri gerektiği zaman zaman içinde herhangi bir noktada, Azure altyapı düşük öncelikli sanal makineleri çıkarırsınız. Bu nedenle, düşük öncelikli sanal makineleri toplu işleri, geliştirme ve test ortamları, büyük hesaplama iş yüklerini ve daha fazla işleme gibi kesintilerine işleyebilir iş yükleri için mükemmeldir.

Kullanılabilir unutilized kapasite miktarı boyutu, bölge, günün saatini ve daha fazla göre değişebilir. Düşük öncelikli sanal makine ölçek dağıtma ayarladığında Azure VM'ler kullanılabilir kapasite, ancak bu VM'ler için hiçbir SLA ayırır. Düşük öncelikli ölçek kümesini tek hata etki alanında dağıtılır ve yüksek kullanılabilirliği garanti sunar.

> [!NOTE]
> Düşük öncelikli ölçek kümeleri önizlemede ve geliştirme ve test senaryoları için hazır. 

## <a name="eviction-policy"></a>Çıkarma İlkesi

Düşük öncelikli ölçek ayarladığınızda VM'ler çıkarılacak, bunlar varsayılan olarak durduruldu (serbest bırakıldı) durumuna taşınır. Bu çıkarma ilkesiyle çıkarılan örnekleri yeniden dağıtabilirsiniz, ancak garanti ayırmayı başarılı olur. Durdurulan VM ölçek kümesi örnek kotanızı karşı sayılacaktır ve temel diskleriniz için sizden ücret alınır. 

Düşük öncelikli ölçeği çıkarılacak zaman silinecek Ayarla Vm'leriniz kullanmak isterseniz, silmek için çıkarma ilkesi ayarlayabilirsiniz, [Azure Resource Manager şablonu](#use-azure-resource-manager-templates). Silmek üzere ayarlanmış çıkarma İlkesi ile ölçek kümesi örnek sayısı özelliği artırarak yeni VM'ler oluşturabilirsiniz. Çıkarılan VM'ler kendi temel diskleri birlikte silinir ve bu nedenle, depolama alanı için ücret alınmaz. Otomatik olarak deneyin ve çıkarılan VM'ler için dengelemek için ölçek kümeleri otomatik ölçeklendirme özelliğini de kullanabilirsiniz, ancak ayırma başarılı olur garanti yoktur. Disklerinizi ve kota sınırları basarsa maliyetini önlemek için silmek için çıkarma İlkesi ayarladığınızda, yalnızca düşük öncelikli ölçek kümeleri üzerinde otomatik ölçeklendirmeye özelliği kullanmanız önerilir. 

> [!NOTE]
> Önizleme sırasında kullanarak çıkarma ilkenizi ayarlamak kullanamazsınız [Azure Resource Manager şablonları](#use-azure-resource-manager-templates). 

## <a name="deploying-low-priority-vms-on-scale-sets"></a>Düşük öncelikli sanal makine ölçek dağıtma ayarlar

Düşük öncelikli VM ölçek kümesi üzerinde dağıtmak için yeni ayarlayabileceğiniz *öncelik* bayrağını *düşük*. Düşük öncelikli ölçek kümenizdeki tüm VM'ler ayarlanır. Düşük öncelikli sanal makineleri ile ayarlamak ölçeği oluşturmak için aşağıdaki yöntemlerden birini kullanın:
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager şablonları](#use-azure-resource-manager-templates)

## <a name="use-the-azure-cli-20"></a>Azure CLI 2.0 kullanın

Düşük öncelikli sanal makineleri ile ayarlamak ölçek oluşturma işlemi içinde ayrıntılı olarak aynıdır [makale Başlarken](virtual-machine-scale-sets-create-cli.md). Yalnızca Ekle '--öncelik ' CLI parametresi çağırın ve ayarlamak *düşük* aşağıdaki örnekte gösterildiği gibi:

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

Düşük öncelikli sanal makineleri ile ayarlamak ölçek oluşturma işlemi içinde ayrıntılı olarak aynıdır [makale Başlarken](virtual-machine-scale-sets-create-powershell.md).
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

Düşük öncelikli sanal makineleri kullanan bir ölçek kümesi oluşturma işlemi için alma başlatılan makalesinde ayrıntılı olarak aynıdır [Linux](virtual-machine-scale-sets-create-template-linux.md) veya [Windows](virtual-machine-scale-sets-create-template-windows.md). 'Priority' özelliği Ekle *Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile* kaynak şablonunuzda ve *düşük* değeri olarak. Kullandığınızdan emin olun *2017-10-30-Önizleme* API sürümü veya daha yüksek. 

Silme için çıkarma İlkesi'ni ayarlamak için 'evictionPolicy' parametresini ekleyin ve ayarlamak *silmek*.

Aşağıdaki örnek, bir Linux düşük öncelikli ölçeği adlandırılmış Ayarla oluşturur *myScaleSet* içinde *Batı Orta ABD*, hangilerinin *silmek* Vm'lerde ölçeği çıkarma üzerinde ayarla:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
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
## <a name="next-steps"></a>Sonraki adımlar
Düşük öncelikli sanal makineleri ile ayarlamak ölçek oluşturduğunuza göre dağıtmayı deneyin bizim [düşük öncelikli kullanarak otomatik ölçek şablonunu](https://github.com/Azure/vm-scale-sets/tree/master/preview/lowpri).

Kullanıma [fiyatlandırma sayfası sanal makine ölçek kümesi](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) fiyatlandırma ayrıntıları için.