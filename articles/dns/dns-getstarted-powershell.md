---
title: PowerShell ile Azure DNS kullanmaya başlama | Microsoft Docs
description: Azure DNS'te DNS bölgesi ve kaydı oluşturma hakkında bilgi edinin. Bu kılavuzda, PowerShell kullanarak ilk DNS bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım talimatlar sunulmaktadır.
services: dns
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: kumud
ms.openlocfilehash: 050111f4a5e8459e89d049ccb879b5079ff68527
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30175333"
---
# <a name="get-started-with-azure-dns-using-powershell"></a>PowerShell ile Azure DNS’i kullanmaya başlama

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Bu makalede, Azure PowerShell kullanarak ilk DNS bölgesi ve kaydınızı oluşturma adımları gösterilmektedir. Ayrıca, Azure portal veya platformlar arası Azure CLI kullanarak aşağıdaki adımları gerçekleştirebilirsiniz. Azure DNS ayrıca özel etki alanları oluşturmayı destekler. İlk özel DNS bölgenizi ve kaydınızı oluşturma hakkında adım adım yönergeler için [PowerShell ile Azure DNS özel bölgelerini kullanmaya başlama](private-dns-getstarted-powershell.md) konusunu inceleyin.

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Son olarak, DNS bölgenizi Internet'te yayımlamak için etki alanının ad sunucularını yapılandırmanız gerekir. Bu adımların her biri aşağıda açıklanmıştır.

Bu yönergeler, Azure PowerShell’i zaten yüklediğinizi ve oturum açtığınızı varsayar. Yardım için bkz. [PowerShell ile DNS bölgelerini yönetme](dns-operations-dnszones.md).

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

DNS bölgesini oluşturmadan önce, DNS Bölgesi’ni içerecek bir kaynak grubu oluşturulur. Aşağıda, komut gösterilmektedir.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `New-AzureRmDnsZone` cmdlet’i kullanılarak oluşturulur. Aşağıdaki örnek, *MyResourceGroup* adlı kaynak grubunda *contoso.com* adlı bir DNS bölgesi oluşturur. Değerleri kendinizinkilerle değiştirerek DNS bölgesini oluşturmak için örneği kullanın.

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```
Azure DNS artık özel DNS bölgelerini de destekler (şu anda genel önizlemede).  Özel DNS bölgeleri hakkında daha fazla bilgi için bkz. [Özel etki alanları için Azure DNS'i kullanma](private-dns-overview.md). Özel bir DNS bölgesi oluşturma örneği için bkz. [PowerShell kullanarak Azure DNS özel bölgeleriyle çalışmaya başlama](./private-dns-getstarted-powershell.md).

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

`New-AzureRmDnsRecordSet` cmdlet’ini kullanarak kayıt kümeleri oluşturabilirsiniz. Aşağıdaki örnekte, "MyResourceGroup" kaynak grubu içindeki "contoso.com" DNS Bölgesinde göreli adı "www" olan bir kaynak oluşturulmaktadır. "www.contoso.com", kayıt kümesinin tam adıdır. Kayıt türü "A", IP adresi "1.2.3.4" ve TTL 3600 saniyedir.

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

Diğer kayıt türleri, birden fazla kayıt içerek kayıt kümeleri ve var olan kayıtların değiştirilmesi hakkında bilgi için bkz. [Azure PowerShell kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets.md). 


## <a name="view-records"></a>Kayıtları görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şu seçenekleri kullanın:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a>Ad sunucularını güncelleştirme

DNS bölgenizin ve kayıtlarınızın doğru şekilde ayarlandığına karar verdikten sonra, Azure DNS ad sunucularını kullanmak için etki alanınızın adını yapılandırmanız gerekir. Bunun yapılması, İnternet üzerindeki diğer kullanıcıların DNS kayıtlarınızı bulmasını sağlar.

Bölgenizin ad sunucuları `Get-AzureRmDnsZone` cmdlet’i tarafından belirtilir:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

Bu ad sunucuları, etki alanı adı kayıt şirketi (etki alanı adını satın aldığınız şirket) ile birlikte yapılandırılmalıdır. Kayıt şirketiniz, etki alanı için ad sunucularını ayarlama seçeneğini sunar. Daha fazla bilgi için bkz. [Etki alanınızı Azure DNS’e devretme](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Bu makalede oluşturulan tüm kaynakları silmek için, aşağıdaki adımları izleyin:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Azure DNS hakkında daha fazla bilgi için bkz. [Azure DNS'e genel bakış](dns-overview.md).

Azure DNS’te DNS bölgelerini yönetme hakkında daha fazla bilgi için bkz. [PowerShell ile Azure DNS’te DNS bölgelerini yönetme](dns-operations-dnszones.md).

Azure DNS’te DNS kayıtlarını yönetme hakkında daha fazla bilgi için bkz. [PowerShell ile Azure DNS’te DNS kayıtlarını yönetme](dns-operations-recordsets.md).

