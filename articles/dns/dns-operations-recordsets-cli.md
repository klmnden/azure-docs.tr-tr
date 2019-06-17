---
title: Azure CLI ile Azure DNS'te DNS kayıtlarını yönetme | Microsoft Docs
description: DNS kayıt kümeleri ve Azure DNS kayıtları etki alanınızı Azure DNS'ye üzerinde barındırırken yönetme.
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 05/15/2018
ms.author: victorh
ms.openlocfilehash: 4864a46b91b4e243ce6a2ae3d9d36df28fe74d8d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61293364"
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-the-azure-cli"></a>DNS kayıtlarını ve Azure CLI kullanarak Azure DNS kayıt kümelerini yönetme

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Bu makalede, Windows, Mac ve Linux için kullanılabildiği platformlar arası Azure CLI kullanarak DNS bölgenizin DNS kayıtlarını yönetme işlemini göstermektedir. Ayrıca kullanarak DNS kayıtlarınızı yönetebilirsiniz [Azure PowerShell](dns-operations-recordsets.md) veya [Azure portalında](dns-operations-recordsets-portal.md).

Bu makaledeki örneklerde varsayılmaktadır [yüklü Azure CLI, oturum açtığınız ve DNS bölgesi oluşturduğunuz](dns-operations-dnszones-cli.md).

## <a name="introduction"></a>Giriş

Azure DNS’de DNS kayıtlarını oluşturmadan önce Azure DNS’nin DNS kayıtlarını DNS kayıt kümeleri şeklinde nasıl düzenlediğini kavramanız gerekir.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Azure DNS’deki DNS kayıtları hakkında daha fazla bilgi için bkz. [DNS bölgeleri ve kayıtları](dns-zones-records.md).

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

Bir DNS kaydı oluşturmak için kullanın `az network dns record-set <record-type> add-record` komut (burada `<record-type>` kayıt türü yani srv, txt, vs.) Yardım için bkz. `az network dns record-set --help`.

Bir kayıt oluştururken kaynak grubu adını, bölge adını, kaynak kümesi adını, kaynak türünü ve oluşturulan kaynağın ayrıntılarını belirtmeniz gerekir. Verilen kayıt kümesi adı olmalıdır bir *göreli* ad, bölge adı dışarıda gerekir anlamına gelir.

Kayıt kümesi mevcut değilse bu komutla oluşturulur. Kayıt kümesi mevcutsa bu komut belirttiğiniz kaydı var olan kayıt kümesine ekler.

Yeni bir kayıt kümesi oluşturuluyorsa yaşam süresi (TTL) olarak varsayılan 3600 değeri kullanılır. Farklı TTL'ler kullanma hakkında yönergeler için bkz: [DNS kayıt kümesi oluşturma](#create-a-dns-record-set).

Aşağıdaki örnek *www* adlı A kaydını *contoso.com* bölgesinde ve *MyResourceGroup* kaynak grubu içinde oluşturur. A kaydının IP adresi *1.2.3.4* olarak belirtilmiştir.

```azurecli
az network dns record-set a add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

İçinde bölgenin tepesinde bir kayıt kümesi oluşturmak için (Bu durumda "contoso.com"), kayıt adını kullanın "\@", tırnak işaretleri dahil olmak üzere:

```azurecli
az network dns record-set a add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --ipv4-address 1.2.3.4
```

## <a name="create-a-dns-record-set"></a>DNS kayıt kümesi oluşturma

Yukarıdaki örneklerde, DNS kaydı ya da var olan bir kayıt kümesine eklendi veya kayıt kümesi oluşturulmuş *örtük olarak*. Kayıt kümesi de oluşturabilirsiniz *açıkça* kayıtlarını eklemeden önce. Azure DNS, DNS kayıtlarını oluşturmadan önce bir DNS adını ayırmak için bir yer tutucu olarak görev yapabilir 'empty' kayıt kümelerini destekler. Boş kayıt kümeleri, Azure DNS denetimi düzlemde görünür, ancak Azure DNS ad sunucularında görünmez.

Kayıt kümeleri kullanılarak oluşturulur `az network dns record-set <record-type> create` komutu. Yardım için bkz. `az network dns record-set <record-type> create --help`.

Kayıt kümesi açıkça oluşturma kayıt kümesinin özellikleri gibi belirtmenize olanak verir [yaşam süresi (TTL)](dns-zones-records.md#time-to-live) ve meta verileri. [Kayıt kümesi meta verileri](dns-zones-records.md#tags-and-metadata) uygulamaya özgü verileri, anahtar-değer çiftleri olarak her bir kayıt kümesi ile ilişkilendirmek için kullanılabilir.

Aşağıdaki örnek, kullanarak bir 60 saniye TTL ' A' türünde bir boş kayıt kümesi oluşturur `--ttl` parametre (kısa form `-l`):

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --ttl 60
```

Aşağıdaki örnek, iki meta veri girdileri ile bir kayıt oluşturur. "Bölüm Finans =" ve "ortam üretim =", kullanarak `--metadata` parametresi:

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --metadata "dept=finance" "environment=production"
```

Boş bir kayıt kümesi oluşturulduktan sonra kayıtları kullanarak eklenebilir `azure network dns record-set <record-type> add-record` açıklandığı [bir DNS kaydı oluşturma](#create-a-dns-record).

## <a name="create-records-of-other-types"></a>Diğer tür kayıtları oluşturma

Aşağıdaki örnekler, 'A' kaydı oluşturma adımları ayrıntılı olarak görülen, Azure DNS tarafından desteklenen diğer kayıt türlerinin kaydının nasıl oluşturulacağını gösterir.

Kayıt verilerini belirtmek için kullanılan parametreler, kayıt türüne bağlı olarak değişiklik gösterir. Örneğin "A" türünde bir kayıt için IPv4 adresini `--ipv4-address <IPv4 address>` parametresiyle belirtirsiniz. Tüm kayıt türlerine ait parametreleri kullanarak listelenebilir `az network dns record-set <record-type> add-record --help`.

Her durumda, tek bir kaydını nasıl oluşturacağınızı göstereceğiz. Kayıt, var olan kayıt kümesine ya da örtük olarak oluşturulmuş bir kayıt kümesi için eklenir. Kayıt kümeleri oluşturma ve kayıt tanımlama hakkında daha fazla bilgi için parametre açıkça, bakın [DNS kayıt kümesi oluşturma](#create-a-dns-record-set).

SOAs oluşturulduğundan bir SOA kayıt kümesi oluşturmak için örnek atamayın ve ile her bir DNS bölgesi silindi ve oluşturulamıyor veya ayrı olarak silindi. Ancak, [SOA, bir sonraki örnekte gösterildiği gibi değiştirilebilir](#to-modify-an-soa-record).

### <a name="create-an-aaaa-record"></a>Bir AAAA kaydı oluşturun

```azurecli
az network dns record-set aaaa add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-aaaa --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-an-caa-record"></a>CAA kaydı oluşturun

```azurecli
az network dns record-set caa add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-caa --flags 0 --tag "issue" --value "ca1.contoso.com"
```

### <a name="create-a-cname-record"></a>bir CNAME kaydı oluşturun

> [!NOTE]
> DNS standartları bölge tepesinde CNAME kayıtlarına izin vermez (`--Name "@"`), ya da birden fazla kayıt içeren kayıt kümeleri izin yapın.
> 
> Daha fazla bilgi için [CNAME kayıtları](dns-zones-records.md#cname-records).

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.contoso.com
```

### <a name="create-an-mx-record"></a>Bir MX kaydı oluşturun

Bu örnekte, kayıt kümesi adını kullanıyoruz "\@" bölge tepesinde MX kaydı oluşturmak için (Bu durumda "contoso.com").

```azurecli
az network dns record-set mx add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a>Bir NS kayıt oluşturma

```azurecli
az network dns record-set ns add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-ns --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a>PTR kaydı oluştur

Bu durumda ' my-arpa-Zone ' IP aralığınızı temsil eden ARPA bölgesini ifade eder. Bu bölgedeki her PTR kaydı bu IP aralığındaki bir IP adresine karşılık gelir.  Kayıt adı '10' IP adresinin bu kayıt tarafından temsil edilen bu IP aralığındaki son sekizli ' dir.

```azurecli
az network dns record-set ptr add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name my-arpa.zone.com --ptrdname myservice.contoso.com
```

### <a name="create-an-srv-record"></a>Bir SRV kaydı oluşturun

Oluştururken bir [SRV kayıt kümesi](dns-zones-records.md#srv-records), belirtin  *\_hizmet* ve  *\_Protokolü* kayıt kümesi adı. Eklemenize gerek yoktur "\@" SRV kayıt kümesi bölgenin tepesinde oluştururken kayıt kümesi adı.

```azurecli
az network dns record-set srv add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name _sip._tls --priority 10 --weight 5 --port 8080 --target sip.contoso.com
```

### <a name="create-a-txt-record"></a>Bir TXT kaydı oluşturun

Aşağıdaki örnek, bir TXT kaydı oluşturma işlemi gösterilmektedir. TXT kayıtlarının desteklenen maksimum dize uzunluğu hakkında daha fazla bilgi için bkz: [TXT kayıtlarının](dns-zones-records.md#txt-records).

```azurecli
az network dns record-set txt add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-txt --value "This is a TXT record"
```

## <a name="get-a-record-set"></a>Kayıt kümesi Al

Mevcut bir kayıt kümesi almak için kullanın `az network dns record-set <record-type> show`. Yardım için bkz. `az network dns record-set <record-type> show --help`.

Bir kayıt veya kayıt kümesi oluştururken, verilen kayıt kümesi adı olması gerektiği bir *göreli* ad, bölge adı dışarıda gerekir anlamına gelir. Kayıt türü, kayıt kümesi ve bölge içeren kaynak grubunu içeren bir bölge belirtmeniz gerekir.

Aşağıdaki örnek kaydı alır *www* tür a bölgeden *contoso.com* kaynak grubundaki *MyResourceGroup*:

```azurecli
az network dns record-set a show --resource-group myresourcegroup --zone-name contoso.com --name www
```

## <a name="list-record-sets"></a>Kayıt kümeleri listesi

Kullanarak tüm kayıtları bir DNS bölgesi listeleyebilirsiniz `az network dns record-set list` komutu. Yardım için bkz. `az network dns record-set list --help`.

Bu örnek, tüm kayıt kümeleri bölgede döndürür *contoso.com*, kaynak grubundaki *MyResourceGroup*, ad veya kayıt türü bağımsız olarak:

```azurecli
az network dns record-set list --resource-group myresourcegroup --zone-name contoso.com
```

Bu örnek, belirli kayıt türü (Bu durumda, 'A' kaydı) eşleşen tüm kayıt kümelerini döndürür:

```azurecli
az network dns record-set a list --resource-group myresourcegroup --zone-name contoso.com 
```

## <a name="add-a-record-to-an-existing-record-set"></a>Bir kaydı var olan bir kayıt kümesine ekleyin.

Kullanabileceğiniz `az network dns record-set <record-type> add-record` hem yeni bir kayıt kümesindeki bir kayıt oluşturmak veya mevcut bir kayıt eklemek için kayıt kümesi.

Daha fazla bilgi için [bir DNS kaydı oluşturma](#create-a-dns-record) ve [diğer tür kayıtlar oluşturma](#create-records-of-other-types) yukarıda.

## <a name="remove-a-record-from-an-existing-record-set"></a>Mevcut bir kayıt kümesinden bir kaydı kaldırın.

Bir DNS kaydı var olan bir kayıt kümesinden kaldırmak için `az network dns record-set <record-type> remove-record`. Yardım için bkz. `az network dns record-set <record-type> remove-record -h`.

Bu komut, kayıt kümesindeki bir DNS kaydı siler. Bir kayıt kümesindeki son kaydı silinirse, kayıt kümesi kendisi de silinir. Bunun yerine boş kaydını tutmak için kullanın `--keep-empty-record-set` seçeneği.

Silinecek kaydın belirtmeniz gerekir ve bölge, kullanarak bir kayıt oluştururken, aynı parametreleri kullanarak silinmesi `az network dns record-set <record-type> add-record`. Bu parametreler açıklanmıştır [bir DNS kaydı oluşturma](#create-a-dns-record) ve [diğer tür kayıtlar oluşturma](#create-records-of-other-types) yukarıda.

Aşağıdaki örnek kayıttan ' 1.2.3.4 adlandırılmış kümesi' değerine sahip bir kaydı siler *www* bölgesinde *contoso.com*, kaynak grubundaki *MyResourceGroup*.

```azurecli
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "www" --ipv4-address 1.2.3.4
```

## <a name="modify-an-existing-record-set"></a>Mevcut bir kayıt kümesini değiştirme

Her bir kayıt kümesi içeren bir [yaşam süresi (TTL)](dns-zones-records.md#time-to-live), [meta verileri](dns-zones-records.md#tags-and-metadata)ve DNS kayıtları. Aşağıdaki bölümlerde, bu özelliklerin her biri değiştirme işlemleri açıklanmaktadır.

### <a name="to-modify-an-a-aaaa-caa-mx-ns-ptr-srv-or-txt-record"></a>Bir A, AAAA, CAA, MX, NS, PTR, SRV ve TXT kaydı değiştirmek için

Var olan bir tür A, AAAA, CAA, MX, NS, PTR, SRV ve TXT kaydı değiştirmek için önce yeni bir kayıt ekleyin ve varolan bir kaydı silin. Bu makalenin önceki bölümlerinde silin ve kayıt ekleme konusunda ayrıntılı yönergeler için bkz.

Aşağıdaki örnek, bir 'A' kaydı IP adresine 5.6.7.8 1.2.3.4 IP adresinden değiştirmek gösterilmektedir:

```azurecli
az network dns record-set a add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 5.6.7.8
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

Ekleyemez, Kaldır veya otomatik olarak oluşturulan NS kayıt bölge tepesinde kümesine kayıtları değiştirme (`--Name "@"`, tırnak işaretleri dahil olmak üzere). Bu kayıt kümesi için izin verilen tek bir kaydı değiştirmek için TTL ve meta verileri ayarlama değişikliklerdir.

### <a name="to-modify-a-cname-record"></a>CNAME kaydı değiştirmek için

Çoğu diğer kayıt türlerinin aksine, bir CNAME kaydı kümesi yalnızca tek bir kayıt içerebilir.  Bu nedenle, yeni bir kayıt eklemek ve diğer kayıt türleri için varolan bir kaydı kaldırarak geçerli değeri değiştirilemiyor.

Bunun yerine bir CNAME kaydı değiştirmek için kullanın `az network dns record-set cname set-record`. Yardım için bkz: `az network dns record-set cname set-record --help`

CNAME kaydı kümesi örnek değiştirir *www* bölgesinde *contoso.com*, kaynak grubundaki *MyResourceGroup*, mevcut yerine 'www.fabrikam.net için' işaret edecek şekilde değer:

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.fabrikam.net
``` 

### <a name="to-modify-an-soa-record"></a>SOA kaydı değiştirmek için

Çoğu diğer kayıt türlerinin aksine, bir CNAME kaydı kümesi yalnızca tek bir kayıt içerebilir.  Bu nedenle, yeni bir kayıt eklemek ve diğer kayıt türleri için varolan bir kaydı kaldırarak geçerli değeri değiştirilemiyor.

SOA kaydı değiştirmek için bunun yerine, kullanın `az network dns record-set soa update`. Yardım için bkz. `az network dns record-set soa update --help`.

Aşağıdaki örnek 'e-posta' özelliğinin bölgenin SOA kaydının nasıl ayarlandığını gösterir *contoso.com* kaynak grubundaki *MyResourceGroup*:

```azurecli
az network dns record-set soa update --resource-group myresourcegroup --zone-name contoso.com --email admin.contoso.com
```

### <a name="to-modify-ns-records-at-the-zone-apex"></a>NS kayıtları bölgenin tepesindeki değiştirmek için

NS kayıt bölge tepesinde kümesi her DNS bölge ile otomatik olarak oluşturulur. Bu bölgeye atanan Azure DNS ad sunucularının adlarını içerir.

Ek ad sunucuları bu NS kayıt için birden fazla DNS sağlayıcısı ile ortak barındırma etki alanlarını destekleyecek şekilde ayarlama, ekleyebilirsiniz. Bu kayıt kümesi için meta veriler ve TTL de değiştirebilirsiniz. Ancak, kaldırın veya önceden doldurulmuş Azure DNS ad sunucularını değiştirin.

Bu bölge tepesinde yalnızca NS kayıt kümesi için geçerli olduğunu unutmayın. (Alt bölgeleri temsilci seçmek için kullanıldığı şekilde), bu bölgedeki diğer NS kayıt kümeleri, kısıtlama değiştirilebilir.

Aşağıdaki örnekte bölgenin tepesinde ayarlayın NS kaydı ek ad sunucusu ekleme gösterir:

```azurecli
az network dns record-set ns add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="to-modify-the-ttl-of-an-existing-record-set"></a>Var olan bir kayıt kümesine TTL değiştirmek için

Var olan bir kayıt kümesine TTL değiştirmek için `azure network dns record-set <record-type> update`. Yardım için bkz. `azure network dns record-set <record-type> update --help`.

Aşağıdaki örnek, bir kayıt kümesi TTL, bu durumda 60 saniye olarak değiştirmek gösterilmektedir:

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set ttl=60
```

### <a name="to-modify-the-metadata-of-an-existing-record-set"></a>Mevcut bir kayıt kümesi meta verileri değiştirmek için

[Kayıt kümesi meta verileri](dns-zones-records.md#tags-and-metadata) uygulamaya özgü verileri, anahtar-değer çiftleri olarak her bir kayıt kümesi ile ilişkilendirmek için kullanılabilir. Mevcut bir kayıt kümesi meta verileri değiştirmek için `az network dns record-set <record-type> update`. Yardım için bkz. `az network dns record-set <record-type> update --help`.

Aşağıdaki örnek, iki meta veri girdileri ile bir kaydı değiştirmek gösterilmektedir "Bölüm Finans =" ve "ortam üretim =". Var olan tüm meta verilere unutmayın *yerini* tarafından verilen bir değer.

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set metadata.dept=finance metadata.environment=production
```

## <a name="delete-a-record-set"></a>Bir kayıt kümesini Sil

Kullanarak kayıt kümelerini silinebilir `az network dns record-set <record-type> delete` komutu. Yardım için bkz. `azure network dns record-set <record-type> delete --help`. Kayıt kümesi sildiğinizde, kayıt kümesindeki tüm kayıtlar da silinir.

> [!NOTE]
> SOA silemezsiniz ve NS kayıt kümeleri bölgenin tepesinde (`--name "@"`).  Bunlar, ne zaman dilimi oluşturuldu ve bölge silindiğinde otomatik olarak silinir otomatik olarak oluşturulur.

Aşağıdaki örnek, kayıt adlandırılmış kümesi siler *www* tür a bölgesinden *contoso.com* kaynak grubundaki *MyResourceGroup*:

```azurecli
az network dns record-set a delete --resource-group myresourcegroup --zone-name contoso.com --name www
```

Silme işlemini onaylamanız istenir. Bu uyarıyı engellemek için kullanın `--yes` geçin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [bölgelerini ve kayıtlarını Azure DNS'de](dns-zones-records.md).
<br>
Bilgi nasıl [bölgeleri ve kayıtlarını koruma](dns-protect-zones-recordsets.md) Azure DNS kullanırken.
