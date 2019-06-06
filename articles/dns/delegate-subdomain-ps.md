---
title: Azure PowerShell kullanarak bir Azure DNS alt etki alanı temsilcisi
description: Azure PowerShell kullanarak bir Azure DNS alt etki alanı temsilcisi öğrenin.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 2/7/2019
ms.author: victorh
ms.openlocfilehash: 4ee4d9e6390c9a091096bb7c06160b76fd8af90f
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730281"
---
# <a name="delegate-an-azure-dns-subdomain-using-azure-powershell"></a>Azure PowerShell kullanarak bir Azure DNS alt etki alanı temsilcisi

Bir DNS alt etki alanı temsilcisi için Azure PowerShell kullanabilirsiniz. Örneğin, contoso.com etki alanına aitse, adlı bir alt etki alanını devredebilirsiniz *mühendislik* başka ve ayrı bölgesine contoso.com bölgesi ayrı olarak yönetebilir.

Tercih ederseniz, kullanarak bir alt etki alanını devredebilirsiniz [Azure portalı](delegate-subdomain.md).

> [!NOTE]
> Contoso.com, bu makalenin tamamında örnek olarak kullanılır. contoso.com yerine kendi etki alanı adınızı yazın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Önkoşullar

Bir Azure DNS alt etki alanı atanacak genel etki alanınızı Azure DNS'e devretmeniz gerekir. Bkz: [bir etki alanını Azure DNS'ye devretme](./dns-delegate-domain-azure-dns.md) ad sunucularınızın temsilcisi için yapılandırma hakkında yönergeler için. Etki alanınızda, Azure DNS bölgesini temsilci sonra alt etki alanını devredebilirsiniz.

## <a name="create-a-zone-for-your-subdomain"></a>Bir bölge, alt etki alanı oluşturma

İlk olarak, bölgenin oluşturma **mühendislik** alt etki alanı.

`New-AzDnsZone -ResourceGroupName <resource group name> -Name engineering.contoso.com`

## <a name="note-the-name-servers"></a>Ad sunucularını unutmayın

Ardından, mühendislik alt etki alanı için dört ad sunucusunun unutmayın.

`Get-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -RecordType NS`

## <a name="create-a-test-record"></a>Bir test kaydı oluşturun

Oluşturma bir **A** test etmek için kullanılacak Mühendisliği bölgenin kayıt.

   `New-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -Name www -RecordType A -ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address 10.10.10.10)`.

## <a name="create-an-ns-record"></a>Bir NS kayıt oluşturma

Ardından, bir ad sunucusu (NS) kaydı için oluşturma **mühendislik** contoso.com bölgesi bölgesinde.

```azurepowershell
$Records = @()
$Records += New-AzDnsRecordConfig -Nsdname <name server 1 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 2 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 3 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 4 noted previously>
$RecordSet = New-AzDnsRecordSet -Name engineering -RecordType NS -ResourceGroupName <resource group name> -TTL 3600 -ZoneName contoso.com -DnsRecords $Records
```

## <a name="test-the-delegation"></a>Temsilci seçmeyi test

Temsilci seçmeyi test etmek için nslookup kullanın.

1. Bir PowerShell penceresi açın.
2. Komut istemine yazın `nslookup www.engineering.contoso.com.`
3. Adresini gösteren bir yetkili olmayan yanıt alması gereken **10.10.10.10**.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [Azure'da barındırılan hizmetleri için ters DNS yapılandırma](dns-reverse-dns-for-azure-services.md).