---
title: Azure Sanal Makine Ölçek Kümeleri Bağlı Veri Diskleri | Microsoft Belgeleri
description: Sanal makine ölçek kümelerinde bağlı veri disklerinin nasıl kullanılacağını öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 4/25/2017
ms.author: manayar
ms.openlocfilehash: 5482e082e3e37d279c4374a8642f2cb6db588dd5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60328842"
---
# <a name="azure-virtual-machine-scale-sets-and-attached-data-disks"></a>Azure sanal makine ölçek kümeleri ve bağlı veri diskleri
Kullanılabilir depolama alanınızı genişletmek için Azure [sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets/), bağlı veri diskleri içeren sanal makine örneklerini destekler. Ölçek kümesi oluşturulduğunda veya mevcut bir ölçek kümesine veri diskleri ekleyebilirsiniz.

> [!NOTE]
> Bağlı veri diskleri içeren bir ölçek kümesi oluşturduğunuzda tek başına Azure sanal makinelerinde olduğu gibi, diskleri kullanabilmek için bir sanal makine içinde takmanız ve biçimlendirmeniz gerekir. Bu işlemi tamamlamanın uygun bir yolu, bir sanal makine üzerindeki tüm verileri bölümlemek ve biçimlendirmek için bir betik çağıran Özel Betik Uzantısı kullanılmasıdır. Bununla ilgili örnekler için bkz. [Azure CLI](tutorial-use-disks-cli.md#prepare-the-data-disks) [Azure PowerShell](tutorial-use-disks-powershell.md#prepare-the-data-disks).


## <a name="create-and-manage-disks-in-a-scale-set"></a>Ölçek kümesinde diskler oluşturma ve yönetme
Bağlı veri diskleri içeren bir ölçek kümesi oluşturma, veri diskleri hazırlayıp biçimlendirme veya veri diskleri ekleme ve kaldırma işlemlerine ilişkin ayrıntılı bilgiler için aşağıdaki öğreticilerden birine bakın:

- [Azure CLI](tutorial-use-disks-cli.md)
- [Azure PowerShell](tutorial-use-disks-powershell.md)

Bu makalenin geri kalanında, veri diskleri gerektiren Service Fabric kümeleri gibi belirli kullanım senaryoları veya bir ölçek kümesine içerik barındıran mevcut veri disklerinin eklenmesi açıklanmaktadır.


## <a name="create-a-service-fabric-cluster-with-attached-data-disks"></a>Eklenen veri diskleri ile bir Service Fabric kümesi oluşturma
Azure’da çalışan bir [Service Fabric](/azure/service-fabric) kümesindeki her [düğüm türü](../service-fabric/service-fabric-cluster-nodetypes.md), bir sanal makine ölçek kümesi tarafından desteklenir. Azure Resource Manager şablonunu kullanarak, Service Fabric kümesini oluşturan ölçek kümelerine veri diskleri ekleyebilirsiniz. Başlangıç noktası olarak [mevcut bir şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates) kullanabilirsiniz. Şablonda, _Microsoft.Compute/virtualMachineScaleSets_ kaynaklarının _storageProfile_ seçeneğine _dataDisks_ bölümü ekleyin ve şablonu dağıtın. Aşağıdaki örnekte 128 GB veri diski kullanıma açılır:

```json
"dataDisks": [
    {
    "diskSizeGB": 128,
    "lun": 0,
    "createOption": "Empty"
    }
]
```

Küme dağıtıldığında veri disklerini otomatik olarak bölebilir, biçimlendirebilir ve bağlayabilirsiniz. Ölçek kümelerinin _virtualMachineProfile_ bölümünün _extensionProfile_ öğesine özel bir betik uzantısı ekleyin.

Bir Windows kümesinde veri disklerini otomatik olarak hazırlamak için şunları ekleyin:

```json
{
    "name": "customScript",
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.8",
        "autoUpgradeMinorVersion": true,
        "settings": {
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.ps1"
        ],
        "commandToExecute": "powershell -ExecutionPolicy Unrestricted -File prepare_vm_disks.ps1"
        }
    }
}
```
Bir Linux kümesinde veri disklerini otomatik olarak hazırlamak için şunları ekleyin:
```json
{
    "name": "lapextension",
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"
        ],
        "commandToExecute": "bash prepare_vm_disks.sh"
        }
    }
}
```


## <a name="adding-pre-populated-data-disks-to-an-existing-scale-set"></a>Önceden doldurulmuş veri disklerini mevcut bir ölçek kümesine ekleme
Ölçek kümesi modelinde belirtilen veri diskleri her zaman boştur. Ancak bir ölçek kümesindeki belirli bir VM’ye var olan bir veri diski ekleyebilirsiniz. Bu özellik Önizleme aşamasında örnekleriyle açıktır [GitHub](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk). Verileri ölçek kümesindeki tüm VM’lere yaymak istiyorsanız, veri diskinizi çoğaltarak ölçek kümesindeki her bir VM’ye ekleyebilirsiniz, verileri içeren özel bir görüntü oluşturup ölçek kümesini bu özel görüntüden sağlayabilirsiniz veya Azure Dosyalar ya da benzer bir veri depolama teklifi kullanabilirsiniz.


## <a name="additional-notes"></a>Ek notlar
Azure Yönetilen diskleri ve ölçek kümesi bağlı veri diskleri için destek, Microsoft.Compute API’sinin [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/compute/resource-manager/Microsoft.Compute/preview/2016-04-30-preview/compute.json) veya üstü sürümlere eklenmiştir.

Ölçek kümelerindeki bağlı veri diskleri için Azure portalı desteği başlangıçta sınırlıydı. Gereksinimlerinize bağlı olarak, bağlı diskleri yönetmek için Azure şablonları, CLI, PowerShell, SDK’lar ve REST API kullanabilirsiniz.


