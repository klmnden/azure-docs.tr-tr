---
title: "Azure DNS'de - PowerShell DNS bölgelerini yönetme | Microsoft Docs"
description: "DNS bölgelerini Azure Powershell kullanarak yönetebilirsiniz. Bu makalede, güncelleştirme, silme ve DNS bölgelerini Azure DNS üzerinde oluşturmak açıklar"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: a67992ab-8166-4052-9b28-554c5a39e60c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2016
ms.author: gwallace
ms.openlocfilehash: 3f28e70bb6ef46f53375d256a520db40fcb71ad0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-manage-dns-zones-using-powershell"></a>PowerShell kullanarak DNS bölgelerini yönetme

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Bu makalede, Azure PowerShell kullanarak DNS bölgelerini yönetme gösterilmektedir. Platformlar arası kullanarak DNS bölgelerini yönetebilmeniz için [Azure CLI](dns-operations-dnszones-cli.md) ya da Azure portal.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `New-AzureRmDnsZone` cmdlet’i kullanılarak oluşturulur.

Aşağıdaki örnek adlı bir DNS bölgesi oluşturur *contoso.com* kaynak grubunda *MyResourceGroup*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

Aşağıdaki örnek bir DNS bölgesi iki oluşturulacağını gösterir [Azure Resource Manager etiketlerine](dns-zones-records.md#tags), *project = demo* ve *env = test*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

Azure DNS şimdi özel DNS bölgelerini (şu anda bir önizleme özelliği) da destekler.  Özel bir DNS bölgesi oluşturma örneği için bkz: [PowerShell kullanarak Azure DNS özel bölgeleriyle başlamak](./private-dns-getstarted-powershell.md).

## <a name="get-a-dns-zone"></a>Bir DNS bölgesini alın

Bir DNS bölgesi almak kullanın `Get-AzureRmDnsZone` cmdlet'i. Bu işlem, Azure DNS'de mevcut bir bölgeyi karşılık gelen bir DNS bölgesi nesnesi döndürür. Nesne bölge (örneğin, kayıt kümesi sayısı) hakkında veri içerir, ancak kayıt kümelerini içermiyor (bkz: `Get-AzureRmDnsRecordSet`).

```powershell
Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-8ec2-f4879750d201
Tags                  : {project, env}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org.,
                        ns4-01.azure-dns.info.}
NumberOfRecordSets    : 2
MaxNumberOfRecordSets : 5000
```

## <a name="list-dns-zones"></a>Liste DNS bölgeleri

Bölge adı atlama tarafından `Get-AzureRmDnsZone`, bir kaynak grubundaki tüm bölgeler sıralayabilirsiniz. Bu işlem, bölge nesneleri içeren bir dizi döndürür.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

Bölge adı ve kaynak grubu adı atlama tarafından `Get-AzureRmDnsZone`, Azure aboneliğindeki tüm bölgeler sıralayabilirsiniz.

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a>Bir DNS bölgesini güncelleştirme

Kullanarak bir DNS bölgesi kaynak değişiklikler yapılabilir `Set-AzureRmDnsZone`. Bu cmdlet herhangi bir bölgede DNS kayıt kümelerini güncelleştirmez (bkz [yönetmek DNS kayıtlarını nasıl](dns-operations-recordsets.md)). Yalnızca, bölge kaynak özelliklerini güncelleştirmek için kullanılır. Yazılabilir bölge özellikleri şu anda sınırlı [Azure Resource Manager 'etiketleri' bölge kaynak için](dns-zones-records.md#tags).

Bir DNS bölgesi güncelleştirmek için aşağıdaki iki yöntemden birini kullanın:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Bölge adını ve kaynak grubu kullanarak bölgesi belirtin

Bu yaklaşım varolan bölge etiketleri belirtilen değerlerle değiştirir.

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-the-zone-using-a-zone-object"></a>Bir $zone nesnesi kullanılarak dilimini belirtin

Bu yaklaşım, bölge nesnelerinden alır, etiketleri değiştirir ve değişiklikleri kaydeder. Bu şekilde, varolan etiketleri korunabilir.

```powershell
# Get the zone object
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

# Remove an existing tag
$zone.Tags.Remove("project")

# Add a new tag
$zone.Tags.Add("status","approved")

# Commit changes
Set-AzureRmDnsZone -Zone $zone
```

Kullanırken `Set-AzureRmDnsZone` $zone nesnesiyle [Etag denetler](dns-zones-records.md#etags) eşzamanlı değişiklikleri yazılmaz sağlamak için kullanılır. Kullanabileceğiniz isteğe bağlı `-Overwrite` bu denetimleri gizlemek için anahtar.

## <a name="delete-a-dns-zone"></a>Bir DNS bölgesi Sil

DNS bölgeleri kullanılarak silinebilir `Remove-AzureRmDnsZone` cmdlet'i.

> [!NOTE]
> Bir DNS bölgesi silme bölgedeki tüm DNS kayıtlarını siler. Bu işlem geri alınamaz. DNS bölgesi kullanımdaysa, bölge silindiğinde bölge kullanarak Hizmetleri başarısız olur.
>
>Yanlışlıkla bölge silinmeye karşı korumak için bkz: [DNS bölgeleri ve kayıtları korumak nasıl](dns-protect-zones-recordsets.md).


Bir DNS bölgesini silmek için aşağıdaki iki yöntemden birini kullanın:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Kaynak grubu adı ve bölge adını kullanarak bölgesi belirtin

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-the-zone-using-a-zone-object"></a>Bir $zone nesnesi kullanılarak dilimini belirtin

Kullanarak silinmesi bölgesine belirtebilirsiniz bir `$zone` tarafından döndürülen nesne `Get-AzureRmDnsZone`.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

Bölge nesnesini parametre olarak geçirilen yerine de yöneltilen:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

İle `Set-AzureRmDnsZone`, bölge kullanarak belirten bir `$zone` nesne eşzamanlı değişiklikleri silinmez emin olmak Etag denetimleri sağlar. Kullanım `-Overwrite` bu denetimleri gizlemek için anahtar.

## <a name="confirmation-prompts"></a>Onay istekleri

`New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, Ve `Remove-AzureRmDnsZone` cmdlet'leri tüm destek onay ister.

Her ikisi de `New-AzureRmDnsZone` ve `Set-AzureRmDnsZone` için onay iste `$ConfirmPreference` PowerShell tercih değişkeni değerine sahip `Medium` veya daha düşük. Bir DNS bölgesi silme olasılığı yüksek etkisi nedeniyle `Remove-AzureRmDnsZone` cmdlet, onay ister `$ConfirmPreference` PowerShell değişkeni sahip herhangi bir değer dışındaki `None`.

İçin varsayılan değer itibaren `$ConfirmPreference` olan `High`, yalnızca `Remove-AzureRmDnsZone` varsayılan olarak onaylamanız istenir.

Geçerli kılabilirsiniz `$ConfirmPreference` kullanarak ayarlama `-Confirm` parametresi. Belirtirseniz `-Confirm` veya `-Confirm:$True` , cmdlet sizden onay kendisinden önce çalışır ister. Belirtirseniz `-Confirm:$False` , cmdlet onaylamanız istenmez.

Hakkında daha fazla bilgi için `-Confirm` ve `$ConfirmPreference`, bkz: [tercih değişkenleri hakkında](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [kayıt kümelerini ve kayıtları yönetmek](dns-operations-recordsets.md) DNS bölgesinde.
<br>
Bilgi edinmek için nasıl [etki alanınızı Azure DNS'ye temsilci](dns-domain-delegation.md).
<br>
Gözden geçirme [Azure DNS PowerShell başvuru belgeleri](/powershell/module/azurerm.dns).

