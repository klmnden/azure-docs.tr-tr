---
title: 'Hızlı başlangıç: Azure PowerShell’i kullanarak Azure DNS bölgesi ve kaydı oluşturma'
description: Azure DNS'te DNS bölgesi ve kaydı oluşturma hakkında bilgi edinin. Bu hızlı başlangıçta, Azure PowerShell kullanarak ilk DNS bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım yönergeler sağlanır.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: ccf60a333dcc83e27702d572f922ef6aec741c14
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730302"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-powershell"></a>Hızlı Başlangıç: Bir Azure DNS bölgesi ve kaydı Azure PowerShell kullanarak oluşturma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu hızlı başlangıçta PowerShell kullanarak ilk DNS bölgenizi ve kaydını oluşturursunuz. Ayrıca, [Azure portal](dns-getstarted-portal.md) veya [Azure CLI](dns-getstarted-cli.md)'sini kullanarak aşağıdaki adımları gerçekleştirebilirsiniz. 

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Son olarak, DNS bölgenizi Internet'te yayımlamak için etki alanının ad sunucularını yapılandırmanız gerekir. Bu adımların her biri aşağıda açıklanmıştır.

Azure DNS ayrıca özel etki alanları oluşturmayı da destekler. İlk özel DNS bölgenizi ve kaydınızı oluşturma hakkında adım adım yönergeler için [PowerShell ile Azure DNS özel bölgelerini kullanmaya başlama](private-dns-getstarted-powershell.md) konusunu inceleyin.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

DNS bölgesini oluşturmadan önce, DNS bölgesini içerecek kaynak grubunu oluşturun.

```powershell
New-AzResourceGroup -name MyResourceGroup -location "eastus"
```

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `New-AzDnsZone` cmdlet’i kullanılarak oluşturulur. Aşağıdaki örnekte adlı bir DNS bölgesi oluşturur *contoso.xyz* adlı kaynak grubunda *MyResourceGroup*. Değerleri kendinizinkilerle değiştirerek DNS bölgesini oluşturmak için örneği kullanın.

```powershell
New-AzDnsZone -Name contoso.xyz -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

`New-AzDnsRecordSet` cmdlet’ini kullanarak kayıt kümeleri oluşturabilirsiniz. Aşağıdaki örnek "contoso.xyz", "MyResourceGroup" kaynak grubunda DNS bölgesinde "www" göreli adı olan bir kaynak oluşturulmaktadır. Kayıt kümesinin tam adı "www.contoso.xyz" dir. Kayıt türü "A", "10.10.10.10" IP adresine sahip olan ve TTL 3600 saniyedir.

```powershell
New-AzDnsRecordSet -Name www -RecordType A -ZoneName contoso.xyz -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "10.10.10.10")
```

## <a name="view-records"></a>Kayıtları görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şu seçenekleri kullanın:

```powershell
Get-AzDnsRecordSet -ZoneName contoso.xyz -ResourceGroupName MyResourceGroup
```

## <a name="test-the-name-resolution"></a>Ad çözümlemesini test etme

Test 'A' kaydı test DNS bölgesi olduğuna göre ad çözümlemesi adında bir araç ile test edebilirsiniz *nslookup*. 

**DNS ad çözümlemesini test etmek için:**

1. Bölgeniz için ad sunucuları listesini almak için aşağıdaki cmdlet'i çalıştırın:

   ```azurepowershell
   Get-AzDnsRecordSet -ZoneName contoso.xyz -ResourceGroupName MyResourceGroup -RecordType ns
   ```

1. Ad sunucusu adlarını biri önceki adımın çıktıdan kopyalayın.

1. Bir komut istemi açın ve aşağıdaki komutu çalıştırın:

   ```
   nslookup www.contoso.xyz <name server name>
   ```

   Örneğin:

   ```
   nslookup www.contoso.xyz ns1-08.azure-dns.com.
   ```

   Aşağıdaki ekrana benzer bir şey görmeniz gerekir:

   ![nslookup](media/dns-getstarted-portal/nslookup.PNG)

Ana bilgisayar adı **www\.contoso.xyz** çözümler **10.10.10.10**yapılandırdığınız şekilde. Bu sonuç, ad çözümlemesi doğru şekilde çalıştığını doğrular.

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Artık gerekmediğinde, kaynak grubunu silerek bu hızlı başlangıçta oluşturulan tüm kaynakları silebilirsiniz:

```powershell
Remove-AzResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell kullanarak ilk DNS bölgenizi ve kaydı oluşturduğunuza göre artık özel etki alanında bulunan bir web uygulaması için kayıt oluşturabilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında bir web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)

