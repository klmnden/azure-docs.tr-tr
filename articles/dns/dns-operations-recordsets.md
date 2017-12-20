---
title: "Azure PowerShell kullanarak Azure DNS'de DNS kayıtlarını yönetme | Microsoft Docs"
description: "DNS kayıt kümelerini ve Azure DNS kayıtlarını Azure DNS'nin etki alanınızda barındırdığında yönetme. Kayıt kümelerini ve kayıtları işlemleri için tüm PowerShell komutları."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 7136a373-0682-471c-9c28-9e00d2add9c2
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: fee96a77436f09e5cf2841b36b244e2d03f57f74
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a>DNS kayıtlarını ve kayıt kümeleri Azure PowerShell kullanarak Azure DNS'de yönetme

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Bu makalede, Azure PowerShell kullanarak DNS bölgenizi için DNS kayıtlarını yönetme gösterilmektedir. DNS kayıtlarını da yönetilebilir platformlar arası kullanarak [Azure CLI](dns-operations-recordsets-cli.md) veya [Azure portal](dns-operations-recordsets-portal.md).

Bu makaledeki örneklerde, zaten sahip varsayılmaktadır [, oturum açan Azure PowerShell yüklenmiş ve bir DNS bölgesi oluşturulan](dns-operations-dnszones.md).

## <a name="introduction"></a>Giriş

Azure DNS’de DNS kayıtlarını oluşturmadan önce Azure DNS’nin DNS kayıtlarını DNS kayıt kümeleri şeklinde nasıl düzenlediğini kavramanız gerekir.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Azure DNS’deki DNS kayıtları hakkında daha fazla bilgi için bkz. [DNS bölgeleri ve kayıtları](dns-zones-records.md).


## <a name="create-a-new-dns-record"></a>Yeni bir DNS kaydı oluşturun

Yeni kaydınız aynı ad ve varolan bir kayıt türü varsa, gerek [var kayıt kümesine ekleme](#add-a-record-to-an-existing-record-set). Farklı bir ad ve tüm mevcut kayıtları tür yeni kaydınız varsa, yeni bir kayıt kümesi oluşturmanız gerekir. 

### <a name="create-a-records-in-a-new-record-set"></a>Yeni bir kayıt kümesinde 'Bir' kayıtları oluşturma

`New-AzureRmDnsRecordSet` cmdlet’ini kullanarak kayıt kümeleri oluşturabilirsiniz. Kayıt kümesi oluştururken, kaydı Canlı (TTL), ad, bölge, saati ayarlamak belirtmek zorunda oluşturulacak kayıt türünü ve kaydeder.

Bir kayıt kümesine kayıt eklemeye yönelik parametreler, kayıt kümesinin türüne bağlı olarak farklılık gösterir. Örneğin, 'A' türündeki kayıt kümesi kullanırken, parametre kullanarak IP adresini belirtmek ihtiyacınız `-IPv4Address`. Diğer parametreler diğer kayıt türleri için kullanılır. Bkz: [ek kayıt türü örnekleri](#additional-record-type-examples) Ayrıntılar için.

Aşağıdaki örnek, bir kayıt DNS bölgesinde "contoso.com" göreli adına sahip 'www' kümesi oluşturur. Tam kayıt kümesinin 'www.contoso.com' adıdır. 'A' kayıt türüdür ve TTL 3600 saniye. Kayıt kümesi '1.2.3.4' IP adresiyle tek bir kayıt içerir.

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

Bir kayıt 'tepesinde' bir bölgenin kümesi oluşturmak için (Bu durumda, "contoso.com"), kayıt kümesi adını kullanın ' @' (tırnak işaretleri hariç):

```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

Bir kayıt birden çok kayıt içeren kümesi oluşturmanız gerekiyorsa, ilk yerel bir dizi oluşturmak ve kayıtları ekleyin ve sonra diziye geçirmek `New-AzureRmDnsRecordSet` gibi:

```powershell
$aRecords = @()
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

[Kayıt kümesi meta verileri](dns-zones-records.md#tags-and-metadata) uygulamaya özgü verileri anahtar-değer çiftleri olarak her bir kayıt kümesi ile ilişkilendirmek için kullanılabilir. Aşağıdaki örnekte bir kayıt iki meta veri girişlerle kümesi oluşturma gösterilmektedir ' bölüm Finans =' ve ' ortam Üretim ='.

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

Azure DNS, ayrıca DNS kayıtlarını oluşturmadan önce bir DNS adı ayırmak için bir yer tutucu olarak davranıp 'empty' kayıt kümelerini destekler. Boş kayıt kümeleri içinde Azure DNS denetim düzlemi görünür, ancak Azure DNS ad sunucuları üzerinde görünür. Aşağıdaki örnek, boş bir kayıt kümesi oluşturur:

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a>Diğer türleri kayıtları oluşturma

Aşağıdaki örnekler, ayrıntılı olarak 'Bir' kayıtları oluşturmak nasıl görülen, Azure DNS sunucuları tarafından desteklenen diğer kayıt türlerinin kayıtları oluşturmak nasıl gösterir.

Her durumda, tek bir kayıt içeren ayarlamak kaydının nasıl oluşturulacağını gösterir. 'Bir' kayıtları önceki örneklerde, meta veriler ile birden çok kayıt içeren diğer türleri kayıt kümesini oluşturun veya boş kaydı kümeleri oluşturmak için uyarlanabilir.

SOAs oluşturulduğundan bir SOA kayıt kümesi oluşturmak için örnek vermeyin ve her DNS bölge ile silinmiş ve oluşturulamaz veya ayrı olarak silinir. Ancak, [SOA, bir sonraki örnekte gösterildiği gibi değiştirilebilir](#to-modify-an-SOA-record).

### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Tek kayıtlı bir AAAA kayıt kümesi oluşturma

```powershell
New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ipv6Address "2607:f8b0:4009:1803::1005") 
```

### <a name="create-a-caa-record-set-with-a-single-record"></a>CAA kayıt ile tek bir kayıt kümesi oluşturma

```powershell
New-AzureRmDnsRecordSet -Name "test-caa" -RecordType CAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Caaflags 0 -CaaTag "issue" -CaaValue "ca1.contoso.com") 
```

### <a name="create-a-cname-record-set-with-a-single-record"></a>Tek kayıtlı bir CNAME kayıt kümesi oluşturma

> [!NOTE]
> DNS standartlarında bir bölge tepesinde CNAME kayıtlarına izin vermediği (`-Name '@'`), ya da birden çok kayıt içeren kayıt kümeleri izin yapın.
> 
> Daha fazla bilgi için bkz: [CNAME kayıtları](dns-zones-records.md#cname-records).


```powershell
New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a>Tek kayıtlı bir MX kayıt kümesi oluşturma

Bu örnekte, kayıt kümesi adını kullanıyoruz ' @' bölgenin tepesinde bir MX kaydı oluşturmak için (Bu durumda, "contoso.com").


```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a>Tek kayıtlı bir NS kayıt kümesi oluşturma

```powershell
New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a>Tek kayıtlı bir PTR kaydı kümesi oluşturma

Bu durumda, ' my-arpa-zone.com' IP aralığınızı temsil eden ARPA geriye doğru arama bölgesini temsil eder. Bu bölgedeki her PTR kaydı bu IP aralığındaki bir IP adresine karşılık gelir. Kayıt '10' IP adresi bu kayıt tarafından temsil edilen bu IP aralığı içinde son sekizlisi addır.

```powershell
New-AzureRmDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a>Tek kayıtlı bir SRV kayıt kümesi oluşturma

Oluştururken bir [SRV kayıt kümesi](dns-zones-records.md#srv-records), belirtin  *\_hizmet* ve  *\_Protokolü* kayıt kümesi adı. Dahil etmek için gerekli ' @' kayıt kümesi adında SRV kayıt kümesi bölgenin tepesinde oluştururken.

```powershell
New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a>Bir tek kayıtlı bir TXT kayıt kümesi oluşturma

Aşağıdaki örnek, bir TXT kaydını oluşturmak gösterilmiştir. TXT kayıtlarının desteklenen maksimum dize uzunluğu hakkında daha fazla bilgi için bkz: [TXT kayıtlarının](dns-zones-records.md#txt-records).

```powershell
New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a>Bir kayıt kümesini Al

Varolan bir kayıt kümesini almak kullanın `Get-AzureRmDnsRecordSet`. Bu cmdlet kayıt Azure DNS'de kümesine temsil eden yerel bir nesne döndürür.

İle `New-AzureRmDnsRecordSet`, verilen kayıt kümesi adı olmalıdır bir *göreli* ad, bölge adı dışarıda gerekir anlamına gelir. Ayrıca kayıt türünü belirtmeniz gerekir ve kayıt içeren dilimini ayarlayın.

Aşağıdaki örnek, bir kayıt kümesini Al gösterilmektedir. Bu örnekte, bölgenin kullanarak belirtilen `-ZoneName` ve `-ResourceGroupName` parametreleri.

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Alternatif olarak, aynı zamanda kullanılarak geçirilen bir bölge nesnesi kullanarak bölgesi belirtebilirsiniz `-Zone` parametresi.

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a>Liste kayıt kümeleri

Aynı zamanda `Get-AzureRmDnsZone` listesini atlanması tarafından bir bölgedeki kayıt kümelerine `-Name` ve/veya `-RecordType` parametreleri.

Aşağıdaki örnekte, tüm kayıt bölgede ayarlar döndürür:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Aşağıdaki örnek, tüm kayıt kümeleri belirli bir türde nasıl gösterir kaydı atlama kümesi adı kayıt türü belirterek alınabilir:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Kayıt türleri arasında belirli bir ada sahip tüm kayıt kümelerini almak için tüm kayıt kümelerinin almak ve sonuçlara filtre uygulamak gerekir:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

Tüm Yukarıdaki örneklerde, bölge ya da kullanılarak belirtilebilir `-ZoneName` ve `-ResourceGroupName`parametreleri (gösterildiği gibi) veya bir bölge nesnesi belirterek:

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzureRmDnsRecordSet -Zone $zone
```

## <a name="add-a-record-to-an-existing-record-set"></a>Bir kaydı var olan bir kayıt kümesine ekleme

Bir kaydı var olan bir kayıt kümesine eklemek için aşağıdaki üç adımı izleyin:

1. Mevcut kayıt kümesini Al

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Yeni kayıttaki yerel kayıt kümesine ekleyin. Bu çevrimdışı bir işlemdir.

    ```powershell
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Değişikliği geri Azure DNS hizmeti uygulayın. 

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $rs
    ```

Kullanarak `Set-AzureRmDnsRecordSet` *değiştirir* mevcut kayıt Azure DNS (ve içerdiği tüm kayıtları) belirtilen kayıt kümesi ile kümesi. [ETag denetimleri](dns-zones-records.md#etags) eşzamanlı değişiklikleri yazılmaz sağlamak için kullanılır. Kullanabileceğiniz isteğe bağlı `-Overwrite` bu denetimleri gizlemek için anahtar.

Bu işlem dizisi de olabilir *yöneltilen*, parametre olarak geçirme yerine kanal kullanarak kayıt kümesi nesnesi geçirdiğiniz anlamına gelir:

```powershell
Get-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

Yukarıdaki örnekler bir '' kayıt türü 'A' Varolan kayıt kümesine eklemek nasıl gösterir. Benzer bir işlem dizisi değiştirerek diğer türleri kayıt kümelerine kayıtları eklemek için kullanılan `-Ipv4Address` parametresinin `Add-AzureRmDnsRecordConfig` her bir kayıt türü belirli diğer parametrelere sahip. Her bir kayıt türü için parametreler aynıdır `New-AzureRmDnsRecordConfig` gösterildiği gibi cmdlet [ek kayıt türü örnekleri](#additional-record-type-examples) üstünde.

Kayıt kümeleri 'CNAME' veya 'SOA' türünde birden fazla kayıt içeremez. Bu sınırlama DNS standartları ortaya çıkar. Azure DNS sınırlandırılmasıdır değil.

## <a name="remove-a-record-from-an-existing-record-set"></a>Varolan bir kayıt kümesinden bir kaydı kaldırma

Bir kayıt kümesinden bir kaydı kaldırma işlemini, var olan bir kayıt kümesine kayıt ekleme işlemi benzerdir:

1. Mevcut kayıt kümesini Al

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Kayıt yerel kayıt kümesi nesnesinden kaldırın. Bu çevrimdışı bir işlemdir. Kaldırılmakta kaydı varolan bir kayıtla tam bir eşleşme tüm parametreleri arasında olmalıdır.

    ```powershell
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Değişikliği geri Azure DNS hizmeti uygulayın. İsteğe bağlı kullanmak `-Overwrite` gizlemek için anahtar [Etag denetler](dns-zones-records.md#etags) eşzamanlı değişiklikleri.

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $Rs
    ```

Son kaydı kayıt kümesinden kaldırmak için yukarıdaki dizisi kullanılarak kayıt kümesi silinmez, bunun yerine boş bir kayıt kümesi bırakır. Bir kayıt kümesi tamamen kaldırmak için bkz: [bir kayıt kümesini Sil](#delete-a-record-set).

Benzer şekilde bir kayıt kümesine kayıt eklemeye bir kayıt kümesini kaldırmak için işlem dizisi de yöneltilen:

```powershell
Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

Farklı kayıt türleri için uygun türe özgü parametreleri geçirerek desteklenir `Remove-AzureRmDnsRecordSet`. Her bir kayıt türü için parametreler aynıdır `New-AzureRmDnsRecordConfig` gösterildiği gibi cmdlet [ek kayıt türü örnekleri](#additional-record-type-examples) üstünde.


## <a name="modify-an-existing-record-set"></a>Varolan bir kayıt kümesini değiştirme

Varolan bir kayıt kümesini değiştirme adımları ekleme veya kayıt kümesindeki kayıt kaldırırken adımlar benzerdir:

1. Varolan kayıt kullanarak kümesi almak `Get-AzureRmDnsRecordSet`.
2. Yerel kayıt kümesi nesnesi tarafından değiştirin:
    * Ekleme veya kayıt kaldırma
    * Varolan kayıtları parametrelerini değiştirme
    * Kayıt değiştirme, meta verileri ve zamanı dinamik (TTL) ayarlayın
3. Kullanarak, değişiklikleri `Set-AzureRmDnsRecordSet` cmdlet'i. Bu *değiştirir* mevcut kayıt belirtilen kayıt kümesi ile Azure DNS'de kümesi.

Kullanırken `Set-AzureRmDnsRecordSet`, [Etag denetler](dns-zones-records.md#etags) eşzamanlı değişiklikleri yazılmaz sağlamak için kullanılır. Kullanabileceğiniz isteğe bağlı `-Overwrite` bu denetimleri gizlemek için anahtar.

### <a name="to-update-a-record-in-an-existing-record-set"></a>Varolan bir kayıt kümesindeki bir kaydı güncelleştirmeye

Bu örnekte, varolan 'Bir' kaydı IP adresini değiştirin:

```powershell
$rs = Get-AzureRmDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-an-soa-record"></a>Bir SOA kaydına değiştirmek için

Ekleme veya kayıtları otomatik olarak oluşturulan SOA kayıt bölgenin tepesinde kümesi kaldırın (`-Name "@"`, tırnak işaretleri dahil olmak üzere). Ancak, SOA kaydı (dışında "ana bilgisayar") içinde parametrelerinden herhangi birini değiştirmek ve TTL kaydı ayarlayın.

Aşağıdaki örnekte nasıl değiştirileceğini gösterir *e-posta* SOA kaydı özelliği:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-ns-records-at-the-zone-apex"></a>Bölge tepesinde NS kayıtları değiştirmek için

NS kayıt bölgenin tepesinde kümesi her DNS bölge ile otomatik olarak oluşturulur. Bölgeye atanan Azure DNS ad sunucularının adlarını içerir.

Birden fazla DNS sağlayıcınız ile birlikte barındırma etki alanlarını destekleyecek şekilde bu NS kaydının sunucularına ayarlayın ek ad ekleyebilirsiniz. Bu kayıt kümesi için meta verileri ve TTL de değiştirebilirsiniz. Ancak, kaldırmak veya önceden doldurulmuş haldedir Azure DNS ad sunucuları değiştirin.

Bu bölge tepesinde yalnızca NS kayıt kümesi için geçerli olduğunu unutmayın. (Alt bölgelere temsilci seçmek için kullanıldığı şekilde), bu bölgedeki diğer NS kayıt kümelerini kısıtlama değiştirilebilir.

Aşağıdaki örnekte, bölgenin tepesinde ayarlamak NS kaydı bir ek ad sunucusu eklemek gösterilmektedir:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-record-set-metadata"></a>Kayıt kümesi meta verilerini değiştirmek için

[Kayıt kümesi meta verileri](dns-zones-records.md#tags-and-metadata) uygulamaya özgü verileri anahtar-değer çiftleri olarak her bir kayıt kümesi ile ilişkilendirmek için kullanılabilir.

Aşağıdaki örnek, varolan bir kayıt kümesini meta verileri değiştirmeye gösterilmektedir:

```powershell
# Get the record set
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"

# Add 'dept=finance' name-value pair
$rs.Metadata.Add('dept', 'finance') 

# Remove metadata item named 'environment'
$rs.Metadata.Remove('environment')  

# Commit changes
Set-AzureRmDnsRecordSet -RecordSet $rs
```


## <a name="delete-a-record-set"></a>Bir kayıt kümesini Sil

Kayıt kümeleri kullanarak silinebilir `Remove-AzureRmDnsRecordSet` cmdlet'i. Kayıt kümesi siliniyor, kayıt kümesindeki tüm kayıtları siler.

> [!NOTE]
> SOA silemezsiniz ve NS kayıt kümeleri bölgenin tepesinde (`-Name '@'`).  Azure DNS bölgesi oluşturuldu ve bölge silindiğinde bunları otomatik olarak siler. Bunlar otomatik olarak oluşturulur.

Aşağıdaki örnek, bir kayıt kümesini Sil gösterilmektedir. Bu örnekte, kayıt kümesi adını, kayıt kümesi türü, bölge adını ve kaynak grubu her açıkça belirtilmiş.

```powershell
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Alternatif olarak, kayıt kümesi adını ve türünü ve nesneyi kullanarak belirtilen bölge tarafından belirtilebilir:

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

Üçüncü seçenek olarak, kayıt kendisini kümesine bir kayıt kümesi nesnesi kullanılarak belirtilebilir:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -RecordSet $rs
```

Kayıt bir kayıt kümesi nesnesi kullanarak silinecek kümesi belirttiğinizde [Etag denetler](dns-zones-records.md#etags) eşzamanlı değişiklikleri silinmez sağlamak için kullanılır. Kullanabileceğiniz isteğe bağlı `-Overwrite` bu denetimleri gizlemek için anahtar.

Kayıt kümesi nesnesi bir parametre olarak geçirilen yerine de yöneltilen:

```powershell
Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzureRmDnsRecordSet
```

## <a name="confirmation-prompts"></a>Onay istekleri

`New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, Ve `Remove-AzureRmDnsRecordSet` cmdlet'leri tüm destek onay ister.

Her bir cmdlet için onay ister `$ConfirmPreference` PowerShell tercih değişkeni değerine sahip `Medium` veya daha düşük. İçin varsayılan değer itibaren `$ConfirmPreference` olan `High`, bu komut istemlerini varsayılan PowerShell ayarlarını kullanırken verilmez.

Geçerli kılabilirsiniz `$ConfirmPreference` kullanarak ayarlama `-Confirm` parametresi. Belirtirseniz `-Confirm` veya `-Confirm:$True` , cmdlet sizden onay kendisinden önce çalışır ister. Belirtirseniz `-Confirm:$False` , cmdlet onaylamanız istenmez. 

Hakkında daha fazla bilgi için `-Confirm` ve `$ConfirmPreference`, bkz: [tercih değişkenleri hakkında](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [bölgeleri ve Azure DNS kayıtlarında](dns-zones-records.md).
<br>
Bilgi nasıl [bölgeleri ve kayıtları korumak](dns-protect-zones-recordsets.md) Azure DNS kullanırken.
<br>
Gözden geçirme [Azure DNS PowerShell başvuru belgeleri](/powershell/module/azurerm.dns).
