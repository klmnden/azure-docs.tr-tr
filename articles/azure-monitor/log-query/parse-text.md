---
title: Azure İzleyici günlüklerine metin verileri ayrıştırmak | Microsoft Docs
description: Günlük verilerini Azure İzleyici kayıtlardaki veriler alınır ve her biri için göreli avantajları karşılaştırma sorguda alındığında ayrıştırmak için farklı seçenekler açıklanır.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/04/2018
ms.author: bwren
ms.openlocfilehash: ad4839a1b9e951a2bb206518254826a066330000
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61426740"
---
# <a name="parse-text-data-in-azure-monitor-logs"></a>Azure İzleyici günlüklerine metin verileri ayrıştırılamadı
Bazı günlük verilerini Azure İzleyici tarafından toplanan bilgilerin birden çok parça içinde tek bir özellik içerir. Bu veriler birden çok özelliklerini ayrıştırma kolaylaştıran sorguları kullanın. Bir ortak örnek bir [özel günlük](../../log-analytics/log-analytics-data-sources-custom-logs.md) , tek bir özellikte birden çok değer içeren bir tüm günlük girdisi toplar. Farklı değerleri için ayrı özellikler oluşturarak, arama yapabilirsiniz ve her toplama.

Bu makalede, Azure İzleyici'de günlük verilerini veriler alınır ve her biri için göreli avantajları karşılaştırma sorguda alındığında ayrıştırmak için farklı seçenekler açıklanır.


## <a name="parsing-methods"></a>Yöntemleri ayrıştırma
Veri toplanması, alım zamanında veya sorgu verilerini ayrıştırabilirsiniz bir sorgu ile verileri analiz edilirken zaman. Aşağıda açıklandığı gibi her stratejinin benzersiz avantajları vardır.

### <a name="parse-data-at-collection-time"></a>Veri toplama zamanında ayrıştırılamıyor
Veri toplama zamanında ayrıştırılamadı olduğunda, yapılandırdığınız [özel alanlar](../../log-analytics/log-analytics-custom-fields.md) tablosunda yeni özellikler oluşturun. Sorgular, ayrıştırma tüm mantığı içerir ve yalnızca tablodaki herhangi bir alan olarak bu özellikleri kullanmak zorunda değilsiniz.

Bu yöntemin avantajları şunlardır:

- Eklemeniz gerekmez. bu yana toplanan verileri ayrıştırmak sorgu komutlarında sorgu daha kolay.
- Sorgu beri daha iyi sorgu performansını ayrıştırma gerçekleştirmeniz gerekmez.
 
Bu yöntemin dezavantajları şunlardır:

- Önceden tanımlanmış olması gerekir. Zaten toplanmış veriler dahil edemezsiniz.
- Ayrıştırma mantığı değiştirirseniz, yalnızca yeni verilere uygulanır.
- Sorgularda kullanılabilenden daha az ayrıştırma seçenekleri.
- Veri toplamak için gecikme süresi artar.
- Hataları işlemek zor olabilir.


### <a name="parse-data-at-query-time"></a>Sorgu zamanında verileri ayrıştırılamadı
Sorgu zamanında verileri ayrıştırılamadı olduğunda, verileri birden çok alana ayrıştırmak için sorgunuzu mantığı içerir. Gerçek tablo değiştirilmez.

Bu yöntemin avantajları şunlardır:

- Zaten toplanmış veriler dahil olmak üzere herhangi bir veri geçerlidir.
- Mantığındaki değişiklikler hemen tüm veriler için uygulanabilir.
- Ayrıştırma seçeneklerini belirli veri yapıları için önceden tanımlanmış mantığı dahil esnek.
 
Bu yöntemin dezavantajları şunlardır:

- Daha karmaşık sorgular gerekir. Bu kullanılarak azaltılabilir [işlevleri bir tabloyu benzetimini yapmak için](#use-function-to-simulate-a-table).
- Birden çok sorgu ayrıştırma mantığı çoğaltılması gerekir. Mantığa işlevleri aracılığıyla paylaşabilirsiniz.
- Karmaşık mantık çok büyük kayıt karşı çalışan (kayıtları milyarlarca) belirlediğinde yükü oluşturabilirsiniz.

## <a name="parse-data-as-its-collected"></a>Verileri toplandıktan olarak ayrıştırılamadı
Bkz: [Azure İzleyici'de özel alanlar oluşturma](../platform/custom-fields.md) toplandıktan gibi veri ayrıştırma ilişkin ayrıntılar için. Bu özel özellikler sorgular gibi diğer herhangi bir özelliği tarafından kullanılan bir tablo oluşturur.

## <a name="parse-data-in-query-using-patterns"></a>Sorgu desenleri kullanarak verileri ayrıştırılamadı
Ayrıştırılacak istediğiniz verilerin kayıtlarda yinelenen bir desen tarafından belirlenebildiğinde farklı işleçleri kullanabilirsiniz [Kusto sorgu dili](/azure/kusto/query/) bir veya daha fazla yeni özellikler belirli veri parçası ayıklanamadı.

### <a name="simple-text-patterns"></a>Basit metin desenleri

Kullanım [ayrıştırma](/azure/kusto/query/parseoperator) dize ifadesinden ayıklanması gereken bir veya daha fazla özel özellikler oluşturmak için sorgu işleci. Tanımlanması için desen ve adlarını oluşturmak üzere özellikleri belirtin. Benzer şekilde bir form anahtar-değer dizeleri verilerle yararlıdır _anahtar = değer_.

Verileri aşağıdaki biçimde özel bir günlük göz önünde bulundurun.

```
Time=2018-03-10 01:34:36 Event Code=207 Status=Success Message=Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
Time=2018-03-10 01:33:33 Event Code=208 Status=Warning Message=Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
Time=2018-03-10 01:35:44 Event Code=209 Status=Success Message=Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
Time=2018-03-10 01:38:22 Event Code=302 Status=Error Message=Application could not connect to database
Time=2018-03-10 01:31:34 Event Code=303 Status=Error Message=Application lost connection to database
```

Aşağıdaki sorgu bu verileri ayrı ayrı Özellikler içinde ayrıştırma. Satırla _proje_ yalnızca dönün, hesaplanan özellikler eklenir ve _RawData_, giriş tamamına özel günlük tutan tek özellik.

```Kusto
MyCustomLog_CL
| parse RawData with * "Time=" EventTime " Event Code=" Code " Status=" Status " Message=" Message
| project EventTime, Code, Status, Message
```

Kullanıcı adı etki alanınızdaki bir UPN sonu başka bir örnek aşağıdadır _AzureActivity_ tablo.

```Kusto
AzureActivity
| parse  Caller with UPNUserPart "@" * 
| where UPNUserPart != "" //Remove non UPN callers (apps, SPNs, etc)
| distinct UPNUserPart, Caller
```


### <a name="regular-expressions"></a>Normal ifadeler
Normal bir ifade ile verilerinizi tanımlanabilir, kullanabileceğiniz [normal ifadeler kullanan işlevler](/azure/kusto/query/re2) bireysel değerlerini ayıklamak için. Aşağıdaki örnekte [ayıklamak](/azure/kusto/query/extractfunction) şekilde _UPN_ alanını _AzureActivity_ kaydeder ve sonra ayrı kullanıcıların dönün.

```Kusto
AzureActivity
| extend UPNUserPart = extract("([a-z.]*)@", 1, Caller) 
| distinct UPNUserPart, Caller
```

Etkinleştirmek için verimli büyük ölçekte, ayrıştırma Azure İzleyici re2 sürümünü normal ifadeler, benzer, ancak bazı diğer normal ifade çeşitler aynı olduğu kullanır. Başvurmak [re2 ifadesi söz dizimi](https://aka.ms/kql_re2syntax) Ayrıntılar için.


## <a name="parse-delimited-data-in-a-query"></a>Bir sorgudaki sınırlandırılmış verileri ayrıştırma
Sınırlı bir veri alanları bir CSV dosyasında virgül gibi ortak bir karakter ile ayırır. Kullanım [bölme](/azure/kusto/query/splitfunction) belirttiğiniz bir sınırlayıcıyı kullanarak sınırlandırılmış verileri ayrıştırmak için işlevi. Bu ile [genişletmek](/azure/kusto/query/extendoperator) verilerdeki tüm alanları döndürür veya çıktısında dahil edilecek her bir alanı belirtmek için işleci.

> [!NOTE]
> Bölünmüş dinamik bir nesne döndürdüğünden, sonuçları işleçler ve filtreleri kullanılacak dize gibi veri türlerini açıkça başvurusuna gerekebilir.

Özel bir günlük aşağıdaki CSV biçiminde verilerle göz önünde bulundurun.

```
2018-03-10 01:34:36, 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
2018-03-10 01:33:33, 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
2018-03-10 01:35:44, 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
2018-03-10 01:38:22, 302,Error,Application could not connect to database
2018-03-10 01:31:34, 303,Error,Application lost connection to database
```

Aşağıdaki sorguda bu verileri ayrıştırmak ve ancak iki hesaplanan özellikleri özetler. İlk satırı böler _RawData_ özelliğine bir dize dizisi. Sonraki satırların her biri ayrı ayrı özellikler için bir ad verir ve uygun veri türüne dönüştürülecek işlevleri'ni kullanarak çıkış ekler.

```Kusto
MyCustomCSVLog_CL
| extend CSVFields  = split(RawData, ',')
| extend EventTime  = todatetime(CSVFields[0])
| extend Code       = toint(CSVFields[1]) 
| extend Status     = tostring(CSVFields[2]) 
| extend Message    = tostring(CSVFields[3]) 
| where getyear(EventTime) == 2018
| summarize count() by Status,Code
```

## <a name="parse-predefined-structures-in-a-query"></a>Önceden tanımlanmış bir sorgu yapılarda Ayrıştır
Verilerinizi bilinen bir yapıda biçimlendirilmişse işlevlerin kullanmayı mümkün olabilir [Kusto sorgu dili](/azure/kusto/query/) önceden tanımlanmış yapıları ayrıştırmak için:

- [JSON](/azure/kusto/query/parsejsonfunction)
- [XML](/azure/kusto/query/parse-xmlfunction)
- [IPv4](/azure/kusto/query/parse-ipv4function)
- [URL](/azure/kusto/query/parseurlfunction)
- [URL sorgusu](/azure/kusto/query/parseurlqueryfunction)
- [Dosya yolu](/azure/kusto/query/parsepathfunction)
- [Kullanıcı Aracısı](/azure/kusto/query/parse-useragentfunction)
- [Sürüm dizesi](/azure/kusto/query/parse-versionfunction)

Aşağıdaki örnek sorgu ayrıştırır _özellikleri_ alanını _AzureActivity_ JSON'da yapılandırılmış tablo. Adlı bir dinamik özellik sonuçlarını kaydeder _parsedProp_, kişi adlı bir JSON değeri içerir. Bu değerler, filtreleme ve sorgu sonuçları özetlemek için kullanılır.

```Kusto
AzureActivity
| extend parsedProp = parse_json(Properties) 
| where parsedProp.isComplianceCheck == "True" 
| summarize count() by ResourceGroup, tostring(parsedProp.tags.businessowner)
```

Sorgunuzu birden çok özelliklerinden biçimlendirilmiş veri kullandığında, kullanılması gereken şekilde ayrıştırma bu işlevler, yoğun işlemci olabilir. Aksi takdirde, işleme basit desen, daha hızlı olacaktır.

Aşağıdaki örnek, etki TGT ön kimlik doğrulama türü dökümünü gösterir. XML dizesi olan yalnızca EventData alanda türü var, ancak hiçbir diğer veriler bu alan gereklidir. Bu durumda, [ayrıştırma](/azure/kusto/query/parseoperator) gerekli veri parçasını almak için kullanılır.

```Kusto
SecurityEvent
| where EventID == 4768
| parse EventData with * 'PreAuthType">' PreAuthType '</Data>' * 
| summarize count() by PreAuthType
```

## <a name="use-function-to-simulate-a-table"></a>İşlev bir tablo benzetimini yapmak için kullanın
Belirli bir tablonun aynı ayrıştırma gerçekleştirmek birden fazla sorgu gerekebilir. Bu durumda, [bir işlev oluşturma](functions.md) her sorgu ayrıştırma mantık çoğaltmak yerine Ayrıştırılan verileri döndürür. Ardından, diğer sorgulara işlev diğer adı yerine özgün tabloyu kullanabilirsiniz.

Yukarıdaki virgülle ayrılmış özel günlük örneği göz önünde bulundurun. Bunlar Ayrıştırılan verilerde birden çok sorguları kullanmak için aşağıdaki sorguyu kullanarak bir işlev oluşturun ve diğer adla Kaydet _MyCustomCSVLog_.

```Kusto
MyCustomCSVLog_CL
| extend CSVFields = split(RawData, ',')
| extend DateTime  = tostring(CSVFields[0])
| extend Code      = toint(CSVFields[1]) 
| extend Status    = tostring(CSVFields[2]) 
| extend Message   = tostring(CSVFields[3]) 
```

Şimdi diğer kullanabilirsiniz _MyCustomCSVLog_ sorgularda aşağıdaki gibi gerçek tablo adı yerine.

```Kusto
MyCustomCSVLog
| summarize count() by Status,Code
```


## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum sorguları](log-query-overview.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için.