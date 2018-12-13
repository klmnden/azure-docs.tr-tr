---
title: 'Hızlı başlangıç: Azure PowerShell’i kullanarak Azure DNS bölgesi ve kaydı oluşturma'
description: Azure DNS'te DNS bölgesi ve kaydı oluşturma hakkında bilgi edinin. Bu hızlı başlangıçta, Azure PowerShell kullanarak ilk DNS bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım yönergeler sağlanır.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 12/4/2018
ms.author: victorh
ms.openlocfilehash: db6ac089f11477ecdb1568195855fae00d3eac61
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52888367"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell’i kullanarak Azure DNS bölgesi ve kaydı oluşturma

Bu hızlı başlangıçta PowerShell kullanarak ilk DNS bölgenizi ve kaydını oluşturursunuz. Ayrıca, [Azure portal](dns-getstarted-portal.md) veya [Azure CLI](dns-getstarted-cli.md)'sini kullanarak aşağıdaki adımları gerçekleştirebilirsiniz. 

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Son olarak, DNS bölgenizi Internet'te yayımlamak için etki alanının ad sunucularını yapılandırmanız gerekir. Bu adımların her biri aşağıda açıklanmıştır.

Azure DNS ayrıca özel etki alanları oluşturmayı da destekler. İlk özel DNS bölgenizi ve kaydınızı oluşturma hakkında adım adım yönergeler için [PowerShell ile Azure DNS özel bölgelerini kullanmaya başlama](private-dns-getstarted-powershell.md) konusunu inceleyin.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

DNS bölgesini oluşturmadan önce, DNS bölgesini içerecek kaynak grubunu oluşturun.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "eastus"
```

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `New-AzureRmDnsZone` cmdlet’i kullanılarak oluşturulur. Aşağıdaki örnek, *MyResourceGroup* adlı kaynak grubunda *contoso.com* adlı bir DNS bölgesi oluşturur. Değerleri kendinizinkilerle değiştirerek DNS bölgesini oluşturmak için örneği kullanın.

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

`New-AzureRmDnsRecordSet` cmdlet’ini kullanarak kayıt kümeleri oluşturabilirsiniz. Aşağıdaki örnekte, "MyResourceGroup" kaynak grubu içindeki "contoso.com" DNS Bölgesinde göreli adı "www" olan bir kaynak oluşturulmaktadır. "www.contoso.com", kayıt kümesinin tam adıdır. Kayıt türü "A", IP adresi "1.2.3.4" ve TTL 3600 saniyedir.

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

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

Bu ad sunucuları, etki alanı adı kayıt şirketi (etki alanı adını satın aldığınız şirket) ile birlikte yapılandırılmalıdır. Kayıt şirketiniz, etki alanı için ad sunucularını ayarlama seçeneğini sunar. Daha fazla bilgi için [Öğreticisi: etki alanınızı Azure DNS'de konak](dns-delegate-domain-azure-dns.md#delegate-the-domain).

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Artık gerekmediğinde, kaynak grubunu silerek bu hızlı başlangıçta oluşturulan tüm kaynakları silebilirsiniz:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell kullanarak ilk DNS bölgenizi ve kaydı oluşturduğunuza göre artık özel etki alanında bulunan bir web uygulaması için kayıt oluşturabilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında bir web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)

