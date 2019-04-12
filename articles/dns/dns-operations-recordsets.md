---
title: Azure PowerShell ile Azure DNS'te DNS kayıtlarını yönetme | Microsoft Docs
description: DNS kayıt kümeleri ve Azure DNS kayıtları etki alanınızı Azure DNS'ye üzerinde barındırırken yönetme. Kayıt kümeleri ve kayıtları üzerinde işlemler için tüm PowerShell komutları.
services: dns
documentationcenter: na
author: vhorne
manager: timlt
ms.assetid: 7136a373-0682-471c-9c28-9e00d2add9c2
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: victorh
ms.openlocfilehash: 61871ada0679a68b7f9d872a0df36d22cfb1f0de
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59491211"
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a>DNS kayıtlarını ve Azure PowerShell kullanarak Azure DNS kayıt kümelerini yönetme

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure klasik CLI](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Bu makalede, Azure PowerShell kullanarak DNS bölgenizin DNS kayıtlarını yönetme işlemini göstermektedir. DNS kayıtlarını da yönetilebilir platformlar arası kullanarak [Azure CLI](dns-operations-recordsets-cli.md) veya [Azure portalında](dns-operations-recordsets-portal.md).

Bu makaledeki örneklerde varsayılmaktadır [, oturum açtığınız, Azure PowerShell yüklenmiş ve DNS bölgesi oluşturduğunuz](dns-operations-dnszones.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="introduction"></a>Giriş

Azure DNS’de DNS kayıtlarını oluşturmadan önce Azure DNS’nin DNS kayıtlarını DNS kayıt kümeleri şeklinde nasıl düzenlediğini kavramanız gerekir.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Azure DNS’deki DNS kayıtları hakkında daha fazla bilgi için bkz. [DNS bölgeleri ve kayıtları](dns-zones-records.md).


## <a name="create-a-new-dns-record"></a>Yeni bir DNS kaydı oluşturma

Yeni kaydınızı aynı ada ve türe varolan bir kaydı olarak varsa yapmanız [var olan kayıt kümesine eklemeniz](#add-a-record-to-an-existing-record-set). Yeni kaydınızın adı ve türü var olan tüm kayıtlardan varsa, yeni bir kayıt kümesi oluşturmanız gerekir. 

### <a name="create-a-records-in-a-new-record-set"></a>Yeni bir kayıt kümesindeki 'A' kaydı oluşturma

`New-AzDnsRecordSet` cmdlet’ini kullanarak kayıt kümeleri oluşturabilirsiniz. Kayıt, Canlı (TTL) için ad, bölge, zaman kümesi belirtmenize gerek kayıt kümesi oluştururken kayıt türünü ve kayıtları oluşturulacak.

Bir kayıt kümesine kayıt eklemeye yönelik parametreler, kayıt kümesinin türüne bağlı olarak farklılık gösterir. Örneğin, 'A' türünde bir kayıt kümesi kullanırken, parametre kullanarak IP adresini belirtmeniz gerekir `-IPv4Address`. Diğer parametreler, diğer kayıt türleri için kullanılır. Ek kayıt türü örnekleri Ayrıntılar için bkz.

Aşağıdaki örnek, "contoso.com" DNS bölgesinde göreli adı "www" ile ayarlanmış bir kayıt oluşturur. Kayıt kümesinin tam adı "www.contoso.com" dir. 'A' kaydı türüdür ve TTL 3600 saniyedir. Tek kayıtlı, IP adresi '1.2.3.4' kayıt kümesi içerir.

```powershell
New-AzDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "1.2.3.4") 
```

Ayarla '' bir bölgenin tepesinde bir kayıt oluşturmak için (Bu durumda "contoso.com"), kayıt kümesi adını kullan '\@' (çift tırnak işaretlerini atarak):

```powershell
New-AzDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "1.2.3.4") 
```

Bir kayıt kümesi birden fazla kayıt içeren oluşturmanız gerekiyorsa, önce yerel bir dizi oluşturmak ve kayıtları eklemek ve ardından diziye geçirmek `New-AzDnsRecordSet` gibi:

```powershell
$aRecords = @()
$aRecords += New-AzDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

[Kayıt kümesi meta verileri](dns-zones-records.md#tags-and-metadata) uygulamaya özgü verileri, anahtar-değer çiftleri olarak her bir kayıt kümesi ile ilişkilendirmek için kullanılabilir. Aşağıdaki örnek, iki meta veri girdileri ile kayıt oluşturma işlemi gösterilmektedir ' bölüm Finans =' ve ' ortamında Üretim ='.

```powershell
New-AzDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

Azure DNS, DNS kayıtlarını oluşturmadan önce bir DNS adını ayırmak için bir yer tutucu olarak görev yapabilir 'empty' kayıt kümelerini de destekler. Boş kayıt kümeleri, Azure DNS denetimi düzlemde görünür, ancak Azure DNS ad sunucularında görünür. Aşağıdaki örnek, boş bir kayıt kümesi oluşturur:

```powershell
New-AzDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a>Diğer tür kayıtları oluşturma

Aşağıdaki örnekler, 'A' kaydı oluşturma adımları ayrıntılı olarak görülen, Azure DNS tarafından desteklenen diğer kayıt türleri kayıtlarının nasıl oluşturulacağını gösterir.

Her durumda, bir kayıt içeren tek bir kayıt kümesi oluşturmak nasıl göstereceğiz. Diğer tür meta verilerle birden çok kayıt içeren kayıt kümeleri oluşturma veya boş bir kayıt kümesi oluşturmak için önceki örnekler 'A' kaydı için uyarlanabilir.

SOAs oluşturulduğundan bir SOA kayıt kümesi oluşturmak için örnek atamayın ve ile her bir DNS bölgesi silindi ve oluşturulamıyor veya ayrı olarak silindi. Ancak, [SOA, bir sonraki örnekte gösterildiği gibi değiştirilebilir](#to-modify-an-soa-record).

### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Tek kayıtlı bir AAAA kayıt kümesi oluşturma

```powershell
New-AzDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Ipv6Address "2607:f8b0:4009:1803::1005") 
```

### <a name="create-a-caa-record-set-with-a-single-record"></a>CAA kaydı ile tek bir kayıt kümesi oluşturma

```powershell
New-AzDnsRecordSet -Name "test-caa" -RecordType CAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Caaflags 0 -CaaTag "issue" -CaaValue "ca1.contoso.com") 
```

### <a name="create-a-cname-record-set-with-a-single-record"></a>Tek kayıtlı bir CNAME kayıt kümesi oluşturma

> [!NOTE]
> DNS standartları bölge tepesinde CNAME kayıtlarına izin vermez (`-Name '@'`), ya da birden fazla kayıt içeren kayıt kümeleri izin yapın.
> 
> Daha fazla bilgi için [CNAME kayıtları](dns-zones-records.md#cname-records).


```powershell
New-AzDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a>Tek kayıtlı bir MX kayıt kümesi oluşturma

Bu örnekte, kayıt kümesi adını kullanıyoruz '\@' bölgenin tepesinde bir MX kaydı oluşturmak için (Bu durumda "contoso.com").


```powershell
New-AzDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a>Tek kayıtlı bir NS kayıt kümesi oluşturma

```powershell
New-AzDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a>Tek kayıtlı bir PTR kaydı kümesi oluşturma

Bu durumda ' my-arpa-Zone ' IP aralığınızı temsil eden ARPA geriye doğru arama bölgesini temsil eder. Bu bölgedeki her PTR kaydı bu IP aralığındaki bir IP adresine karşılık gelir. Kayıt adı '10' IP adresinin bu kayıt tarafından temsil edilen bu IP aralığındaki son sekizli ' dir.

```powershell
New-AzDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a>Tek kayıtlı bir SRV kayıt kümesi oluşturma

Oluştururken bir [SRV kayıt kümesi](dns-zones-records.md#srv-records), belirtin  *\_hizmet* ve  *\_Protokolü* kayıt kümesi adı. Eklemenize gerek yoktur '\@' bir SRV kaydı bölge tepesinde kümesi oluştururken kayıt kümesi adı.

```powershell
New-AzDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a>Bir tek kayıtlı bir TXT kayıt kümesi oluşturma

Aşağıdaki örnek, bir TXT kaydı oluşturma işlemi gösterilmektedir. TXT kayıtlarının desteklenen maksimum dize uzunluğu hakkında daha fazla bilgi için bkz: [TXT kayıtlarının](dns-zones-records.md#txt-records).

```powershell
New-AzDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a>Kayıt kümesi Al

Mevcut bir kayıt kümesi almak için kullanın `Get-AzDnsRecordSet`. Bu cmdlet, kayıt kümesine Azure DNS'de temsil eden yerel bir nesne döndürür.

Olduğu gibi `New-AzDnsRecordSet`, verilen kayıt kümesi adı olmalıdır bir *göreli* ad, bölge adı dışarıda gerekir anlamına gelir. Ayrıca kayıt türünü belirtmeniz gerekir ve kaydın bulunduğu bölgeye ayarlayın.

Aşağıdaki örnek, bir kayıt kümesi almak gösterilmektedir. Bu örnekte, bölgenin kullanarak belirtilen `-ZoneName` ve `-ResourceGroupName` parametreleri.

```powershell
$rs = Get-AzDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Alternatif olarak, ayrıca iletilen bir bölge nesnesini kullanarak bölgeyi belirtin `-Zone` parametresi.

```powershell
$zone = Get-AzDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a>Kayıt kümeleri listesi

Ayrıca `Get-AzDnsZone` atlama tarafından bir bölgedeki kayıt kümeleri listesi için `-Name` ve/veya `-RecordType` parametreleri.

Aşağıdaki örnek, tüm kayıt bölgeyi ayarlar döndürür:

```powershell
$recordsets = Get-AzDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Aşağıdaki örnek, tüm kaydı kümeleri belirli bir türden nasıl gösterir. kaydı atlayarak adı ayarlanırken, kayıt türünü belirterek alınabilir:

```powershell
$recordsets = Get-AzDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Kayıt türleri arasında belirli bir ada sahip tüm kayıt kümelerini almak için tüm kayıt kümelerini almak ve sonra sonuçları filtrelemek gerekir:

```powershell
$recordsets = Get-AzDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

Tüm Yukarıdaki örneklerde, bölge kullanarak ya da belirtilebilir `-ZoneName` ve `-ResourceGroupName`(gösterildiği gibi) parametreleri veya bölge nesnesi belirterek:

```powershell
$zone = Get-AzDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzDnsRecordSet -Zone $zone
```

## <a name="add-a-record-to-an-existing-record-set"></a>Bir kaydı var olan bir kayıt kümesine ekleyin.

Var olan bir kayıt kümesine bir kayıt eklemek için aşağıdaki üç adımı izleyin:

1. Var olan kayıt kümesine Al

    ```powershell
    $rs = Get-AzDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Yeni kayıt yerel kayıt kümesine ekleyebilirsiniz. Bu bir çevrimdışı bir işlemdir.

    ```powershell
    Add-AzDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Azure DNS hizmetine değişikliği işleyin. 

    ```powershell
    Set-AzDnsRecordSet -RecordSet $rs
    ```

Kullanarak `Set-AzDnsRecordSet` *değiştirir* mevcut kayıt belirtilen kayıt kümesi ile Azure DNS (ve içerdiği tüm kayıtları) kümesi. [ETag denetimleri](dns-zones-records.md#etags) eşzamanlı değişiklikleri yazılmaz emin olmak için kullanılır. Kullanabileceğiniz isteğe bağlı `-Overwrite` bu denetimleri bastırmak için anahtar.

Bu işlemlerin sırasını da olabilir *yöneltilen*, bir parametre olarak geçirerek yerine kanal kullanarak kayıt kümesi nesnesi geçirdiğiniz anlamına gelir:

```powershell
Get-AzDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzDnsRecordSet
```

Yukarıdaki örneklerde, mevcut bir kayıt kümesine 'A' türünde bir 'A' kaydı ekleme işlemini göstermektedir. Benzer bir dizi işlem değiştirerek, diğer türlerinin kayıt kümelerine kayıt eklemek için kullanılan `-Ipv4Address` parametresinin `Add-AzDnsRecordConfig` belirli kayıt türlerinin diğer parametrelere sahip. Tüm kayıt türlerine ait parametreleri aynıdır `New-AzDnsRecordConfig` ek kayıt türü Yukarıdaki örneklerde gösterildiği gibi cmdlet'i.

'CNAME' veya 'SOA' türündeki kayıt kümesi birden fazla kayıtla içeremez. Bu kısıtlama, DNS standartları ortaya çıkar. Azure DNS bir kısıtlaması değil.

## <a name="remove-a-record-from-an-existing-record-set"></a>Mevcut bir kayıt kümesinden bir kaydı Kaldır

Kayıt kümesinden bir kaydı kaldırma işlemini işlemi var olan bir kayıt kümesine kayıt eklemeye benzer:

1. Var olan kayıt kümesine Al

    ```powershell
    $rs = Get-AzDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Kaydı yerel kayıt kümesi nesneden kaldırın. Bu bir çevrimdışı bir işlemdir. Kaldırılmakta olan kaydı varolan bir kaydı ile tam eşleşmiyorsa tüm parametreleri arasında olmalıdır.

    ```powershell
    Remove-AzDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Azure DNS hizmetine değişikliği işleyin. İsteğe bağlı `-Overwrite` bastırmak için anahtar [Etag denetler](dns-zones-records.md#etags) eş zamanlı değişiklikler.

    ```powershell
    Set-AzDnsRecordSet -RecordSet $Rs
    ```

Kayıt kümesindeki en son kaydını kaldırmak için yukarıdaki dizisi kullanarak kayıt kümesi silinmez, bunun yerine, boş bir kayıt kümesi bırakır. Bir kayıt kümesi tamamen kaldırmak için bkz: [kayıt kümesini Sil](#delete-a-record-set).

Benzer şekilde bir kayıt kümesine kayıt eklemeye kayıt kümesi kaldırmak için işlem dizisi ayrıca yöneltilebilir:

```powershell
Get-AzDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzDnsRecordSet
```

Farklı kayıt türleri, uygun türe özgü parametrelerle geçirerek desteklenir `Remove-AzDnsRecordSet`. Tüm kayıt türlerine ait parametreleri aynıdır `New-AzDnsRecordConfig` ek kayıt türü Yukarıdaki örneklerde gösterildiği gibi cmdlet'i.


## <a name="modify-an-existing-record-set"></a>Mevcut bir kayıt kümesini değiştirme

Mevcut bir kayıt kümesini değiştirme adımları ekleyerek veya kayıt kümesindeki kayıtları kaldırırken adımlar benzerdir:

1. Ayarlamak için mevcut kaydı almak `Get-AzDnsRecordSet`.
2. Yerel kayıt kümesi nesnesiyle değiştirin:
    * Ekleme veya kaldırma kayıtları
    * Var olan kayıtların parametrelerinin değiştirilmesi
    * Kayıt değiştirme meta verileri ve zamanını ayarlayıp Canlı (TTL)
3. Kullanarak değişikliklerinizi işleyin `Set-AzDnsRecordSet` cmdlet'i. Bu *değiştirir* mevcut kayıt belirtilen kayıt kümesi ile Azure DNS'de kümesi.

Kullanırken `Set-AzDnsRecordSet`, [Etag denetler](dns-zones-records.md#etags) eşzamanlı değişiklikleri yazılmaz emin olmak için kullanılır. Kullanabileceğiniz isteğe bağlı `-Overwrite` bu denetimleri bastırmak için anahtar.

### <a name="to-update-a-record-in-an-existing-record-set"></a>Var olan bir kayıt kümesine bir kaydı güncelleştirmek için

Bu örnekte biz varolan 'A' kaydı, IP adresini değiştirin:

```powershell
$rs = Get-AzDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-an-soa-record"></a>SOA kaydı değiştirmek için

Ekleyemez veya otomatik olarak oluşturulan SOA kaydı bölgenin tepesinde kümesi kayıtları kaldırmak (`-Name "@"`, tırnak işaretleri dahil olmak üzere). Ancak, herhangi bir parametre içinde SOA kaydını ("ana" dışında) değiştirebilirsiniz ve TTL kaydı ayarlayın.

Aşağıdaki örnek nasıl değiştirileceğini gösterir *e-posta* SOA kaydı özelliği:

```powershell
$rs = Get-AzDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-ns-records-at-the-zone-apex"></a>NS kayıtları bölgenin tepesindeki değiştirmek için

NS kayıt bölge tepesinde kümesi her DNS bölge ile otomatik olarak oluşturulur. Bu bölgeye atanan Azure DNS ad sunucularının adlarını içerir.

Ek ad sunucuları bu NS kayıt için birden fazla DNS sağlayıcısı ile ortak barındırma etki alanlarını destekleyecek şekilde ayarlama, ekleyebilirsiniz. Bu kayıt kümesi için meta veriler ve TTL de değiştirebilirsiniz. Ancak, kaldırın veya önceden doldurulmuş Azure DNS ad sunucularını değiştirin.

Bu bölge tepesinde yalnızca NS kayıt kümesi için geçerli olduğunu unutmayın. (Alt bölgeleri temsilci seçmek için kullanıldığı şekilde), bu bölgedeki diğer NS kayıt kümeleri, kısıtlama değiştirilebilir.

Aşağıdaki örnekte bölgenin tepesinde ayarlayın NS kaydı ek ad sunucusu ekleme gösterir:

```powershell
$rs = Get-AzDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-record-set-metadata"></a>Kayıt kümesi meta verileri değiştirmek için

[Kayıt kümesi meta verileri](dns-zones-records.md#tags-and-metadata) uygulamaya özgü verileri, anahtar-değer çiftleri olarak her bir kayıt kümesi ile ilişkilendirmek için kullanılabilir.

Aşağıdaki örnek, mevcut bir kayıt kümesi meta verileri değiştirmek gösterilmektedir:

```powershell
# Get the record set
$rs = Get-AzDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"

# Add 'dept=finance' name-value pair
$rs.Metadata.Add('dept', 'finance') 

# Remove metadata item named 'environment'
$rs.Metadata.Remove('environment')  

# Commit changes
Set-AzDnsRecordSet -RecordSet $rs
```


## <a name="delete-a-record-set"></a>Bir kayıt kümesini Sil

Kullanarak kayıt kümelerini silinebilir `Remove-AzDnsRecordSet` cmdlet'i. Kayıt kümesi sildiğinizde, kayıt kümesindeki tüm kayıtlar da silinir.

> [!NOTE]
> SOA silemezsiniz ve NS kayıt kümeleri bölgenin tepesinde (`-Name '@'`).  Azure DNS bölgesi oluşturuldu ve bölge silindiğinde bunları otomatik olarak siler. Bunlar otomatik olarak oluşturulur.

Aşağıdaki örnek, bir kayıt kümesini silmek gösterilmektedir. Bu örnekte, kayıt kümesi adını, kayıt kümesi türü, bölge adını ve kaynak grubu her açıkça belirtilir.

```powershell
Remove-AzDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Alternatif olarak, kayıt kümesi adını ve türünü ve kullanarak bir nesneyi belirtilen bölge tarafından belirtilebilir:

```powershell
$zone = Get-AzDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

Üçüncü bir seçenek olarak öğesi kendisine ayarlanmış kaydı, bir kayıt kümesi nesnesi kullanılarak belirtilebilir:

```powershell
$rs = Get-AzDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzDnsRecordSet -RecordSet $rs
```

Kayıt kümesi bir kayıt kümesi nesnesini kullanarak silinecek belirttiğinizde [Etag denetler](dns-zones-records.md#etags) eşzamanlı değişiklikleri silinmediğinden emin olmak için kullanılır. Kullanabileceğiniz isteğe bağlı `-Overwrite` bu denetimleri bastırmak için anahtar.

Kayıt kümesi nesnesi, ayrıca bir parametre olarak geçirilen yerine yöneltilebilir:

```powershell
Get-AzDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzDnsRecordSet
```

## <a name="confirmation-prompts"></a>Onay istemleri

`New-AzDnsRecordSet`, `Set-AzDnsRecordSet` ve `Remove-AzDnsRecordSet` cmdlet’lerinin tamamı onay istemlerini destekler.

Her cmdlet'i, onay ister `$ConfirmPreference` PowerShell tercih değişkeni değerine sahip `Medium` veya daha düşük. İçin varsayılan değer beri `$ConfirmPreference` olan `High`, bu komut istemlerini ayarlarıyla PowerShell kullanırken sunulmaz.

`-Confirm` parametresini kullanarak geçerli `$ConfirmPreference` ayarını geçersiz kılabilirsiniz. `-Confirm` veya `-Confirm:$True` belirtirseniz cmdlet, çalıştırılmadan önce size onay istemi görüntüler. `-Confirm:$False` belirtirseniz cmdlet size onay istemi görüntülemez. 

`-Confirm` ve `$ConfirmPreference` hakkında daha fazla bilgi için bkz. [Tercih Değişkenleri Hakkında](/powershell/module/microsoft.powershell.core/about/about_preference_variables.1).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [bölgelerini ve kayıtlarını Azure DNS'de](dns-zones-records.md).
<br>
Bilgi nasıl [bölgeleri ve kayıtlarını koruma](dns-protect-zones-recordsets.md) Azure DNS kullanırken.
<br>
Gözden geçirme [Azure DNS PowerShell başvuru belgeleri](/powershell/module/az.dns).
