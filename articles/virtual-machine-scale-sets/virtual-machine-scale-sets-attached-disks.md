---
title: Azure Sanal Makine Ölçek Kümeleri Bağlı Veri Diskleri | Microsoft Belgeleri
description: Sanal makine ölçek kümelerinde bağlı veri disklerinin nasıl kullanılacağını öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: negat
ms.openlocfilehash: ec11a2d66530129fb61d97681e6882b887c8654c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-virtual-machine-scale-sets-and-attached-data-disks"></a>Azure sanal makine ölçek kümeleri ve bağlı veri diskleri
Kullanılabilir depolama alanınızı genişletmek için Azure [sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets/), bağlı veri diskleri içeren sanal makine örneklerini destekler. Ölçek kümesi oluşturulduğunda veya mevcut bir ölçek kümesine veri diskleri ekleyebilirsiniz.

> [!NOTE]
>  Bağlı veri diskleri içeren bir ölçek kümesi oluşturduğunuzda tek başına Azure sanal makinelerinde olduğu gibi, diskleri kullanabilmek için bir sanal makine içinde takmanız ve biçimlendirmeniz gerekir. Bu işlemi tamamlamanın uygun bir yolu, bir sanal makine üzerindeki tüm verileri bölümlemek ve biçimlendirmek için bir betik çağıran Özel Betik Uzantısı kullanılmasıdır. Bununla ilgili örnekler için bkz. [Azure CLI 2.0](tutorial-use-disks-cli.md#prepare-the-data-disks) [Azure PowerShell](tutorial-use-disks-powershell.md#prepare-the-data-disks).


## <a name="create-and-manage-disks-in-a-scale-set"></a>Ölçek kümesinde diskler oluşturma ve yönetme
Bağlı veri diskleri içeren bir ölçek kümesi oluşturma, veri diskleri hazırlayıp biçimlendirme veya veri diskleri ekleme ve kaldırma işlemlerine ilişkin ayrıntılı bilgiler için aşağıdaki öğreticilerden birine bakın:

- [Azure CLI 2.0](tutorial-use-disks-cli.md)
- [Azure PowerShell](tutorial-use-disks-powershell.md)

Bu makalenin geri kalanında, veri diskleri gerektiren Service Fabric kümeleri gibi belirli kullanım senaryoları veya bir ölçek kümesine içerik barındıran mevcut veri disklerinin eklenmesi açıklanmaktadır.


## <a name="create-a-service-fabric-cluster-with-attached-data-disks"></a>Eklenen veri diskleri ile bir Service Fabric kümesi oluşturma
Azure’da çalışan bir [Service Fabric](/azure/service-fabric) kümesindeki her [düğüm türü](../service-fabric/service-fabric-cluster-nodetypes.md), bir sanal makine ölçek kümesi tarafından desteklenir.  Azure Resource Manager şablonunu kullanarak, Service Fabric kümesini oluşturan ölçek kümelerine veri diskleri ekleyebilirsiniz. Başlangıç noktası olarak [mevcut bir şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates) kullanabilirsiniz. Şablonda, _Microsoft.Compute/virtualMachineScaleSets_ kaynaklarının _storageProfile_ seçeneğine _dataDisks_ bölümü ekleyin ve şablonu dağıtın. Aşağıdaki örnekte 128 GB veri diski kullanıma açılır:

```json
"dataDisks": [
    {
    "diskSizeGB": 128,
    "lun": 0,
    "createOption": "Empty"
    }
]
```

Küme dağıtıldığında veri disklerini otomatik olarak bölebilir, biçimlendirebilir ve bağlayabilirsiniz.  Ölçek kümelerinin _virtualMachineProfile_ bölümünün _extensionProfile_ öğesine özel bir betik uzantısı ekleyin.

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


## <a name="adding-pre-populated-data-disks-to-an-existent-scale-set"></a>Önceden doldurulmuş veri disklerini mevcut bir ölçek kümesine ekleme 
> Tasarım gereği, mevcut ölçek kümesi modeline disk eklediğinizde, disk her zaman boş oluşturulur. Bu senaryo ayrıca ölçek kümesi tarafından oluşturulan yeni örnekleri içerir. Bu davranışın nedeni ölçek kümesinin boş veri diski tanımına sahip olmasıdır. Mevcut bir ölçek kümesi modeli için önceden doldurulmuş veri sürücüleri oluşturmak için iki seçenekten birini seçebilirsiniz:

* Örnek 0 VM’sinden verileri özel bir betik çalıştırarak diğer VM’lerdeki veri disklerine kopyalayabilirsiniz.
* İşletim sistemi diskine ve veri diskine (gerekli verilerle) sahip yönetilen bir görüntü oluşturup görüntüde yeni bir ölçek kümesi oluşturabilirsiniz. Bu şekilde oluşturulan her yeni VM, ölçek kümesi tanımında sağlanan bir veri diskine sahip olur. Bu tanım, özelleştirilmiş verilere sahip bir veri disk görüntüsüne başvuracağından, ölçek kümesindeki her sanal makine bu değişiklikleri otomatik olarak içerir.

> Özel bir görüntü oluşturmak için izlenecek yol şurada bulunabilir: [Azure’da genelleştirilmiş bir VM’nin yönetilen görüntüsü oluşturma](/azure/virtual-machines/windows/capture-image-resource/) 

> Kullanıcının gerekli verileri içeren örnek 0 VM’yi yakalaması, ardından da görüntü tanımı için bu VHD’yi kullanması gerekir.


## <a name="additional-notes"></a>Ek notlar
Azure Yönetilen diskleri ve ölçek kümesi bağlı veri diskleri için destek, Microsoft.Compute API’sinin [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) veya üstü sürümlere eklenmiştir.

Ölçek kümeleri için bağlı disk desteğinin ilk uygulamasında, veri disklerini ölçek kümesindeki VM’lere bağlama/VM’lerden ayırma işlemini her VM için ayrı ayrı gerçekleştiremezsiniz.

Ölçek kümelerindeki bağlı veri diskleri için Azure portalı desteği başlangıçta sınırlıydı. Gereksinimlerinize bağlı olarak, bağlı diskleri yönetmek için Azure şablonları, CLI, PowerShell, SDK’lar ve REST API kullanabilirsiniz.


