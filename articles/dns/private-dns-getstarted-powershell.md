---
title: "PowerShell ile Azure DNS özel bölgelerini kullanmaya başlama | Microsoft Docs"
description: "Azure DNS'te özel DNS bölgesi ve kaydı oluşturma hakkında bilgi edinin. Bu kılavuzda, PowerShell kullanarak ilk özel DNS bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım talimatlar sunulmaktadır."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2017
ms.author: kumud
ms.openlocfilehash: d71e2391b6415b2403447479dea4fd0a3b818ed0
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2017
---
# <a name="get-started-with-azure-dns-private-zones-using-powershell"></a>PowerShell ile Azure DNS özel bölgelerini kullanmaya başlama

Bu makalede, Azure PowerShell kullanarak ilk özel DNS bölgesi ve kaydınızı oluşturma adımları gösterilmektedir.

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Sanal ağınızda özel bir DNS bölgesi yayımlamak için bölge içindeki kaynakları çözümleme izni olan sanal ağların listesini belirtmeniz gerekir.  Bu ağlara "çözümleme ağları" adı verilir.  Bir VM oluşturulduğunda, IP değiştirdiğinde veya yok edildiğinde Azure DNS'te ana bilgisayar adı kayıtlarının tutulacağı bir sanal makine kümesi de belirtebilirsiniz.  Bu ağlara "kayıt ağları" adı verilir.

Bu özellik şu anda yönetilen önizleme sürümünde olduğundan önizleme sürümünde bir PowerShell modülü sağlanır.

[!INCLUDE [private-dns-preview-notice](../../includes/private-dns-preview-notice.md)]

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

DNS bölgesini oluşturmadan önce, DNS bölgesini içerecek bir kaynak grubu oluşturulur. Aşağıda, komut gösterilmektedir.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `New-AzureRmDnsZone` cmdlet’i kullanılarak oluşturulur. Aşağıdaki örnek *MyResourceGroup* adlı kaynak grubunda *contoso.local* adlı bir DNS bölgesi oluşturur ve DNS bölgesini *MyAzureVnet* adlı sanal ağda kullanılabilir hale getirir. Değerleri kendinizinkilerle değiştirerek DNS bölgesini oluşturmak için örneği kullanın.

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

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Bu makalede oluşturulan tüm kaynakları silmek için, aşağıdaki adımları izleyin:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Özel DNS bölgeleri hakkında daha fazla bilgi için bkz. [Özel etki alanları için Azure DNS'i kullanma](private-dns-overview.md).

Azure DNS’te DNS kayıtlarını yönetme hakkında daha fazla bilgi için bkz. [PowerShell ile Azure DNS’te DNS kayıtlarını yönetme](dns-operations-recordsets.md).

