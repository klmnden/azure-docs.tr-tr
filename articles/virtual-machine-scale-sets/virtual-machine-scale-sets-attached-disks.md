---
title: "Azure Sanal Makine Ölçek Kümeleri Bağlı Veri Diskleri | Microsoft Belgeleri"
description: "Sanal makine ölçek kümelerinde bağlı veri disklerinin nasıl kullanılacağını öğrenin"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: guybo
ms.openlocfilehash: 22c7e589efa9a9f401549ec9b95c58c4eaf07b94
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-vm-scale-sets-and-attached-data-disks"></a>Azure VM ölçek kümeleri ve bağlı veri diskleri
Azure [sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets/), bağlı veri diskleri olan sanal makineleri artık desteklemektedir. Veri diskleri, Azure Yönetilen Diskler ile oluşturulmuş olan ölçek kümeleri için depolama profilinde tanımlanabilir. Daha önce, ölçek kümelerindeki sanal makinelere doğrudan bağlı depolama seçeneği olarak yalnızca işletim sistemi sürücüsü ve geçici sürücüler kullanılabiliyordu.

> [!NOTE]
>  Bağlı veri diskleri tanımlanmış bir ölçek kümesi oluşturduğunuz durumlarda da tek başına Azure sanal makinelerinde olduğu gibi, diskleri kullanabilmek için bir VM içinde takmanız ve biçimlendirmeniz gerekir. Bunu yapmanın uygun bir yolu, bir VM üzerindeki tüm verileri bölümlemek ve biçimlendirmek için standart bir betik çağıran özel bir betik uzantısı kullanılmasıdır.

## <a name="create-a-scale-set-with-attached-data-disks"></a>Bağlı veri diskleri olan ölçek kümesi oluşturma
Bağlı diskleri olan bir ölçek kümesi oluşturmanın basit bir yolu, [Azure CLI](https://github.com/Azure/azure-cli) _vmss create_ komutunu kullanmaktır. Aşağıdaki örnekte bir Azure kaynak grubu ile birlikte, her birinin boyutu sırasıyla 50 GB ve 100 GB olan 2 bağlı veri diski içeren 10 Ubuntu sanal makinesiyle bir VM ölçek kümesi oluşturulmaktadır.
```bash
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```
Belirtmemeniz durumunda, _vmss create_ komutu bazı yapılandırma değerlerini varsayılan olarak kullanır. Geçersiz kılabileceğiniz seçenekleri görmek için şunları deneyin:
```bash
az vmss create --help
```
Bağlı diskleri olan bir ölçek kümesi oluşturmanın başka bir yolu ise bir Azure Resource Manager şablonunda ölçek kümesi tanımlamak, _storageProfile_ içine bir _dataDisks_ bölümü eklemek ve şablonu dağıtmaktır. Yukarıdaki 50 GB ve 100 GB disk örneği, şablonda şunun gibi tanımlanır:
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    }
]
```
Bağlı diski olan bir ölçek kümesi şablonunun dağıtıma hazır, tam bir örneğini şurada görebilirsiniz: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).

## <a name="adding-a-data-disk-to-an-existing-scale-set"></a>Mevcut ölçek kümesine veri diski ekleme
> [!NOTE]
>  Veri disklerini yalnızca [Azure Yönetilen Diskleri](./virtual-machine-scale-sets-managed-disks.md) ile oluşturulmuş olan bir ölçek kümesine ekleyebilirsiniz.

Azure CLI _az vmss disk attach_ komutunu kullanarak bir VM ölçek kümesine veri diski ekleyebilirsiniz. Henüz kullanımda olmayan bir mantıksal birim numarası belirttiğinizden emin olun. Aşağıdaki CLI örneği mantıksal birim numarası 3’e 50 GB sürücü ekler:
```bash
az vmss disk attach -g dsktest -n dskvmss --size-gb 50 --lun 3
```

Aşağıdaki PowerShell örneği mantıksal birim numarası 3’e 50 GB sürücü ekler:
```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myvmssrg -VMScaleSetName myvmss
$vmss = Add-AzureRmVmssDataDisk -VirtualMachineScaleSet $vmss -Lun 3 -Caching 'ReadWrite' -CreateOption Empty -DiskSizeGB 50 -StorageAccountType StandardLRS
Update-AzureRmVmss -ResourceGroupName myvmssrg -Name myvmss -VirtualMachineScaleSet $vmss
```

> [!NOTE]
> Farklı VM boyutları, destekledikleri bağlı sürücü sayısı ile ilgili farklı sınırlara sahiptir. Yeni bir disk eklemeden önce [sanal makine boyut özelliklerini](../virtual-machines/windows/sizes.md) denetleyin.

Ayrıca bir ölçek kümesi tanımının _storageProfile_ kısmındaki _dataDisks_ özelliğine yeni bir giriş ekleyip değişikliği uygulayarak da bir disk ekleyebilirsiniz. Bunu test etmek için [Azure Kaynak Gezgini](https://resources.azure.com/)’nde var olan bir ölçek kümesi tanımı bulun. _Düzenle_’yi seçin ve veri diskleri listesine yeni bir disk ekleyin. Örneğin yukarıdaki örneği kullanın:
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    },
    {
    "lun": 3,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 20
    }          
]
```

Ardından _PUT_ öğesini seçerek değişiklikleri ölçek kümenize uygulayın. Bu örnek, ikiden fazla bağlı veri diskini destekleyen bir VM boyutu kullandığınız sürece işe yarar.

> [!NOTE]
> Ölçek kümesi tanımında, veri diski ekleme veya kaldırma gibi bir değişiklik yaptığınızda değişiklik yeni oluşturulan tüm sanal makineler için geçerli olur. Ancak _upgradePolicy_ özelliği "Otomatik" olarak ayarlanırsa, değişiklik yalnızca mevcut sanal makinelere uygulanır. Bu özellik "El ile" olarak ayarlanırsa, yeni modeli mevcut sanal makinelere el ile uygulamanız gerekir. Bu işlemi portalda _Update-AzureRmVmssInstance_ PowerShell komutunu veya _az vmss update-instances_ CLI komutunu kullanarak yapabilirsiniz.

## <a name="adding-pre-populated-data-disks-to-an-existent-scale-set"></a>Önceden doldurulmuş veri disklerini mevcut bir ölçek kümesine ekleme 
> Tasarım gereği, mevcut ölçek kümesi modeline disk eklediğinizde, disk her zaman boş oluşturulur. Bu senaryo ayrıca ölçek kümesi tarafından oluşturulan yeni örnekleri içerir. Bu davranışın nedeni ölçek kümesinin boş veri diski tanımına sahip olmasıdır. Mevcut bir ölçek kümesi modeli için önceden doldurulmuş veri sürücüleri oluşturmak için iki seçenekten birini seçebilirsiniz:

* Örnek 0 VM’sinden verileri özel bir betik çalıştırarak diğer VM’lerdeki veri disklerine kopyalayabilirsiniz.
* İşletim sistemi diskine ve veri diskine (gerekli verilerle) sahip yönetilen bir görüntü oluşturup görüntüde yeni bir ölçek kümesi oluşturabilirsiniz. Bu şekilde oluşturulan her yeni VM, ölçek kümesi tanımında sağlanan bir veri diskine sahip olur. Bu tanım, özelleştirilmiş verilere sahip bir veri disk görüntüsüne başvuracağından, ölçek kümesindeki her sanal makine otomatik olarak bu değişikliklerle açılır.

> Özel bir görüntü oluşturmak için izlenecek yol şurada bulunabilir: [Azure’da genelleştirilmiş bir VM’nin yönetilen görüntüsü oluşturma](/azure/virtual-machines/windows/capture-image-resource/) 

> Kullanıcının gerekli verileri içeren örnek 0 VM’yi yakalaması, ardından da görüntü tanımı için bu VHD’yi kullanması gerekir.

## <a name="removing-a-data-disk-from-a-scale-set"></a>Bir ölçek kümesinden veri diski kaldırma
Azure CLI _az vmss disk detach_ komutunu kullanarak bir VM ölçek kümesinden veri diski kaldırabilirsiniz. Örneğin, aşağıdaki komut mantıksal birim numarası 2’de tanımlanmış diski kaldırır:
```bash
az vmss disk detach -g dsktest -n dskvmss --lun 2
```  
Benzer şekilde, _storageProfile_ içindeki _dataDisks_ özelliğinden bir girişi kaldırıp değişikliği uygulayarak da diski ölçek kümesinden kaldırabilirsiniz. 

## <a name="additional-notes"></a>Ek notlar
Azure Yönetilen diskleri ve ölçek kümesi bağlı veri diskleri için destek, Microsoft.Compute API’sinin [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) veya üstü sürümlere eklenmiştir.

Ölçek kümeleri için bağlı disk desteğinin ilk uygulamasında, veri disklerini ölçek kümesindeki VM’lere bağlama/VM’lerden ayırma işlemini her VM için ayrı ayrı gerçekleştiremezsiniz.

Ölçek kümelerindeki bağlı veri diskleri için Azure portalı desteği başlangıçta sınırlıydı. Gereksinimlerinize bağlı olarak, bağlı diskleri yönetmek için Azure şablonları, CLI, PowerShell, SDK’lar ve REST API kullanabilirsiniz.


