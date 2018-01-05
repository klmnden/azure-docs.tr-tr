---
title: "Kullanılabilirlik bölgeleri (Önizleme) kullanan bir Azure ölçek kümesi oluşturma | Microsoft Docs"
description: "Kesintilere karşı artıklığı için kullanılabilirlik bölgeleri kullanmak Azure sanal makine ölçek kümeleri oluşturma hakkında bilgi edinin"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: iainfou
ms.openlocfilehash: 310fdc68d3eb662906053d2bc0c45e6cfa18d4da
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones-preview"></a>Kullanılabilirlik bölgeleri (Önizleme) kullanan bir sanal makine ölçek kümesi oluşturma
Sanal makine ölçek kümeleri datacenter düzeyi arızasına karşı korumak için bir kullanılabilirlik dilimini ölçek oluşturabilirsiniz. Kullanılabilirlik bölgeleri destekleyen azure bölgeleri sahip en az üç ayrı bölgeler, her biri kendi bağımsız güç kaynağı, ağ ve soğutma. Daha fazla bilgi için bkz: [kullanılabilirlik bölgeleri genel bakış](../availability-zones/az-overview.md).

Kullanılabilirlik bölgeleri kullanmak için ölçek kümesini oluşturulmalıdır bir [Azure bölgesi desteklenen](../availability-zones/az-overview.md#regions-that-support-availability-zones). Aşağıdaki yöntemlerden biriyle kullanılabilirlik bölgeleri kullanan bir ölçek kümesi oluşturabilirsiniz:

- [Azure portalı](#use-the-azure-portal)
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager şablonları](#use-azure-resource-manager-templates)


## <a name="use-the-azure-portal"></a>Azure portalı kullanma
Bir kullanılabilirlik bölge kullanan bir ölçek kümesi oluşturmak için işlem içinde ayrıntılı olarak aynıdır [makale Başlarken](virtual-machine-scale-sets-create-portal.md). Desteklenen bir Azure bölgesini seçtiğinizde, aşağıdaki örnekte gösterildiği gibi bir ölçeği kullanılabilen bölgeler, birini Ayarla oluşturabilirsiniz:

![Ölçeği tek bir kullanılabilirlik bölgesinde Ayarla oluşturma](media/virtual-machine-scale-sets-use-availability-zones/create-portal-single-az.png)

Ölçek kümesi ve Azure yük dengeleyici ve genel IP adresi gibi kaynakları destekleyen belirttiğiniz tek bir bölge içinde oluşturulur.


## <a name="use-the-azure-cli-20"></a>Azure CLI 2.0 kullanın
Bir kullanılabilirlik bölge kullanan bir ölçek kümesi oluşturmak için işlem içinde ayrıntılı olarak aynıdır [makale Başlarken](virtual-machine-scale-sets-create-cli.md). Kullanılabilirlik bölgeleri kullanmak için desteklenen bir Azure bölgesinde ayarlayın, Ölçek oluşturmanız gerekir. Ekleme `--zones` parametresi [az vmss oluşturmak](/cli/azure/vmss#az_vmss_create) komut ve kullanmak için hangi bölgede belirtin (bölge gibi *1*, *2*, veya *3*). Aşağıdaki örnek, ölçeği adlandırılmış Ayarla oluşturur *myScaleSet* bölgesinde *1*:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones 1
```

Oluşturun ve tüm belirttiğiniz bölgesinde kaynakları ve VM ölçek kümesi yapılandırmak için birkaç dakika sürer.


## <a name="use-azure-powershell"></a>Azure PowerShell kullanma
Bir kullanılabilirlik bölge kullanan bir ölçek kümesi oluşturmak için işlem içinde ayrıntılı olarak aynıdır [makale Başlarken](virtual-machine-scale-sets-create-powershell.md). Kullanılabilirlik bölgeleri kullanmak için desteklenen bir Azure bölgesinde ayarlayın, Ölçek oluşturmanız gerekir. Ekleme `-Zone` parametresi [yeni AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) komut ve kullanmak için hangi bölgede belirtin (bölge gibi *1*, *2*, veya *3*). Aşağıdaki örnek, adlandırılmış bir ölçek kümesi config oluşturur *vmssConfig* içinde *Doğu ABD 2* bölge *1*:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1"
```

Gerçek ölçek kümesi oluşturmak için ayrıntılı biçimde açıklandığı gibi ek adımlar uygulayın [makale Başlarken](virtual-machine-scale-sets-create-powershell.md).


## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma
Bir kullanılabilirlik bölge kullanan bir ölçek kümesi oluşturma işlemi için alma başlatılan makalesinde ayrıntılı olarak aynıdır [Linux](virtual-machine-scale-sets-create-template-linux.md) veya [Windows](virtual-machine-scale-sets-create-template-windows.md). Kullanılabilirlik bölgeleri kullanmak için desteklenen bir Azure bölgesinde ayarlayın, Ölçek oluşturmanız gerekir. Ekleme `zones` özelliğine *Microsoft.Compute/virtualMachineScaleSets* kaynak türü şablonunuzda ve kullanmak için hangi bölgede belirtin (bölge gibi *1*, *2*, veya *3*). Aşağıdaki örnek, bir Linux ölçeği adlandırılmış Ayarla oluşturur *myScaleSet* içinde *Doğu ABD 2* bölge *1*:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "zones": ["1"],
  "sku": {
    "name": "Standard_A1",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
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

Gerçek ölçek kümesi oluşturmak için ek adımlar için alma başlatılan makalesinde ayrıntılı olarak izleyin [Linux](virtual-machine-scale-sets-create-template-linux.md) veya [Windows](virtual-machine-scale-sets-create-template-windows.md)


## <a name="next-steps"></a>Sonraki adımlar
Ölçeği bir kullanılabilirlik bölgesinde Ayarla oluşturduğunuza göre öğrenebilirsiniz nasıl [dağıtma uygulamaları sanal makineye ölçek kümeleri](virtual-machine-scale-sets-deploy-app.md) veya [ile sanal makine ölçek kümeleri otomatik ölçeklendirme kullanmak](virtual-machine-scale-sets-autoscale-overview.md).
