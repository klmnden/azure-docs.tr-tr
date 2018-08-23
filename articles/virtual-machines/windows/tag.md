---
title: Azure'da Windows VM kaynak etiketleme | Microsoft Docs
description: Azure Resource Manager dağıtım modeli kullanılarak oluşturulan bir Windows sanal makinenin etiketleme hakkında bilgi edinin
services: virtual-machines-windows
documentationcenter: ''
author: mmccrory
manager: jeconnoc
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
ms.openlocfilehash: 6c461fe06e1a869d0495551ab014452c03dc60b2
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42057607"
---
# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Azure'da Windows sanal makine etiketleme
Bu makalede Resource Manager dağıtım modeliyle Azure'da Windows sanal makine etiketleme için farklı yollar açıklanmaktadır. Etiketler doğrudan bir kaynak veya kaynak grubu üzerinde yerleştirilen kullanıcı tanımlı anahtar/değer çiftleridir. Azure, şu anda kaynak ve kaynak grubu başına en fazla 15 etiket destekler. Etiket oluşturma sırasında bir kaynakta yerleştirilen veya var olan bir kaynağı eklendi. Etiketleri yalnızca Resource Manager dağıtım modeliyle oluşturulan kaynakları için desteklendiğini unutmayın. Linux sanal makinesi etiketlemek istiyorsanız, bkz. [Azure'da bir Linux sanal makinesi etiketleme](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>PowerShell ile etiketleme
Oluşturulacak, ekleyin ve ayarlamak için gerekir, PowerShell aracılığıyla etiketleri silme, [PowerShell ortamında Azure Resource Manager ile][PowerShell environment with Azure Resource Manager]. Kurulumu tamamladıktan sonra işlem, ağ ve depolama kaynakları oluşturma veya PowerShell kaynak oluşturulduktan sonra etiketleri yerleştirebilirsiniz. Bu makalede, sanal makineler üzerinde yerleştirilen görüntüleme veya düzenleme etiketleri hakkında odaklanacaktır.

İlk olarak, sanal makinesine gidin `Get-AzureRmVM` cmdlet'i.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Sanal makinenizi etiketleri içeriyorsa, ardından tüm etiketleri kaynağınızda görürsünüz:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

PowerShell üzerinden etiket eklemek istiyorsanız, kullanabileceğiniz `Set-AzureRmResource` komutu. PowerShell aracılığıyla etiketler güncelleştirilirken bir bütün olarak etiketler güncelleştirilir unutmayın. Bu nedenle etiketler zaten olan bir kaynak için bir etiket ekliyorsanız, kaynakta yerleştirilmesini istediğiniz tüm etiketleri dahil etmek gerekir. Bir kaynağa PowerShell cmdlet'leri aracılığıyla ek etiketler ekleme örneği aşağıdadır.

Bu ilk cmdlet'i tüm yerleştirildiği etiketleri ayarlar *MyTestVM* için *$tags* değişken kullanarak `Get-AzureRmResource` ve `Tags` özelliği.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

İkinci komut, belirtilen değişkeni için etiketleri görüntüler.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment

Üçüncü komut ek bir etikete ekler *$tags* değişkeni. Kullanımına dikkat edin **+=** yeni bir anahtar/değer çifti için eklenecek *$tags* listesi.

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

Dördüncü komut içinde tanımlanan tüm ayarlar *$tags* belirtilen kaynak için değişken. Bu durumda, MyTestVM olur.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Beşinci komut tüm etiketleri kaynakta görüntüler. Gördüğünüz gibi *konumu* artık içeren bir etiketi olarak tanımlanan *double* değeri.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment
        Value        MyLocation
        Name        Location

PowerShell ile etiketleme hakkında daha fazla bilgi edinmek için kullanıma [Azure kaynak cmdlet'leri][Azure Resource Cmdlets].

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Azure kaynaklarınızı etiketleme hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış] [ Azure Resource Manager Overview] ve [kullanarak Azure kaynaklarınızı düzenlemek için etiketleri] [ Using Tags to organize your Azure Resources].
* Etiketler, Azure kaynaklarının kullanımını yönetmenize nasıl yardımcı olabileceğini görmek için bkz. [Azure faturanızı anlama] [ Understanding your Azure Bill] ve [Microsoft Azure kaynak tüketiminize öngörü] [Gain insights into your Microsoft Azure resource consumption].

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.resources/
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
