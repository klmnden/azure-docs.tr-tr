---
title: "CLI kullanarak bir DNS Bölgesi için kayıt kümesi ve kayıt oluşturma| Microsoft Belgeleri"
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
ms.date: 12/09/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: bfbffe7843bc178cdf289c999925c690ab82e922
ms.openlocfilehash: e377f176fe24a8e7e42d409f86d6b0093ce5e7c4

---

# <a name="create-dns-record-sets-and-records-by-using-cli"></a>CLI kullanarak DNS kayıt kümelerini ve kayıtları oluşturma

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure CLI](dns-getstarted-create-recordset-cli.md)

Bu makale, CLI kullanarak kayıtlar ve kayıt kümeleri oluşturma işlemi boyunca size yol gösterir. Bunu yapmak için öncelikle DNS kayıtlarını ve kayıt kümelerini anlamanız gerekir.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Bu bölümde Azure DNS'de DNS kaydı oluşturma adımları açıklanmaktadır. Örnekte [Azure CLI'yi yüklediğiniz, oturum açtığınız ve DNS bölgesi oluşturduğunuz](dns-getstarted-create-dnszone-cli.md) varsayılmaktadır.

Bu sayfadaki tüm örnekler 'A' DNS kaydı türü kullanmaktadır. Diğer kayıt türleri ile DNS kayıtlarını ve kayıt kümelerini yönetme hakkında daha fazla bilgi için bkz. [Azure CLI kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-cli.md).

## <a name="create-a-record-set-and-record"></a>Kayıt kümesi ve kayıt oluşturma

Bu bölümde bir kayıt kümesinin ve kayıtların nasıl oluşturulduğunu size göstereceğiz. Bu örnekte, "contoso.com" DNS bölgesinde "www" göreli adına sahip olan bir kayıt kümesi oluşturacaksınız. "www.contoso.com" kayıtların tam adıdır. Kayıt türü "A" ve yaşam süresi (TTL) 60 saniyedir. Bu adımı tamamladıktan sonra, boş bir kayıt kümesi oluşturmuş olacaksınız.

Bölgenin tepesinde bir kayıt kümesi oluşturmak için (bu durumda "contoso.com"), tırnak işaretleri dahil olmak üzere "@", kayıt adını kullanın. Bu genel bir DNS kuralıdır.

### <a name="1-create-a-record-set"></a>1. Kayıt kümesi oluşturma

Yeni kaydınız var olan kayıtlardan biriyle aynı ada ve türe sahipse var olan kayıt kümesine eklemeniz gerekir. Bu adımı atlayarak aşağıdaki [Kayıt ekleme](#add-records) bölümüne geçebilirsiniz. Aksi halde yeni kaydınızın adı ve türü var olan tüm kayıtlardan farklıysa yeni bir kayıt kümesi oluşturmanız gerekir.

Kayıt kümesi oluşturmak için `azure network dns record-set create` komutunu kullanabilirsiniz. Yardım için bkz. `azure network dns record-set create -h`.  

Kayıt kümesi oluştururken kayıt kümesi adını, bölgeyi, yaşam süresini (TTL) ve kayıt türünü belirtmeniz gerekir. 

```azurecli
azure network dns record-set create myresourcegroup contoso.com www A 60
```

Bu adımı tamamladıktan sonra, boş bir "www" kayıt kümesine sahip olursunuz. Yeni oluşturulan "www" kayıt kümesini kullanmak için öncelikle buna kayıt eklemeniz gerekir.

### <a name="2-add-records"></a>2. Kayıt ekleme

`azure network dns record-set add-record` kullanarak kayıt kümelerine kayıt ekleyebilirsiniz. Yardım için bkz. `azure network dns record-set add-record -h`.

Bir kayıt kümesine kayıt eklemeye yönelik parametreler, kayıt kümesinin türüne bağlı olarak farklılık gösterir. Örneğin, "A" türünde bir kayıt kümesi kullanırken yalnızca `-a <IPv4 address>` parametreli kayıtları belirtebilirsiniz. Diğer kayıt türlerinin parametrelerini listelemek için bkz. `azure network dns record-set add-record -h`.

Aşağıdaki komutu kullanarak yukarıda oluşturulan "www" kayıt kümesine bir A kaydı ekleyebilirsiniz:

```azurecli
azure network dns record-set add-record myresourcegroup contoso.com  www A  -a 1.2.3.4
```

### <a name="verify-name-resolution"></a>Ad çözümlemesini doğrulama

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




<!--HONumber=Dec16_HO2-->


