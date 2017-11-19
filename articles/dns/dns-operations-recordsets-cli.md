---
title: "Azure CLI 2.0 kullanan Azure DNS'de DNS kayıtlarını yönetme | Microsoft Docs"
description: "DNS kayıt kümelerini ve Azure DNS kayıtlarını Azure DNS'nin etki alanınızda barındırdığında yönetme. Kayıt kümelerini ve kayıtları işlemleri için tüm CLI 2.0 komutları."
services: dns
documentationcenter: na
author: subsarma
manager: carmonm
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 11/08/2017
ms.author: subsarma
ms.openlocfilehash: 47be36aee053b81913286f0119edb6c8caa7c456
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-the-azure-cli-20"></a>DNS kayıtlarını ve kayıt kümeleri Azure CLI 2.0 kullanan Azure DNS'de yönetme

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Bu makalede, platformlar arası Azure komut satırı arabirimi (Windows, Mac ve Linux için kullanılabilir olan CLI) 2.0 kullanarak DNS bölgenizi için DNS kayıtlarını yönetme gösterilmektedir. DNS kayıtlarınızı kullanarak da yönetebilirsiniz [Azure PowerShell](dns-operations-recordsets.md) veya [Azure portal](dns-operations-recordsets-portal.md).

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri

Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

* [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md): Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI'mız.
* [Azure CLI 2.0](dns-operations-recordsets-cli.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız.

Bu makaledeki örneklerde, zaten sahip varsayılmaktadır [, oturum açan Azure CLI 2.0 yüklü ve bir DNS bölgesi oluşturulan](dns-operations-dnszones-cli.md).

## <a name="introduction"></a>Giriş

Azure DNS’de DNS kayıtlarını oluşturmadan önce Azure DNS’nin DNS kayıtlarını DNS kayıt kümeleri şeklinde nasıl düzenlediğini kavramanız gerekir.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Azure DNS’deki DNS kayıtları hakkında daha fazla bilgi için bkz. [DNS bölgeleri ve kayıtları](dns-zones-records.md).

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

Bir DNS kaydı oluşturmak üzere kullanmanız `az network dns record-set <record-type> set-record` komutu (burada `<record-type>` kayıt türü yani bir, srv, txt, vs.) Yardım için bkz. `az network dns record-set --help`.

Bir kayıt oluştururken kaynak grubu adını, bölge adını, kaynak kümesi adını, kaynak türünü ve oluşturulan kaynağın ayrıntılarını belirtmeniz gerekir. Verilen kayıt kümesi adı olmalıdır bir *göreli* ad, bölge adı dışarıda gerekir anlamına gelir.

Kayıt kümesi mevcut değilse bu komutla oluşturulur. Kayıt kümesi mevcutsa bu komut belirttiğiniz kaydı var olan kayıt kümesine ekler.

Yeni bir kayıt kümesi oluşturuluyorsa yaşam süresi (TTL) olarak varsayılan 3600 değeri kullanılır. Farklı TTLs kullanma hakkında daha fazla yönerge için bkz: [DNS kayıt kümesi oluşturma](#create-a-dns-record-set).

Aşağıdaki örnek *www* adlı A kaydını *contoso.com* bölgesinde ve *MyResourceGroup* kaynak grubu içinde oluşturur. A kaydının IP adresi *1.2.3.4* olarak belirtilmiştir.

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

Bölgenin tepesinde bir kayıt kümesi oluşturmak için (bu durumda "contoso.com"), tırnak işaretleri dahil olmak üzere "@" kayıt adını kullanın:

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --ipv4-address 1.2.3.4
```

## <a name="create-a-dns-record-set"></a>DNS kayıt kümesi oluşturma

Yukarıdaki örneklerde, DNS kaydı ya da var olan bir kayıt kümesine eklendi veya kayıt kümesi oluşturuldu *örtük olarak*. Kayıt kümesi de oluşturabilirsiniz *açıkça* kayıtlarını eklemeden önce. Azure DNS, DNS kayıtlarını oluşturmadan önce bir DNS adı ayırmak için bir yer tutucu olarak davranıp 'empty' kayıt kümelerini destekler. Boş kaydı kümeleri Azure DNS denetim düzeyi görünür, ancak Azure DNS ad sunucuları üzerinde görünmez.

Kayıt kümeleri kullanarak oluşturulur `az network dns record-set <record-type> create` komutu. Yardım için bkz. `az network dns record-set <record-type> create --help`.

Kayıt açıkça kümesi oluşturma kayıt kümesinin özellikler gibi belirtmenize olanak verir [yaşam süresi (TTL)](dns-zones-records.md#time-to-live) ve meta verileri. [Kayıt kümesi meta verileri](dns-zones-records.md#tags-and-metadata) uygulamaya özgü verileri anahtar-değer çiftleri olarak her bir kayıt kümesi ile ilişkilendirmek için kullanılabilir.

Aşağıdaki örnek, kullanarak bir 60 saniye TTL Değerine sahip ' A' türündeki bir boş kayıt kümesini oluşturur `--ttl` parametre (kısa form `-l`):

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --ttl 60
```

Aşağıdaki örnek, bir kayıt iki meta veri girişlerle kümesi oluşturur "Bölüm Finans =" ve "ortam üretim =", kullanarak `--metadata` parametre:

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --metadata "dept=finance" "environment=production"
```

Boş bir kayıt kümesi oluşturulduktan sonra kayıtları kullanılarak eklenebilir `azure network dns record-set <record-type> set-record` açıklandığı gibi [bir DNS kaydı oluşturma](#create-a-dns-record).

## <a name="create-records-of-other-types"></a>Diğer türleri kayıtları oluşturma

Aşağıdaki örnekler, ayrıntılı olarak 'Bir' kayıtları oluşturmak nasıl görülen, Azure DNS sunucuları tarafından desteklenen diğer kayıt türlerinin kaydının nasıl oluşturulacağını gösterir.

Kayıt verilerini belirtmek için kullanılan parametreler, kayıt türüne bağlı olarak değişiklik gösterir. Örneğin "A" türünde bir kayıt için IPv4 adresini `--ipv4-address <IPv4 address>` parametresiyle belirtirsiniz. Her bir kayıt türü için parametreler kullanılarak listelenebilir `az network dns record-set <record-type> set-record --help`.

Her durumda, tek bir kaydını nasıl oluşturacağınızı gösterir. Kayıt, varolan bir kayıt kümesini veya örtük olarak oluşturulmuş bir kayıt kümesine eklenir. Kayıt kümeleri oluşturma ve kayıt tanımlama hakkında daha fazla bilgi için parametre açıkça, bakın [DNS kayıt kümesi oluşturma](#create-a-dns-record-set).

SOAs oluşturulduğundan bir SOA kayıt kümesi oluşturmak için örnek vermeyin ve her DNS bölge ile silinmiş ve oluşturulamaz veya ayrı olarak silinir. Ancak, [SOA, bir sonraki örnekte gösterildiği gibi değiştirilebilir](#to-modify-an-SOA-record).

### <a name="create-an-aaaa-record"></a>Bir AAAA kaydı oluşturun

```azurecli
az network dns record-set aaaa set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-aaaa --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-an-caa-record"></a>Bir CAA kayıt oluşturma

```azurecli
az network dns record-set caa add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-caa --flags 0 --tag "issue" --value "ca1.contoso.com"
```

### <a name="create-a-cname-record"></a>Bir CNAME kaydı oluşturun

> [!NOTE]
> DNS standartlarında bir bölge tepesinde CNAME kayıtlarına izin vermediği (`--Name "@"`), ya da birden çok kayıt içeren kayıt kümeleri izin yapın.
> 
> Daha fazla bilgi için bkz: [CNAME kayıtları](dns-zones-records.md#cname-records).

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.contoso.com
```

### <a name="create-an-mx-record"></a>MX kaydı oluşturma

Bu örnekte, bölgenin tepesinde (bu durumda "contoso.com") MX kaydı oluşturmak için "@" kayıt kümesi adını kullanıyoruz.

```azurecli
az network dns record-set mx set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a>Bir NS kayıt oluşturma

```azurecli
az network dns record-set ns set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-ns --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a>PTR kaydı oluştur

Bu durumda, ' my-arpa-zone.com' IP aralığınızı temsil eden ARPA bölgeleri temsil eder. Bu bölgedeki her PTR kaydı bu IP aralığındaki bir IP adresine karşılık gelir.  Kayıt '10' IP adresi bu kayıt tarafından temsil edilen bu IP aralığı içinde son sekizlisi addır.

```azurecli
az network dns record-set ptr set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name my-arpa.zone.com --ptrdname myservice.contoso.com
```

### <a name="create-an-srv-record"></a>Bir SRV kaydı oluşturun

Oluştururken bir [SRV kayıt kümesi](dns-zones-records.md#srv-records), belirtin  *\_hizmet* ve  *\_Protokolü* kayıt kümesi adı. Eklenecek gerek yoktur "@" kayıt kümesi adında SRV kayıt kümesi bölgenin tepesinde oluştururken.

```azurecli
az network dns record-set srv set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name _sip._tls --priority 10 --weight 5 --port 8080 --target sip.contoso.com
```

### <a name="create-a-txt-record"></a>TXT kaydı oluşturma

Aşağıdaki örnek, bir TXT kaydını oluşturmak gösterilmiştir. TXT kayıtlarının desteklenen maksimum dize uzunluğu hakkında daha fazla bilgi için bkz: [TXT kayıtlarının](dns-zones-records.md#txt-records).

```azurecli
az network dns record-set txt set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-txt --value "This is a TXT record"
```

## <a name="get-a-record-set"></a>Bir kayıt kümesini Al

Varolan bir kayıt kümesini almak kullanın `az network dns record-set <record-type> show`. Yardım için bkz. `az network dns record-set <record-type> show --help`.

Bir kayıt veya kayıt kümesi oluşturulurken verilen kayıt kümesi adı olmalıdır gibi bir *göreli* ad, bölge adı dışarıda gerekir anlamına gelir. Kayıt kümesi ve bölge içeren kaynak grubunu içeren bölgesi kayıt türünü belirtmeniz gerekir.

Aşağıdaki örnek kaydı alır *www* bölgeden türü a *contoso.com* kaynak grubunda *MyResourceGroup*:

```azurecli
az network dns record-set a show --resource-group myresourcegroup --zone-name contoso.com --name www
```

## <a name="list-record-sets"></a>Liste kayıt kümeleri

Kullanarak bir DNS bölgedeki tüm kayıtları listeleyebilirsiniz `az network dns record-set list` komutu. Yardım için bkz. `az network dns record-set list --help`.

Bu örnekte tüm kayıt kümeleri bölgesinde döndürür *contoso.com*, kaynak grubundaki *MyResourceGroup*, adı veya kayıt türü ne olursa olsun:

```azurecli
az network dns record-set list --resource-group myresourcegroup --zone-name contoso.com
```

Bu örnek, belirli kayıt türü (Bu durumda, 'Bir' kayıtları) eşleşen tüm kayıt kümelerinin döndürür:

```azurecli
az network dns record-set a list --resource-group myresourcegroup --zone-name contoso.com 
```

## <a name="add-a-record-to-an-existing-record-set"></a>Bir kaydı var olan bir kayıt kümesine ekleme

Kullanabileceğiniz `az network dns record-set <record-type> set-record` hem yeni bir kayıt kümesinde bir kayıt oluşturmak veya var olan bir kayıt eklemek için kayıt kümesi.

Daha fazla bilgi için bkz: [bir DNS kaydı oluşturma](#create-a-dns-record) ve [kayıtlar diğer türleri oluşturma](#create-records-of-other-types) üstünde.

## <a name="remove-a-record-from-an-existing-record-set"></a>Varolan bir kayıt kümesinden bir kaydı kaldırma.

Bir DNS kaydı varolan bir kayıt kümesinden kaldırmak için kullanın `az network dns record-set <record-type> remove-record`. Yardım için bkz. `az network dns record-set <record-type> remove-record -h`.

Bu komut, kayıt kümesindeki bir DNS kaydı siler. Bir kayıt kümesindeki son kaydı silinirse, kayıt kümesine kendisi de silinir. Bunun yerine ayarlayın boş kaydı korumak için kullanmak `--keep-empty-record-set` seçeneği.

Silinecek kaydı belirtmeniz gerekir ve bölge, kullanarak bir kayıt oluştururken, aynı parametreleri kullanarak silinmesi `az network dns record-set <record-type> set-record`. Bu parametreler açıklanmaktadır [bir DNS kaydı oluşturma](#create-a-dns-record) ve [kayıtlar diğer türleri oluşturma](#create-records-of-other-types) üstünde.

Aşağıdaki örnek kaydındaki ' 1.2.3.4 adlandırılmış kümesi' değeriyle A kaydını siler *www* bölgesinde *contoso.com*, kaynak grubundaki *MyResourceGroup*.

```azurecli
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "www" --ipv4-address 1.2.3.4
```

## <a name="modify-an-existing-record-set"></a>Varolan bir kayıt kümesini değiştirme

Her bir kayıt kümesi içeren bir [yaşam süresi (TTL)](dns-zones-records.md#time-to-live), [meta veri](dns-zones-records.md#tags-and-metadata)ve DNS kayıtlarını. Aşağıdaki bölümlerde, bu özelliklerin her biri değiştirmek açıklanmaktadır.

### <a name="to-modify-an-a-aaaa-caa-mx-ns-ptr-srv-or-txt-record"></a>Bir A, AAAA, CAA, MX, NS, PTR, SRV ve TXT kaydı değiştirmek için

Var olan bir tür A, AAAA, CAA, MX, NS, PTR, SRV ve TXT kaydını değiştirmek için önce yeni bir kayıt ekleyin ve varolan kaydını silin. Bu makalenin önceki bölümlerinde silin ve kayıt ekleme konusunda ayrıntılı yönergeler için bkz.

Aşağıdaki örnek IP adresine 5.6.7.8 1.2.3.4 IP adresinden bir 'Bir' kaydını değiştirerek gösterilmektedir:

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 5.6.7.8
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

Ekle, Kaldır veya değiştiremezsiniz bölgenin tepesinde Ayarla otomatik olarak oluşturulan NS kaydı kayıtları (`--Name "@"`, tırnak işaretleri dahil olmak üzere). Bu kayıt kümesi için izin verilen yalnızca kaydı değiştirmeye TTL ve meta verileri ayarlama değişir.

### <a name="to-modify-a-cname-record"></a>Bir CNAME kaydı değiştirmek için

Çoğu diğer kayıt türlerinin aksine, bir CNAME kayıt kümesi yalnızca tek bir kaydını içerebilir.  Bu nedenle, yeni bir kayıt ekleyerek ve diğer kayıt türleri için olduğu gibi varolan bir kaydı kaldırma geçerli değeri değiştirilemiyor.

Bunun yerine bir CNAME kaydı değiştirmek için kullanın `az network dns record-set cname set-record`. Yardım için bkz.`az network dns record-set cname set-record --help`

CNAME kayıt kümesi örnek değiştirir *www* bölgesinde *contoso.com*, kaynak grubundaki *MyResourceGroup*, kendi varolan yerine 'www.fabrikam.net' işaret etmek için değer:

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.fabrikam.net
``` 

### <a name="to-modify-an-soa-record"></a>Bir SOA kaydına değiştirmek için

Çoğu diğer kayıt türlerinin aksine, bir CNAME kayıt kümesi yalnızca tek bir kaydını içerebilir.  Bu nedenle, yeni bir kayıt ekleyerek ve diğer kayıt türleri için olduğu gibi varolan bir kaydı kaldırma geçerli değeri değiştirilemiyor.

SOA kaydı değiştirmek için bunun yerine, kullanın `az network dns record-set soa update`. Yardım için bkz. `az network dns record-set soa update --help`.

Aşağıdaki örnekte, bölgenin SOA kaydına 'e-posta' özelliğini ayarlamak gösterilmiştir *contoso.com* kaynak grubunda *MyResourceGroup*:

```azurecli
az network dns record-set soa update --resource-group myresourcegroup --zone-name contoso.com --email admin.contoso.com
```

### <a name="to-modify-ns-records-at-the-zone-apex"></a>Bölge tepesinde NS kayıtları değiştirmek için

NS kayıt bölgenin tepesinde kümesi her DNS bölge ile otomatik olarak oluşturulur. Bölgeye atanan Azure DNS ad sunucularının adlarını içerir.

Birden fazla DNS sağlayıcınız ile birlikte barındırma etki alanlarını destekleyecek şekilde bu NS kaydının sunucularına ayarlayın ek ad ekleyebilirsiniz. Bu kayıt kümesi için meta verileri ve TTL de değiştirebilirsiniz. Ancak, kaldırmak veya önceden doldurulmuş haldedir Azure DNS ad sunucuları değiştirin.

Bu bölge tepesinde yalnızca NS kayıt kümesi için geçerli olduğunu unutmayın. (Alt bölgelere temsilci seçmek için kullanıldığı şekilde), bu bölgedeki diğer NS kayıt kümelerini kısıtlama değiştirilebilir.

Aşağıdaki örnekte, bölgenin tepesinde ayarlamak NS kaydı bir ek ad sunucusu eklemek gösterilmektedir:

```azurecli
az network dns record-set ns set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="to-modify-the-ttl-of-an-existing-record-set"></a>Varolan bir kayıt kümesi TTL değiştirmek için

Varolan bir kayıt kümesi TTL değiştirmek için `azure network dns record-set <record-type> update`. Yardım için bkz. `azure network dns record-set <record-type> update --help`.

Aşağıdaki örnek, bir kayıt kümesi TTL, bu durumda 60 saniye olarak değiştirmek gösterilmektedir:

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set ttl=60
```

### <a name="to-modify-the-metadata-of-an-existing-record-set"></a>Varolan bir kayıt kümesini meta verilerini değiştirmek için

[Kayıt kümesi meta verileri](dns-zones-records.md#tags-and-metadata) uygulamaya özgü verileri anahtar-değer çiftleri olarak her bir kayıt kümesi ile ilişkilendirmek için kullanılabilir. Varolan bir kayıt kümesini meta verilerini değiştirmek için `az network dns record-set <record-type> update`. Yardım için bkz. `az network dns record-set <record-type> update --help`.

Aşağıdaki örnek, bir kayıt kümesi iki meta veri girişi ile değiştirmek gösterilmiştir "Bölüm Finans =" ve "ortam üretim =". Var olan herhangi bir meta veri olduğuna dikkat edin *yerini* verilen değerlere göre.

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set metadata.dept=finance metadata.environment=production
```

## <a name="delete-a-record-set"></a>Bir kayıt kümesini Sil

Kayıt kümeleri kullanarak silinebilir `az network dns record-set <record-type> delete` komutu. Yardım için bkz. `azure network dns record-set <record-type> delete --help`. Kayıt kümesi siliniyor, kayıt kümesindeki tüm kayıtları siler.

> [!NOTE]
> SOA silemezsiniz ve NS kayıt kümeleri bölgenin tepesinde (`--name "@"`).  Bu, zaman dilimi oluşturulduğunu ve bölge silindiğinde otomatik olarak silinir otomatik olarak oluşturulur.

Aşağıdaki örnek kayıt adlandırılmış kümesi siler *www* bölgesinden türü a *contoso.com* kaynak grubunda *MyResourceGroup*:

```azurecli
az network dns record-set a delete --resource-group myresourcegroup --zone-name contoso.com --name www
```

Silme işlemi onaylamanız istenir. Bu uyarıyı gizlemek için kullanın `--yes` geçin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [bölgeleri ve Azure DNS kayıtlarında](dns-zones-records.md).
<br>
Bilgi nasıl [bölgeleri ve kayıtları korumak](dns-protect-zones-recordsets.md) Azure DNS kullanırken.
