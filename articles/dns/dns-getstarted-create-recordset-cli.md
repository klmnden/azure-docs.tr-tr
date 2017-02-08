---
title: "Azure CLI kullanarak DNS kaydı oluşturma | Microsoft Belgeleri"
description: "Azure DNS için ana bilgisayar kayıtları nasıl oluşturulur? CLI kullanarak kayıt kümelerini ve kayıtları ayarlama"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 02b897d3-e83b-4257-b96d-5c29aa59e843
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 18a21cdc0f9641356dfaf6f6d93edfcac11af210
ms.openlocfilehash: 790af1544ed86155f5f864f3914b5fd1c4f42f4b

---

# <a name="create-dns-records-using-the-azure-cli"></a>Azure CLI kullanarak DNS kaydı oluşturma

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure CLI](dns-getstarted-create-recordset-cli.md)

Bu makale, Azure CLI kullanarak kayıtlar ve kayıt kümeleri oluşturma işlemi boyunca size yol gösterir.

## <a name="introduction"></a>Giriş

Azure DNS’de DNS kayıtlarını oluşturmadan önce Azure DNS’nin DNS kayıtlarını DNS kayıt kümeleri şeklinde nasıl düzenlediğini kavramanız gerekir.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Azure DNS’deki DNS kayıtları hakkında daha fazla bilgi için bkz. [DNS bölgeleri ve kayıtları](dns-zones-records.md).

## <a name="create-a-record-set-and-record"></a>Kayıt kümesi ve kayıt oluşturma

Bu bölümde Azure DNS'de DNS kaydı oluşturma adımları açıklanmaktadır. Örnekte [Azure CLI'yi yüklediğiniz, oturum açtığınız ve DNS bölgesi oluşturduğunuz](dns-getstarted-create-dnszone-cli.md) varsayılmaktadır.

Bu sayfadaki tüm örnekler 'A' DNS kaydı türü kullanmaktadır. Diğer kayıt türleri ile DNS kayıtlarını ve kayıt kümelerini yönetme hakkında daha fazla bilgi için bkz. [Azure CLI kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-cli.md).

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

DNS kaydı oluşturmak için `azure network dns record-set add-record` komutunu kullanın. Yardım için bkz. `azure network dns record-set add-record -h`.

Bir kayıt oluştururken kaynak grubu adını, bölge adını, kaynak kümesi adını, kaynak türünü ve oluşturulan kaynağın ayrıntılarını belirtmeniz gerekir.

Kayıt kümesi mevcut değilse bu komutla oluşturulur. Kayıt kümesi mevcutsa bu komut belirttiğiniz kaydı var olan kayıt kümesine ekler. 

Yeni bir kayıt kümesi oluşturuluyorsa yaşam süresi (TTL) olarak varsayılan 3600 değeri kullanılır. Farklı TTL’ler kullanma hakkında talimatlar için bkz. [Azure CLI kullanarak Azure DNS’deki DNS kayıtlarını yönetme](dns-operations-recordsets-cli.md).

Aşağıdaki örnek *www* adlı A kaydını *contoso.com* bölgesinde ve *MyResourceGroup* kaynak grubu içinde oluşturur. A kaydının IP adresi *1.2.3.4* olarak belirtilmiştir.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Bölgenin tepesinde bir kayıt kümesi oluşturmak için (bu durumda "contoso.com"), tırnak işaretleri dahil olmak üzere "@", kayıt adını kullanın:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

Kayıt verilerini belirtmek için kullanılan parametreler, kayıt türüne bağlı olarak değişiklik gösterir. Örneğin "A" türünde bir kayıt için IPv4 adresini `-a <IPv4 address>` parametresiyle belirtirsiniz. Diğer kayıt türlerinin parametrelerini listelemek için bkz. `azure network dns record-set add-record -h`. Tüm kayıt türlerine ait örnekler için bkz. [Azure CLI kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-cli.md).


## <a name="verify-name-resolution"></a>Ad çözümlemesini doğrulama

nslookup, dig veya [Resolve-DnsName PowerShell cmdlet'i](https://technet.microsoft.com/library/jj590781.aspx) gibi DNS araçlarını kullanarak DNS kayıtlarınızın Azure DNS ad sunucularında mevcut olup olmadığını kontrol edebilirsiniz.

Azure DNS'de henüz yeni bölgeyi kullanmak için etki alanınızı devretmediyseniz [DNS sorgusunu bölgenizin ad sunucularından birine doğrudan yönlendirmeniz](dns-getstarted-create-dnszone.md#test-name-servers) gerekir. Aşağıdaki komutta kayıt bölgeniz için doğru değerleri değiştirdiğinizden emin olun.

    nslookup
    > set type=A
    > server ns1-01.azure-dns.com
    > www.contoso.com

    Server:  ns1-01.azure-dns.com
    Address:  40.90.4.1

    Name:    www.contoso.com
    Address:  1.2.3.4

## <a name="next-steps"></a>Sonraki adımlar

[Etki alanı adınızı Azure DNS ad sunucularına devretmeyi](dns-domain-delegation.md) öğrenin.

[Azure CLI'yi kullanarak DNS bölgelerini yönetmeyi](dns-operations-dnszones-cli.md) öğrenin.

[Azure CLI'yi kullanarak DNS kayıtlarını ve kayıt kümelerini yönetmeyi](dns-operations-recordsets-cli.md) öğrenin.




<!--HONumber=Feb17_HO1-->


