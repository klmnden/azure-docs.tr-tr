---
title: Azure DNS'de - PowerShell DNS bölgelerini yönetme | Microsoft Docs
description: DNS bölgelerini Azure Powershell kullanarak yönetebilirsiniz. Bu makalede, güncelleştirme, silme ve DNS bölgelerini Azure DNS üzerinde oluşturmak açıklar
services: dns
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: a67992ab-8166-4052-9b28-554c5a39e60c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/19/2018
ms.author: kumud
ms.openlocfilehash: e7b0bc32d3fa8fbcf73298b6988655fca7cfa793
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="how-to-manage-dns-zones-using-powershell"></a>PowerShell kullanarak DNS bölgelerini yönetme

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Bu makalede, Azure PowerShell kullanarak DNS bölgelerini yönetme gösterilmektedir. Platformlar arası kullanarak DNS bölgelerini yönetebilmeniz için [Azure CLI](dns-operations-dnszones-cli.md) ya da Azure portal.

Bu kılavuz, özellikle ortak DNS bölgeleri ile ilgilidir. Azure DNS'de özel bölgeler yönetmek için Azure PowerShell kullanma hakkında daha fazla bilgi için bkz: [Azure PowerShell kullanarak Azure DNS özel bölgeler ile çalışmaya başlama](private-dns-getstarted-powershell.md).

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

Azure DNS artık özel DNS bölgelerini de destekler (şu anda genel önizlemede).  Özel DNS bölgeleri hakkında daha fazla bilgi için bkz. [Özel etki alanları için Azure DNS'i kullanma](private-dns-overview.md). Özel bir DNS bölgesi oluşturma örneği için bkz. [PowerShell kullanarak Azure DNS özel bölgeleriyle çalışmaya başlama](./private-dns-getstarted-powershell.md).

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

## <a name="list-dns-zones"></a>DNS bölgelerini listeleme

`Get-AzureRmDnsZone` içinden bölge adını atarak bir kaynak grubundaki tüm bölgeleri numaralandırabilirsiniz. Bu işlem, bölge nesnelerinin bir dizisini döndürür.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

`Get-AzureRmDnsZone` içinden hem bölge adını hem de kaynak grubunu atarak, Azure aboneliğindeki tüm bölgeleri numaralandırabilirsiniz.

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a>DNS bölgesini güncelleştirme

`Set-AzureRmDnsZone` kullanılarak bir DNS bölgesi kaynağı üzerinde değişiklikler yapılabilir. Bu cmdlet, bölge içindeki DNS kayıt kümelerinin herhangi birini güncelleştirmez (bkz. [DNS kayıtlarını yönetme](dns-operations-recordsets.md)). Yalnızca bölge kaynağının özelliklerini güncelleştirmek için kullanılır. Yazılabilir bölge özellikleri şu anda sınırlı [Azure Resource Manager 'etiketleri' bölge kaynak için](dns-zones-records.md#tags).

Bir DNS bölgesi güncelleştirmek için aşağıdaki iki yöntemden birini kullanın:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Bölge adını ve kaynak grubu kullanarak bölgesi belirtin

Bu yaklaşım varolan bölge etiketleri belirtilen değerlerle değiştirir.

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-the-zone-using-a-zone-object"></a>$zone nesnesini kullanarak bölgeyi belirtin

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
> Bir DNS bölgesi silindiğinde, bölge içindeki tüm DNS kayıtları da silinir. Bu işlem geri alınamaz. DNS bölgesi kullanımdaysa, bölge silindiğinde bölgeyi kullanan hizmetler başarısız olur.
>
>Bölgenin yanlışlıkla silinmesine karşı koruma oluşturmak için bkz. [DNS bölgelerini ve kayıtlarını koruma](dns-protect-zones-recordsets.md).


DNS bölgesini silmek için aşağıdaki iki yöntemden birini uygulayın:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Bölge adını ve kaynak grubu adını kullanarak bölgeyi belirtin

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-the-zone-using-a-zone-object"></a>$zone nesnesini kullanarak bölgeyi belirtin

`Get-AzureRmDnsZone` tarafından döndürülen bir `$zone` nesnesini kullanarak silinecek bölgeyi belirtebilirsiniz.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

Bölge nesnesi, parametre olarak geçirilmek yerine yöneltilebilir:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

İle `Set-AzureRmDnsZone`, bölge kullanarak belirten bir `$zone` nesne eşzamanlı değişiklikleri silinmez emin olmak Etag denetimleri sağlar. Kullanım `-Overwrite` bu denetimleri gizlemek için anahtar.

## <a name="confirmation-prompts"></a>Onay istemleri

`New-AzureRmDnsZone`, `Set-AzureRmDnsZone` ve `Remove-AzureRmDnsZone` cmdlet’lerinin tamamı onay istemlerini destekler.

`$ConfirmPreference` PowerShell tercih değişkeni, `Medium` veya daha küçük bir değere sahipse hem `New-AzureRmDnsZone` hem de `Set-AzureRmDnsZone` onay istemi görüntüler. DNS bölgesini silmenin olası kritik etkisi nedeniyle, `$ConfirmPreference` PowerShell değişkeninin `None` dışında bir değeri varsa `Remove-AzureRmDnsZone` cmdlet’i, onay istemi görüntüler.

`$ConfirmPreference` için varsayılan değer `High` olduğundan yalnızca `Remove-AzureRmDnsZone` varsayılan olarak onay istemi görüntüler.

`-Confirm` parametresini kullanarak geçerli `$ConfirmPreference` ayarını geçersiz kılabilirsiniz. `-Confirm` veya `-Confirm:$True` belirtirseniz cmdlet, çalıştırılmadan önce size onay istemi görüntüler. `-Confirm:$False` belirtirseniz cmdlet size onay istemi görüntülemez.

`-Confirm` ve `$ConfirmPreference` hakkında daha fazla bilgi için bkz. [Tercih Değişkenleri Hakkında](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [kayıt kümelerini ve kayıtları yönetmek](dns-operations-recordsets.md) DNS bölgesinde.
<br>
Bilgi edinmek için nasıl [etki alanınızı Azure DNS'ye temsilci](dns-domain-delegation.md).
<br>
Gözden geçirme [Azure DNS PowerShell başvuru belgeleri](/powershell/module/azurerm.dns).

