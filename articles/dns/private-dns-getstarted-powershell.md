---
title: PowerShell ile Azure DNS özel bölgelerini kullanmaya başlama | Microsoft Docs
description: Azure DNS'te özel DNS bölgesi ve kaydı oluşturma hakkında bilgi edinin. Bu kılavuzda, PowerShell kullanarak ilk özel DNS bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım talimatlar sunulmaktadır.
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2017
ms.author: kumud
ms.openlocfilehash: d9e5c9b8caa5349fbcda9b71f7524fb9e99b976c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="get-started-with-azure-dns-private-zones-using-powershell"></a>PowerShell ile Azure DNS özel bölgelerini kullanmaya başlama

Bu makalede, Azure PowerShell kullanarak ilk özel DNS bölgesi ve kaydınızı oluşturma adımları gösterilmektedir.

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Sanal ağınızda özel bir DNS bölgesi yayımlamak için bölge içindeki kaynakları çözümleme izni olan sanal ağların listesini belirtmeniz gerekir.  Buna 'çözümleme sanal ağları' adı verilir.  Bir VM oluşturulduğunda, IP’yi değiştirdiğinde veya yok edildiğinde Azure DNS'te ana bilgisayar adı kayıtlarının tutulacağı bir sanal ağ da belirtebilirsiniz.  Buna 'kayıt sanal ağı' adı verilir.

# <a name="get-the-preview-powershell-modules"></a>Önizleme PowerShell modüllerini alma
Bu yönergelerde, Özel Bölge özelliği için gerekli modüllerinizin olduğu ve Azure PowerShell’i önceden yükleyip bunda oturum açmış olduğunuz varsayılır. 

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

DNS bölgesini oluşturmadan önce, DNS bölgesini içerecek bir kaynak grubu oluşturulur. Aşağıdaki örnekte komut gösterilmektedir.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-private-zone"></a>DNS özel bölgesi oluşturma

ZoneType parametresi için "Özel" değeriyle birlikte `New-AzureRmDnsZone` cmdlet’i kullanılarak bir DNS bölgesi oluşturulur. Aşağıdaki örnek *MyResourceGroup* adlı kaynak grubunda *contoso.local* adlı bir DNS bölgesi oluşturur ve DNS bölgesini *MyAzureVnet* adlı sanal ağda kullanılabilir hale getirir. Değerleri kendinizinkilerle değiştirerek DNS bölgesini oluşturmak için örneği kullanın.

ZoneType parametresi atılırsa Bölge bir Genel bölge olarak oluşturulur; bu nedenle bir Özel Bölge oluşturmanız gerekiyorsa bu gereklidir. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyAzureVnet -ResourceGroupName VnetResourceGroup
New-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -ZoneType Private -ResolutionVirtualNetworkId @($vnet.Id)
```

Azure'un bölgedeki ana bilgisayar adı kayıtlarını otomatik olarak oluşturmasını istiyorsanız *ResolutionVirtualNetworkId* yerine *RegistrationVirtualNetworkId* parametresini kullanın.  Kayıt sanal ağlarında çözümleme otomatik olarak etkinleştirilir.

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyAzureVnet -ResourceGroupName VnetResourceGroup
New-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -ZoneType Private -RegistrationVirtualNetworkId @($vnet.Id)
```

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

`New-AzureRmDnsRecordSet` cmdlet’ini kullanarak kayıt kümeleri oluşturabilirsiniz. Aşağıdaki örnekte, "MyResourceGroup" kaynak grubu içindeki "contoso.local" DNS Bölgesinde göreli adı "db" olan bir kaynak oluşturulmaktadır. "db.contoso.local", kayıt kümesinin tam adıdır. Kayıt türü "A", IP adresi "10.0.0.4" ve TTL 3600 saniyedir.

```powershell
New-AzureRmDnsRecordSet -Name db -RecordType A -ZoneName contoso.local -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "10.0.0.4")
```

Diğer kayıt türleri, birden fazla kayıt içerek kayıt kümeleri ve var olan kayıtların değiştirilmesi hakkında bilgi için bkz. [Azure PowerShell kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets.md). 

## <a name="view-records"></a>Kayıtları görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şu seçenekleri kullanın:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.local -ResourceGroupName MyResourceGroup
```

# <a name="list-dns-private-zones"></a>DNS özel bölgelerini listeleme

`Get-AzureRmDnsZone` içinden bölge adını atarak bir kaynak grubundaki tüm bölgeleri numaralandırabilirsiniz. Bu işlem, bölge nesnelerinin bir dizisini döndürür.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

`Get-AzureRmDnsZone` içinden hem bölge adını hem de kaynak grubunu atarak, Azure aboneliğindeki tüm bölgeleri numaralandırabilirsiniz.

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-private-zone"></a>DNS özel bölgesini güncelleştirme

`Set-AzureRmDnsZone` kullanılarak bir DNS bölgesi kaynağı üzerinde değişiklikler yapılabilir. Bu cmdlet, bölge içindeki DNS kayıt kümelerinin herhangi birini güncelleştirmez (bkz. [DNS kayıtlarını yönetme](dns-operations-recordsets.md)). Yalnızca bölge kaynağının özelliklerini güncelleştirmek için kullanılır. Yazılabilir bölge özellikleri şu anda [bölge kaynağı için Azure Resource Manager ‘etiketleri’](dns-zones-records.md#tags) ve Özel Bölgeler için 'RegistrationVirtualNetworkId' ve 'ResolutionVirtualNetworkId' parametreleri ile sınırlıdır.

Aşağıdaki örnek, bir bölgeyle bağlantılı Kayıt Sanal Ağını, yeni bir MyNewAzureVnet ile değiştirir.

Oluşturma işleminin tersine, güncelleştirme için ZoneType parametresini belirtmemeniz gerektiğini lütfen unutmayın. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyNewAzureVnet -ResourceGroupName MyResourceGroup
Set-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -RegistrationVirtualNetworkId @($vnet.Id)
```

Aşağıdaki örnek, bir bölgeyle bağlantılı Çözümleme Sanal Ağını, "MyNewAzureVnet" adlı yeni biriyle değiştirir.

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyNewAzureVnet -ResourceGroupName MyResourceGroup
Set-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -ResolutionVirtualNetworkId @($vnet.Id)
```

## <a name="delete-a-dns-private-zone"></a>DNS özel bölgesini silme

Genel bölgeler gibi, DNS özel bölgeleri de `Remove-AzureRmDnsZone` cmdlet’i kullanılarak silinebilir.

> [!NOTE]
> Bir DNS bölgesi silindiğinde, bölge içindeki tüm DNS kayıtları da silinir. Bu işlem geri alınamaz. DNS bölgesi kullanımdaysa, bölge silindiğinde bölgeyi kullanan hizmetler başarısız olur.
>
>Bölgenin yanlışlıkla silinmesine karşı koruma oluşturmak için bkz. [DNS bölgelerini ve kayıtlarını koruma](dns-protect-zones-recordsets.md).

DNS bölgesini silmek için aşağıdaki iki yöntemden birini uygulayın:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Bölge adını ve kaynak grubu adını kullanarak bölgeyi belirtin

```powershell
Remove-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-the-zone-using-a-zone-object"></a>$zone nesnesini kullanarak bölgeyi belirtin

`Get-AzureRmDnsZone` tarafından döndürülen bir `$zone` nesnesini kullanarak silinecek bölgeyi belirtebilirsiniz.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

Bölge nesnesi, parametre olarak geçirilmek yerine yöneltilebilir:

```powershell
Get-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup | Remove-AzureRmDnsZone

```

## <a name="confirmation-prompts"></a>Onay istemleri

`New-AzureRmDnsZone`, `Set-AzureRmDnsZone` ve `Remove-AzureRmDnsZone` cmdlet’lerinin tamamı onay istemlerini destekler.

`$ConfirmPreference` PowerShell tercih değişkeni, `Medium` veya daha küçük bir değere sahipse hem `New-AzureRmDnsZone` hem de `Set-AzureRmDnsZone` onay istemi görüntüler. DNS bölgesini silmenin olası kritik etkisi nedeniyle, `$ConfirmPreference` PowerShell değişkeninin `None` dışında bir değeri varsa `Remove-AzureRmDnsZone` cmdlet’i, onay istemi görüntüler.

`$ConfirmPreference` için varsayılan değer `High` olduğundan yalnızca `Remove-AzureRmDnsZone` varsayılan olarak onay istemi görüntüler.

`-Confirm` parametresini kullanarak geçerli `$ConfirmPreference` ayarını geçersiz kılabilirsiniz. `-Confirm` veya `-Confirm:$True` belirtirseniz cmdlet, çalıştırılmadan önce size onay istemi görüntüler. `-Confirm:$False` belirtirseniz cmdlet size onay istemi görüntülemez.

`-Confirm` ve `$ConfirmPreference` hakkında daha fazla bilgi için bkz. [Tercih Değişkenleri Hakkında](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).


## <a name="delete-all-resources"></a>Tüm kaynakları silme

Bu makalede oluşturulan tüm kaynakları silmek için, aşağıdaki adımları izleyin:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Özel DNS bölgeleri hakkında daha fazla bilgi için bkz. [Özel etki alanları için Azure DNS'i kullanma](private-dns-overview.md).

Azure DNS’te Özel Bölgeler ile gerçekleştirilebilecek bazı genel senaryolara ([Özel Bölge senaryoları](./private-dns-scenarios.md)) göz atın.

Azure DNS’te DNS kayıtlarını yönetme hakkında daha fazla bilgi için bkz. [PowerShell ile Azure DNS’te DNS kayıtlarını yönetme](dns-operations-recordsets.md).

