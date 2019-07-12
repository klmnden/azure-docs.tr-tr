---
title: Azure'da Windows VM kaynak etiketleme | Microsoft Docs
description: Azure Resource Manager dağıtım modeli kullanılarak oluşturulan bir Windows sanal makinenin etiketleme hakkında bilgi edinin
services: virtual-machines-windows
documentationcenter: ''
author: mmccrory
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: 56d17f45-e4a7-4d84-8022-b40334ae49d2
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror
ms.openlocfilehash: 26ee777f7db05ca1850e2a01c1716810624906c0
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709846"
---
# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Azure'da Windows sanal makine etiketleme
Bu makalede Resource Manager dağıtım modeliyle Azure'da Windows sanal makine etiketleme için farklı yollar açıklanmaktadır. Etiketler doğrudan bir kaynak veya kaynak grubu üzerinde yerleştirilen kullanıcı tanımlı anahtar/değer çiftleridir. Azure, şu anda kaynak ve kaynak grubu başına en fazla 15 etiket destekler. Etiket oluşturma sırasında bir kaynakta yerleştirilen veya var olan bir kaynağı eklendi. Etiketleri yalnızca Resource Manager dağıtım modeliyle oluşturulan kaynakları için desteklendiğini unutmayın. Linux sanal makinesi etiketlemek istiyorsanız, bkz. [Azure'da bir Linux sanal makinesi etiketleme](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>PowerShell ile etiketleme
Oluşturulacak, ekleyin ve ayarlamak için gerekir, PowerShell aracılığıyla etiketleri silme, [PowerShell ortamında Azure Resource Manager ile][PowerShell environment with Azure Resource Manager]. Kurulumu tamamladıktan sonra işlem, ağ ve depolama kaynakları oluşturma veya PowerShell kaynak oluşturulduktan sonra etiketleri yerleştirebilirsiniz. Bu makalede, sanal makineler üzerinde yerleştirilen görüntüleme veya düzenleme etiketleri hakkında odaklanacaktır.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

İlk olarak, sanal makinesine gidin `Get-AzVM` cmdlet'i.

        PS C:\> Get-AzVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Sanal makinenizi etiketleri içeriyorsa, ardından tüm etiketleri kaynağınızda görürsünüz:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

PowerShell üzerinden etiket eklemek istiyorsanız, kullanabileceğiniz `Set-AzResource` komutu. PowerShell aracılığıyla etiketler güncelleştirilirken bir bütün olarak etiketler güncelleştirilir unutmayın. Bu nedenle etiketler zaten olan bir kaynak için bir etiket ekliyorsanız, kaynakta yerleştirilmesini istediğiniz tüm etiketleri dahil etmek gerekir. Bir kaynağa PowerShell cmdlet'leri aracılığıyla ek etiketler ekleme örneği aşağıdadır.

Bu ilk cmdlet'i tüm yerleştirildiği etiketleri ayarlar *MyTestVM* için *$tags* değişken kullanarak `Get-AzResource` ve `Tags` özelliği.

        PS C:\> $tags = (Get-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

İkinci komut, belirtilen değişkeni için etiketleri görüntüler.

```
    PS C:\> $tags
    
    Key           Value
    ----          -----
    Department    MyDepartment
    Application   MyApp1
    Created By    MyName
    Environment   Production
```

Üçüncü komut ek bir etikete ekler *$tags* değişkeni. Kullanımına dikkat edin **+=** yeni bir anahtar/değer çifti için eklenecek *$tags* listesi.

        PS C:\> $tags += @{Location="MyLocation"}

Dördüncü komut içinde tanımlanan tüm ayarlar *$tags* belirtilen kaynak için değişken. Bu durumda, MyTestVM olur.

        PS C:\> Set-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Beşinci komut tüm etiketleri kaynakta görüntüler. Gördüğünüz gibi *konumu* artık içeren bir etiketi olarak tanımlanan *double* değeri.

```
    PS C:\> (Get-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

    Key           Value
    ----          -----
    Department    MyDepartment
    Application   MyApp1
    Created By    MyName
    Environment   Production
    Location      MyLocation
```

PowerShell ile etiketleme hakkında daha fazla bilgi edinmek için kullanıma [Azure kaynak cmdlet'leri][Azure Resource Cmdlets].

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Azure kaynaklarınızı etiketleme hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış][Azure Resource Manager Overview] and [Using Tags to organize your Azure Resources][Using Tags to organize your Azure Resources].
* Etiketler, Azure kaynaklarının kullanımını yönetmenize nasıl yardımcı olabileceğini görmek için bkz. [Azure faturanızı anlama][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/manage-resources-powershell.md
[Azure Resource Cmdlets]: https://docs.microsoft.com/powershell/module/az.resources/
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
