---
title: "Azure üzerinde bir Windows VM kaynak etiketlemek nasıl | Microsoft Docs"
description: "Azure Resource Manager dağıtım modeli kullanılarak oluşturulmuş bir Windows sanal makinenin etiketleme hakkında bilgi edinin"
services: virtual-machines-windows
documentationcenter: 
author: mmccrory
manager: timlt
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
ms.openlocfilehash: 5f00c4265cea3db02dbb09a7f81be636a3fdd3d1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Azure'da Windows sanal makine etiketlemek nasıl
Bu makalede Resource Manager dağıtım modeli aracılığıyla Azure'da Windows sanal makine etiketlemek için farklı yollar açıklanmaktadır. Etiketler doğrudan bir kaynağa veya bir kaynak grubu yerleştirilen kullanıcı tanımlı anahtar/değer çiftleridir. Azure şu anda kaynak ve kaynak grubu başına en fazla 15 etiketlerini destekler. Etiketler oluşturma sırasında bir kaynağa yerleştirilmiş veya mevcut bir kaynağı eklendi. Etiketler Resource Manager dağıtım modeli yalnızca oluşturulan kaynaklar için desteklendiğini unutmayın. Linux sanal makine etiketi istiyorsanız, bkz: [Azure'da bir Linux sanal makine etiketlemek nasıl](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>PowerShell ile etiketleme
Oluşturmak için Ekle ve PowerShell, ayarlamak için ilk gerek aracılığıyla etiketleri silmek, [PowerShell ortam Azure Resource Manager ile][PowerShell environment with Azure Resource Manager]. Kurulum tamamlandığında, etiketler oluşturma veya kaynak PowerShell yoluyla oluşturulduktan sonra işlem, ağ ve depolama kaynakları yerleştirebilirsiniz. Bu makalede, görüntüleme ve düzenleme etiketleri sanal makinelerde yerleştirilen hakkında odaklanacaktır.

İlk olarak, bir sanal makineye üzerinden gidin `Get-AzureRmVM` cmdlet'i.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Sanal makineniz etiketleri içeriyorsa, tüm etiketleri, kaynakta sonra görürsünüz:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

PowerShell aracılığıyla etiketler eklemek istiyorsanız, kullanabileceğiniz `Set-AzureRmResource` komutu. PowerShell aracılığıyla etiketler güncelleştirilirken etiketleri bir bütün olarak güncelleştirilir unutmayın. Bu nedenle etiketleri zaten olan bir kaynağın bir etiket ekliyorsanız, kaynak yerleştirilmesini istediğiniz tüm etiketleri dahil etmeniz gerekir. Aşağıda, PowerShell cmdlet'leri aracılığıyla bir kaynağa ek etiketleri eklemeyi örneğidir.

Bu ilk cmdlet'i tüm getirilen etiketleri ayarlar *MyTestVM* için *$tags* değişken, kullanarak `Get-AzureRmResource` ve `Tags` özelliği.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

İkinci komut belirtilen değişkeni için etiketleri görüntüler.

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

Üçüncü komut ek bir etikete ekler *$tags* değişkeni. Kullanımına dikkat edin  **+=**  yeni anahtar/değer çifti eklemek için *$tags* listesi.

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

Dördüncü komut tüm tanımlanan etiketleri ayarlar *$tags* verilen kaynağa değişken. Bu durumda, MyTestVM olur.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Beşinci komut tüm etiketleri kaynaktaki görüntüler. Gördüğünüz gibi *konumu* şimdi bir etiketle olarak tanımlanan *MyLocation* değeri olarak.

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

PowerShell aracılığıyla etiketleme hakkında daha fazla bilgi için kullanıma [Azure kaynak cmdlet'leri][Azure Resource Cmdlets].

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Azure kaynaklarınızı etiketleme hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış] [ Azure Resource Manager Overview] ve [etiketleri kullanarak Azure kaynaklarınızı düzenleme][Using Tags to organize your Azure Resources].
* Etiketleri kullanımınızı Azure kaynaklarını yönetmek nasıl yardımcı olabileceğini görmek için bkz: [Azure faturanızı anlamak] [ Understanding your Azure Bill] ve [Microsoft Azure kaynak tüketimini Öngörüler elde][Gain insights into your Microsoft Azure resource consumption].

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
