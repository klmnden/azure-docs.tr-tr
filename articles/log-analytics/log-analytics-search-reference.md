---
title: "Azure günlük analizi Arama başvurusu | Microsoft Docs"
description: "Günlük Arama başvurusu arama dili açıklar ve genel sorgu sözdizimi seçenekleri sağlar analizi kullanılacağı verileri için arama ve filtreleme aramanızı daraltmak yardımcı olmak üzere ifadeler."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 402615a2-bed0-4831-ba69-53be49059718
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 264ea071dc0b15964af07c68cbf0dee896b07a3e
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="log-analytics-search-reference"></a>Günlük analizi başvuru arama

>[!NOTE]
> Bu makalede eski sorgu dili günlük analizi kullanarak günlük aramaları açıklanmaktadır.  Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), için başvurmalıdır sonra [yeni dil için dil başvurusu](https://go.microsoft.com/fwlink/?linkid=856079).

Şu bölüme arama dili hakkında ne zaman kullanabilir genel sorgu sözdizimi seçenekleri açıklar verileri için arama ve filtreleme aramanızı daraltmak yardımcı olmak üzere ifadeler. Ayrıca, alınan veri eyleme geçmek için kullanabileceğiniz komutlar anlatır.

Aramalarda dönen alanları hakkında bilgi edinebilirsiniz ve veri benzer kategorileri hakkında daha fazla keşfetmesine yardımcı modelleri içinde [arama alanı ve model başvuru bölüm](#search-field-and-facet-reference).

## <a name="general-query-syntax"></a>Genel sorgu söz dizimi
Genel sorgulamak için sözdizimi aşağıdaki gibidir:

```
filterExpression | command1 | command2 …
```

Filtre ifadesi (`filterExpression`) "where" için sorgu koşul tanımlar. Sorgu tarafından döndürülen sonuçlar komutları uygulamak. Birden çok komut dikey çizgi karakterinden (|) tarafından ayrılmış olması gerekir.

### <a name="general-syntax-examples"></a>Genel sözdizimi örnekleri
Örnekler:

```
system
```

Bu sorgunun döndürdüğü sözcüğünü içeren sonuçları *sistem* için tam metin dizinli veya arama koşulları herhangi bir alanda.

> [!NOTE]
> Bu şekilde tüm alanlar dizini, ancak en sık kullanılan metinsel alanlar (açıklamaları ve adları gibi) genellikle şunlardır.
>
>

```
system error
```

Sözcükler içeren sonuçları bu sorgunun döndürdüğü *sistem* ve *hata*.

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

Sözcükler içeren sonuçları bu sorgunun döndürdüğü *sistem* ve *hata*. Ardından sonuçları göre sıralar *ManagementGroupName* (artan düzende), alan ve sonra *TimeGenerated* alanındaki (Azalan). Yalnızca ilk 10 sonuçlarını alır.

> [!IMPORTANT]
> Tüm alan adları ve değerleri dize ve metin alanları için büyük/küçük harfe duyarlıdır.
>
>

## <a name="filter-expressions"></a>Filtre ifadeleri
Aşağıdaki alt bölümlerde filtre ifadeleri açıklanmıştır.

### <a name="string-literals"></a>Dize değişmez değerleri
Bir dize sabit değeri bir anahtar sözcük veya önceden tanımlanmış veri türü (örneğin, bir sayı veya tarih) ayrıştırıcı tarafından tanınmıyor herhangi bir dize ' dir.

Örnekler:

```
These all are string literals
```

Bu sorgu için tüm beş sözcükleri oluşumları içeren sonuçları arar. Karmaşık dize araması yapmak için dize sabit değeri tırnak işaretleri içine alın. Örneğin:

```
"Windows Server"
```

Bu yalnızca tam eşleşmeleri olan sonuçlar döndürür *Windows Server*.

### <a name="numbers"></a>Sayılar
Ayrıştırıcının sayısal alanlar için ondalık tamsayı ve kayan noktalı sayı sözdizimini destekler.

Örnekler:

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="dates-and-times"></a>Tarihler ve saatler
Her bir veri sisteminde parçası olan bir *TimeGenerated* özgün tarih ve saat kaydının temsil eden özellik. Bazı veri türleri ek tarih ve saat alanları olabilir (örneğin, *LastModified*).

Zaman Çizelgesi **grafik/saat** Azure günlük analizi seçicide sonuçlar dağıtılması (göre çalıştırılan geçerli sorgu) zamanla gösterilir. Bu temel alır *TimeGenerated* alan. Tarih ve saat alanları sorgularında belirli bir zaman çerçevesi sorgusunu sınırlamak için kullanılan bir belirli dize biçimine sahiptir. Sözdizimi, göreli zaman aralıkları (örneğin, "arasındaki 3 gün önce 2 saat önce") başvurmak için de kullanabilirsiniz.

Geçerli tür sözdizimi tarihler ve saatler için şunlardır:

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


Örneğin:

```
TimeGenerated:2013-10-01T12:20
```

Önceki komutu yalnızca kayıtları döndüren bir *TimeGenerated* değeri tam olarak 12:20 1 Ekim 2013.

Ayrıştırıcının anımsatıcı tarih/saat değeri şimdi de destekler. (Veri ile bu hızlı sistemin yapmaz olduğundan, bu herhangi bir sonuç sunacak düşüktür.)

Bu örnek için mutlak tarihleri kullanmak için yapı taşları verilebilir. Sonraki üç alt bölümlerde, göreli bir tarih aralıkları kullanmanız örnekleriyle daha gelişmiş filtreleri kullanmak nasıl görürsünüz.

### <a name="datetime-math"></a>Matematik tarih
Uzaklık ya da basit tarih hesaplamaları kullanarak bir tarih/saat değeri yuvarlamak için tarih matematik işleçleri kullanın.

Sözdizimi:

```
datetime/unit
```

```
datetime[+|-]count unit
```

| işleci | Açıklama |
| --- | --- |
| / |Tarih için belirtilen birim yuvarlar. Örneğin, şimdi / gün geçerli günün gece yarısına geçerli tarih/saat yuvarlar. |
| + veya - |Tarih/Saat birimleri belirtilen sayıda tarafından kaydırır. Örneğin, şimdi + 1 saat kaydırır geçerli tarih/saat şimdi bir saat. 2013-10-01T12:00-10 gün tarih değerini 10 gün tarafından yeniden kaydırır. |

Tarih/saat matematik işleçleri birlikte zincir. Örneğin:

```
NOW+1HOUR-10MONTHS/MINUTE
```

Aşağıdaki tabloda, desteklenen tarih/saat birimleri listeler.

| Tarih/saat birim | Açıklama |
| --- | --- |
| YIL, YIL |Geçerli yıl yuvarlar veya belirtilen yıl sayısı kadar kaydırır. |
| AY, AY |Geçerli ay yuvarlar veya belirtilen ay sayısı kadar kaydırır. |
| GÜN, TARİH |Geçerli ayın yuvarlar veya belirtilen gün sayısı tarafından kaydırır. |
| SAAT, SAAT |Geçerli saat yuvarlar veya tarafından belirtilen sayıda saat kaydırır. |
| DAKİKA, DAKİKA |Geçerli dakikada yuvarlar veya belirtilen dakika sayısı kadar kaydırır. |
| İKİNCİ OLARAK, SANİYE |Geçerli ikinci yuvarlar veya belirtilen sayıda saniye tarafından kaydırır. |
| MİLİSANİYE, MİLİSANİYE CİNSİNDEN MİLİSANİYE, MILLIS |Geçerli saniyeye yuvarlar veya belirtilen milisaniye sayısı kadar kaydırır. |

### <a name="field-facets"></a>Alan modelleri
Alan modelleri kullanarak, belirli alanlar ve tam değerlerine için arama koşulu belirtebilirsiniz. Bu dizin boyunca çeşitli koşullar için "serbest metin" sorguları yazma farklıdır. Bu teknik birkaç önceki örneklerde zaten gördünüz. Daha karmaşık örnekleri aşağıda verilmiştir.

**Sözdizimi**

```
field:value
```

```
field=value
```

**Açıklama**

Belirli değer alanını arar. Değer bir dize sabit değeri, sayı veya tarih ve saat olabilir.

Örneğin:

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

**Sözdizimi**

*alan > değeri*

*alan < değeri*

*alan > = değer*

*alan < value =*

*alan! = değer*

**Açıklama**

Karşılaştırmaları sağlar.

Örneğin:

```
TimeGenerated>NOW+2HOURS
```

**Sözdizimi**

```
field:[from..to]
```

**Açıklama**

Aralık olduğunu sağlar.

Örneğin:

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="in"></a>IN
**IN** anahtar sözcüğü değerler listesinden seçmenize olanak sağlar. Kullandığınız sözdizimi bağlı olarak, bu basit sağladığınız değerler listesini ya da bir toplama değerleri listesi olabilir.

Sözdizimi 1:

```
field IN {value1,value2,value3,...}
```

Bu sözdizimi, basit bir listedeki tüm değerlerin içermek üzere sağlar.



Örnekler:

```
EventID IN {1201,1204,1210}
```

```
Computer IN {"srv01.contoso.com","srv02.contoso.com"}
```

Sözdizimi 2:

```
(Outer query) (Field to use with inner query results) IN {Inner query | measure count() by (Field to send to outer query)} (rest  of outer query)  
```

Bu söz dizimi, bir toplama oluşturmanıza olanak sağlar. Ardından, bu toplama bu değerle olaylarını arar başka bir dış (birincil) arama içine gelen değerler listesi akış. İç arama ayraçlar içinde kapsayan ve ın işlecini kullanarak bir alan dış arama için olası değerler olarak sonuçlarını besleme yapın.

İç sorgu örnek: *şu anda güvenlik güncelleştirmeleri eksik olan bilgisayarlar* aşağıdaki toplama sorguyla:

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

Bulur son sorgu *şu anda güvenlik güncelleştirmeleri eksik olan bilgisayarlar için tüm Windows olayları* aşağıdakine benzer:

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="contains"></a>Contains
**İçerir** anahtar sözcüğü, belirtilen dizeyi içeren bir alanla kayıtlar için filtre olanak sağlar. Bu büyük küçük harfe duyarlıdır, yalnızca dize alanları ile çalışır ve herhangi bir kaçış karakteri içeremez.

Sözdizimi:

```
field:contains("string")
```

Örnek:

```
Type:contains("Event")
```

Bu, "Olay" dizesini içeren bir türü kayıtlarıyla döndürür. Örnekler **olay**, **SecurityEvent**, ve **ServiceFabricOperationEvent**.



### <a name="regular-expressions"></a>Normal ifadeler
Kullanarak bir normal ifade ile bir alan için bir arama koşulu belirtebilirsiniz **Regex** anahtar sözcüğü. Normal ifadelerde kullanabileceğiniz sözdizimi tam bir açıklaması için bkz: [günlük analizi günlük aramalarda filtrelemek için normal ifadeler kullanarak](log-analytics-log-searches-regex.md).

Sözdizimi:

```
field:Regex("Regular Expression")
```

Örnek:

```
Computer:Regex("^C.*")
```

### <a name="logical-operators"></a>Mantıksal işleçler
Mantıksal işleçler sorgu dili destekler (*ve*, *veya*, ve *değil*) ve C tarzı benzersizse (*&&*,  *||* , ve *!*sırasıyla). Bu işleçlere gruplandırmak için parantez kullanabilirsiniz.

Örnekler:

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

Üst düzey filtre bağımsız değişkenler için mantıksal işleç atlayabilirsiniz. Bu durumda, AND işleci varsayılır.

| Filtre ifadesi | Eşdeğer |
| --- | --- |
| Sistem hatası |Sistem ve hata |
| Sistem "Windows Server" veya önem derecesi: 1 |Sistem ve ("Windows Server" veya önem derecesi: 1) |

### <a name="wildcarding"></a>Joker karakterler
Sorgu dili kullanarak destekler ( \* ) sorguda bir değer için bir veya daha fazla karakteri temsil etmesi için.

Örnek:

 "Redmond-SQL" gibi adında "SQL" içeren tüm bilgisayarları bulur.

```
Type=Event Computer=*SQL*
```

> [!NOTE]
> Şu anda tırnak işaretleri joker karakterler kullanılamaz. Örneğin, ileti `"*This text*"` göz önünde bulundurur (\*) sabit değer olarak kullanılan (\*) karakter.


## <a name="commands"></a>Komutlar


Sorgu tarafından döndürülen sonuçları komutları uygulamak. Bir komut alınan sonuçları uygulamak için dikey çizgi karakterinden (|) kullanın. Birden çok komut dikey çizgi karakteriyle ayrılmış olması gerekir.

> [!NOTE]
> Komut adları büyük harf ya da alan adları ve veri aksine, küçük harf yazılabilir.
>
>

### <a name="sort"></a>Sırala
Sözdizimi:

    sort field1 asc|desc, field2 asc|desc, …

Sonuçları belirli alanlara göre sıralar. Artan veya azalan düzende sonuçları sıralamak için asc/desc sonek isteğe bağlıdır. Belirtilmezse, *asc* sıralama düzeni varsayılır. İçin **TimeGenerated** alanı *desc* sıralama düzeni varsayılır, varsayılan olarak en son sonuçları ilk döndürecek şekilde.

### <a name="toplimit"></a>Üst/sınırı
Sözdizimi:

    top number


    limit number
İlk N sonuçları yanıtı sınırlar.

Örnek:

    Type:Alert errors detected | top 10

En iyi 10 eşleşen sonuç döndürür.

### <a name="skip"></a>Atla
Sözdizimi:

    skip number

Listelenen sonuç sayısını atlar.

Örnek:

    Type:Alert errors detected | top 10 | skip 200

200 sonucunda başlangıç üst 10 eşleşen sonuç döndürür.

### <a name="select"></a>Şunu seçin:
Sözdizimi:

    select field1, field2, ...

Seçtiğiniz alan sonuçlarını sınırlandırır.

Örnek:

    Type:Alert errors detected | select Name, Severity

Döndürülen sonuçlar alanları sınırlar *adı* ve *önem*.

### <a name="measure"></a>Ölçü birimi
*Ölçü* komutu ham arama sonuçlarını istatistik işlevleri uygulamak için kullanılır. Bu almak çok kullanışlıdır *grubu tarafından* görünümleri veriler üzerinde. Ölçü komutunu kullandığınızda, günlük analizi arama toplanmış sonuçlarını içeren bir tablo görüntüler.

**Sözdizimi:**

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



Sonuçlarına göre toplayan *grup alanı*ve toplanan ölçüm değerlerini kullanarak hesaplar *aggregatedField*.

| Ölçü istatistiksel işlevi | Açıklama |
| --- | --- |
| *aggregateFunction* |Toplama işlevi (büyük küçük harfe duyarlı) adı. Aşağıdaki toplama işlevleri desteklenir: COUNT, MAX, MIN, SUM, AVG, STDDEV, COUNTDISTINCT, yüzdebirlik ## veya PCT ## (## herhangi bir sayı 1 ile 99 arasında). |
| *aggregatedField* |Toplanmakta alan. Bu alan sayısı toplama işlevi için isteğe bağlıdır, ancak SUM, MAX, MIN, AVG, STDDEV, yüzdebirlik ## veya PCT ## için varolan bir sayısal alana olmalıdır (## herhangi bir sayı 1 ile 99 arasında). AggregatedField herhangi birini de olabilir **Genişlet** işlevleri desteklenir. |
| *fieldAlias* |(İsteğe bağlı) diğer hesaplanan değer birleştirilir. Belirtilmezse, alan adıdır **AggregatedValue**. |
| *Grup alanı* |Sonuç kümesi alanın adını göre gruplandırılır. |
| *Aralığı* |Zaman aralığındaki biçimi:**nnnNAME**. **nnn**pozitif bir tamsayı değil. **AD** aralığı adıdır. Desteklenen aralık adları büyük küçük harfe duyarlı bağlıdır ve şunları içerir: MİLİSANİYE [S] ikinci [S] DAKİKADA [S], saat [S] günü [S], [S] ay ve yıl [S]. |

Aralık seçeneği yalnızca tarih/saat grubu alanlarında kullanılabilir (gibi *TimeGenerated* ve *TimeCreated*). Şu anda, bu hizmet tarafından zorlanmaz ancak arka ucuna geçirilen tarih/saat olmayan bir alan bir çalışma zamanı hatası neden olur. Şema doğrulaması uygulandığında hizmeti API'si için aralığı toplama alanları tarih olmadan kullanan sorguları reddeder. Geçerli *ölçü* uygulaması herhangi bir toplama işlevi için aralığı gruplandırma destekler.

BY yan tümcesi girilmezse, ancak bir zaman aralığı (bir ikinci sözdizimi), belirtilen *TimeGenerated* alan varsayılan olarak kabul edilir.

Örnekler:

**Örnek 1**

    Type:Alert | measure count() as Count by ObjectId

Uyarıları göre gruplandırır *objectID*ve her grup için uyarı sayısını hesaplar. Toplu değer olarak döndürülür *sayısı* alan (diğer ad).

**Örnek 2**

    Type:Alert | measure count() interval 1HOUR

Uyarıları kullanarak 1 saat aralıklarına göre gruplandırır *TimeGenerated* alan ve her aralığa uyarıların sayısını döndürür.

**Örnek 3**

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

Önceki örnekte, ancak toplu alan diğer ad ile aynı (*AlertsPerHour*).

**Örnek 4**

    * | Ölçü count() TimeCreated aralığı 5DAYS tarafından

Kullanarak sonuçları 5 gün aralıklarına göre gruplandırır *TimeCreated* alan ve her aralığa sonuç sayısını döndürür.

**Örnek 5**

    Type:Alert | measure max(Severity) by WorkflowName

Uyarıları iş yükü ada göre gruplandırır ve her bir iş akışı için en fazla uyarı önem derecesi değeri döndürür.

**Örnek 6**

    Type:Alert | measure min(Severity) by WorkflowName

Önceki örnekte, ancak ile aynı *min* işlevi bir araya getirilir.

**Örnek 7**

    Type:Perf | measure avg(CounterValue) by Computer

Bilgisayar tarafından Perf grupları ve ortalama (Ort) hesaplar.

**Örnek 8**

    Type:Perf | measure sum(CounterValue) by Computer

Önceki örnekte, aynı ancak kullanan *toplam*.

**Örnek 9**

    Type:Perf | measure stddev(CounterValue) by Computer

Önceki örnekte, aynı ancak kullanan *stddev*.

**Örnek 10**

    Type:Perf | measure percentile70(CounterValue) by Computer

Önceki örnekte, aynı ancak kullanan *percentile70*.

**Örnek 11**

    Type:Perf | measure pct70(CounterValue) by Computer

Önceki örnekte, aynı ancak kullanan *pct70*. Unutmayın *PCT ##* yalnızca için diğer ad olduğu *yüzdebirlik ##* işlevi.

**Örnek 12**

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

Perf ilk bilgisayar tarafından grupları ve ardından CounterName ve ortalama (Ort) hesaplar.

**Örnek 13**

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

En fazla uyarı sayısı üst beş akışlarıyla alır.

**Örnek 14**

    * | countdistinct(Computer) türüne göre ölçün

Her tür için raporlama benzersiz bilgisayar sayısını sayar.

**Örnek 15**

    * | Ölçü countdistinct(Computer) aralığı 1 saat

Her saat için raporlama benzersiz bilgisayar sayısını sayar.

**Örnek 16**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

% İşlemci zamanı bilgisayara göre gruplandırır ve her saat için ortalamasını döndürür.

**Örnek 17**

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

Yöntemi tarafından W3CIISLog grupları ve en fazla 5 dakikada döndürür.

**Örnek 18**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

% İşlemci zamanı bilgisayar grupları ve her saat için minimum ve ortalama, 75. yüzdebirlik ve maksimum döndürür.

**Örnek 19**

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

% İşlemci zamanı ilk bilgisayar tarafından ardından örnek adını göre gruplandırır ve her saat için minimum ve ortalama, 75. yüzdebirlik ve maksimum döndürür.

**Örnek 20**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

Bilgisayarınızda disk yazma işlemleri her disk için dakika başına maksimum hesaplar.

### <a name="where"></a>Burada
Sözdizimi:

```
**where** AggregatedValue>20
```

Yalnızca sonra kullanılabilir bir *ölçü* daha fazla toplanmış filtrelemek için komutu sonuçları *ölçü* toplama işlevine üretilen.

Örnekler:

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)



### <a name="dedup"></a>Yinelenenleri kaldırma
Sözdizimi:

    Dedup FieldName

Belirtilen alan için her benzersiz değer bulunan ilk belgeyi döndürür.

Örnek:

    Type=Event | Dedup EventID | sort TimeGenerated DESC

Bu örnek olay kimliği başına bir olay (en son olay) döndürür.

### <a name="join"></a>Birleştir
Tek bir sonuç kümesi oluşturmak için iki sorguların sonuçlarını birleştirir.  İzleme tabloda birden fazla birleştirme türü destekler.

| Katılım Türü | Açıklama |
|:--|:--|
| İç | Yalnızca kayıtları eşleşen değere sahip iki sorgularda döndür. |
| Dış | Tüm kayıtları hem sorgularından döndür.  |
| Sol  | Sol sorgu ve eşleşen kayıtları doğru sorgudan tüm kayıtları döndürür. |


- Birleştirmeler şu anda desteklemediği içeren sorgularını **IN** anahtar sözcüğü, **ölçü** komut veya **Genişlet** alana sağ sorgudan hedefler komutu.
- Bu gibi durumlarda, yalnızca tek bir alan şu anda bir birleştirme ekleyebilirsiniz.
- Tek bir arama birden fazla birleşim içermeyebilir.

**Sözdizimi**

```
<left-query> | JOIN <join-type> <left-query-field-name> (<right-query>) <right-query-field-name>
```

**Örnekler**

Farklı bir birleştirme türü göstermek için bir veri türü birleştirme MyBackup_CL her bilgisayar için sinyal ile adlı özel bir günlük toplanan göz önünde bulundurun.  Bu veri türleri, aşağıdaki veri içeriyor.

`Type = MyBackup_CL`

| TimeGenerated | Bilgisayar | LastBackupStatus |
|:---|:---|:---|
| 20/4/2017 01:26:32.137 AM | Srv01.contoso.com | Başarılı |
| 20/4/2017 02:13:12.381 AM | SRV02.contoso.com | Başarılı |
| 20/4/2017 02:13:12.381 AM | srv03.contoso.com | Hata |

`Type = Hearbeat`(Yalnızca bir alt kümesini gösterilen alanları)

| TimeGenerated | Bilgisayar | ComputerIP |
|:---|:---|:---|
| 21/4/2017 12:01:34.482 PM | Srv01.contoso.com | 10.10.100.1 |
| 21/4/2017 12:02:21.916 PM | SRV02.contoso.com | 10.10.100.2 |
| 21/4/2017 12:01:47.373 PM | srv04.contoso.com | 10.10.100.4 |

#### <a name="inner-join"></a>iç birleşim

`Type=MyBackup_CL | join inner Computer (Type=Heartbeat) Computer`

Aşağıdaki kayıtları bilgisayar alanı hem de veri türleri için eşleştiği döndürür.

| Bilgisayar| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 20/4/2017 01:26:32.137 AM | Başarılı | 21/4/2017 12:01:34.482 PM | 10.10.100.1 | Sinyal |
| SRV02.contoso.com | 20/4/2017 02:13:12.381 AM | Başarılı | 21/4/2017 12:02:21.916 PM | 10.10.100.2 | Sinyal |


#### <a name="outer-join"></a>dış birleşim

`Type=MyBackup_CL | join outer Computer (Type=Heartbeat) Computer`

Her iki veri türleri için aşağıdaki kayıtları döndürür.

| Bilgisayar| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 20/4/2017 01:26:32.137 AM | Başarılı  | 21/4/2017 12:01:34.482 PM | 10.10.100.1 | Sinyal |
| SRV02.contoso.com | 20/4/2017 02:14:12.381 AM | Başarılı  | 21/4/2017 12:02:21.916 PM | 10.10.100.2 | Sinyal |
| srv03.contoso.com | 20/4/2017 01:33:35.974 AM | Hata  | 21/4/2017 12:01:47.373 PM | | |
| srv04.contoso.com |                           |          | 21/4/2017 12:01:47.373 PM | 10.10.100.2 | Sinyal |



#### <a name="left-join"></a>Sol birleştirme

`Type=MyBackup_CL | join left Computer (Type=Heartbeat) Computer`

Aşağıdaki kayıtları MyBackup_CL sinyal eşleşen tüm alanları ile döndürür.

| Bilgisayar| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 20/4/2017 01:26:32.137 AM | Başarılı | 21/4/2017 12:01:34.482 PM | 10.10.100.1 | Sinyal |
| SRV02.contoso.com | 20/4/2017 02:13:12.381 AM | Başarılı | 21/4/2017 12:02:21.916 PM | 10.10.100.2 | Sinyal |
| srv03.contoso.com | 20/4/2017 02:13:12.381 AM | Hata | | | |


### <a name="extend"></a>Genişletme
Çalışma zamanı alanları içinde sorgu oluşturmanıza olanak sağlar. Çalışma zamanı alanlar ölçü komutuyla toplama gerçekleştirmek için kullanılamaz unutmayın.

**Örnek 1**

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
Gösterir SQL değerlendirmesi önerileri için öneri puan ağırlıklı.

**Örnek 2**

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
Sayaç değeri içinde KB yerine bayt gösterir.

**Örnek 3**

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
0 ile 100 arasında olduğuna gibi tüm sonuçları WireData TotalBytes değerini ölçeklendirir.

**Örnek 4**

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
Yüzde 50 olarak düşük ve diğerleri olarak yüksek değerinden performans sayacı değerlerini etiketler.

**Örnek 5**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
Bilgisayarınızda disk yazma işlemleri her disk için dakika başına maksimum hesaplar.

**Desteklenen işlevler**

| İşlevi | Açıklama | Sözdizimi örnekleri |
| --- | --- | --- |
| Abs |Belirtilen değer ya da işlevi mutlak değerini döndürür. |`abs(x)` <br> `abs(-5)` |
| ACOS |Bir değer veya bir işlev ARC kosinüsünü döndürür. |`acos(x)` |
| ve |Tüm işlenenleri true olarak değerlendirmek ve yalnızca, true değerini döndürür. |`and(not(exists(popularity)),exists(price))` |
| asin |Bir değer veya bir işlev ARC sinüsünü döndürür. |`asin(x)` |
| atan |Bir değer veya bir işlev ARC tanjantını döndürür. |`atan(x)` |
| ATAN2 |Dikdörtgen koordinatları dönüştürme işlemini kaynaklanan olan açıyı döndürür x, y Kutupsal koordinatları. |`atan2(x,y)` |
| cbrt |Küp kökü. |`cbrt(x)` |
| ceil |Bir tamsayıya yuvarlanır. |`ceil(x)`  <br> `ceil(5.6)`6'yı döndürür |
| cos |Bir açının kosinüsünü döndürür. |`cos(x)` |
| COSH |Bir açının hiperbolik kosinüsünü döndürür. |`cosh(x)` |
| def |Varsayılan kısaltması. Alan "alanını" değerini döndürür. Alan mevcut değilse, belirtilen varsayılan değerini döndürür ve ilk değer verir nerede: `exists()==true`. |`def(rating,5)`. Bu def() işlevi derecelendirme döndürür veya derecelendirmesi belgede belirtilmişse, 5 döndürür. <br> `def(myfield, 1.0)`eşdeğer olan `if(exists(myfield),myfield,1.0)`. |
| derece |Radyan cinsinden dereceye dönüştürür. |`deg(x)` |
| div |`div(x,y)`böler y x. |`div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| Dağıtım |İki vektörüne (nokta) n boyutlu bir alanda arasındaki uzaklığı döndürür. Güç artı iki veya daha çok, ValueSource örneği alır ve iki vektörü arasındaki uzaklıkları hesaplar. Her ValueSource bir sayı olmalıdır. Geçirilen ValueSource örneklerinin bir çift sayı olmalıdır ve ilk yarı ilk vektör temsil eder ve ikinci yarısındaki temsil ikinci vektör yöntemi varsayar. |`dist(2, x, y, 0, 0)`(0,0) arasındaki Euclidean uzaklığı hesaplar ve (x, y) her belge için. <br> `dist(1, x, y, 0, 0)`(0,0) arasındaki Manhattan (taxicab) uzaklığı hesaplar ve (x, y) her belge için. <br> `dist(2,,x,y,z,0,0,0)`(0,0,0) arasındaki Euclidean uzaklığı ve (x, y, z) her belge için.<br>`dist(1,x,y,z,e,f,g)`Arasındaki Manhattan uzaklığı (x, y, z) ve (e, f, g), burada her harf, bir alan adı. |
| var |Alan varsa TRUE üyesi döndürür bulunmaktadır. |`exists(author)`"Yazar" alanda bir değere sahip herhangi bir belgeyi TRUE döndürür.<br>`exists(query(price:5.00))`"Fiyat" eşleşirse, TRUE döndürür "5.00". |
| exp |Euler'ın sayı x kuvvetini döndürür. |`exp(x)` |
| Kat |Tamsayıya yuvarlar kapalı. |`floor(x)`  <br> `floor(5.6)`5 döndürür |
| hypo |Ara taşması veya underflow olmadan Sqrt(SUM(pow(x,2),pow(y,2))) döndürür. |`hypo(x,y)`  <br> ` |
| Eğer |Koşullu işlevi sorguları sağlar. İçinde `if(test,value1,value2)`, test olduğundan veya bir mantıksal değer veya bir mantıksal bir değer (TRUE veya FALSE) döndürür ifade başvuruyor. `value1`Test TRUE döndürürse değer işlev tarafından döndürülür. `value2`Test FALSE döndürürse değer işlev tarafından döndürülür. Bir ifade, Boole değerleri çıkarır herhangi bir işlev olabilir. Ayrıca, durum değeri 0 false olarak yorumlanır veya dizeler, döndürme case hangi boş dize false olarak yorumlanır sayısal değerler döndüren bir işlev olabilir. |`if(termfreq(cat,'electronics'),popularity,42)`Bu işlev her belge kat alanında "elektronik" terimi içerip içermediğini denetler. Aşması durumunda popülerliği alanının değeri döndürülür. Aksi takdirde, 42 değeri döndürülür. |
| Doğrusal |Implements `m*x+c`, burada m ve c sabitleri ve x, rasgele bir işlev. Bu eşdeğer olan `sum(product(m,x),c)`, ancak tek bir işlevi uygulanan biraz daha verimlidir. |`linear(x,m,c) linear(x,2,4)`döndürür`2*x+4` |
| ln |Belirtilen işlev doğal günlüğü döndürür. |`ln(x)` |
| Günlük |Belirtilen işlev temel 10 günlük döndürür. |`log(x)   log(sum(x,100))` |
| eşleme |Min ve Mak, belirtilen hedef kapsayıcı içinde kalan herhangi bir giriş işlevi x değerleri eşler. Bağımsız değişkenler min ve Mak olmalıdır. Bağımsız değişkenler hedef ve varsayılan sabitler veya İşlevler olabilir. X değerini min ve max arasında uymazsa sonra x değeri döndürülür ya da 5 bağımsız değişkeni olarak belirtilen bir varsayılan değeri döndürülür. |`map(x,min,max,target) map(x,0,0,1)`0 ve 1 tüm değerleri değiştirir. Bu, varsayılan 0 değerleri işleme yararlı olabilir.<br> `map(x,min,max,target,default)    map(x,0,100,1,-1)`0 ile 100 için 1 arasında herhangi bir değere ve diğer tüm değerler -1 olarak değiştirir.<br>  `map(x,0,100,sum(x,599),docfreq(text,solr))`Tüm değerleri 0 ile 100 x + 599 ve diğer tüm değerler arasındaki alan metni ' solr' terimi sıklığını değiştirir. |
| max |Birden çok iç içe geçmiş işlevler veya bağımsız değişken olarak belirtilmiş sabitleri en büyük sayısal değerini döndürür: `max(x,y,...)`. Max işlevi de "başka bir işlev bottoming" için yararlı olabilir veya bazı alan sabiti belirtildi.  Kullanım `field(myfield,max)` tek değerli bir alanı en büyük değerini seçmek için sözdizimi. |`max(myfield,myotherfield,0)` |
| dk |Bağımsız değişken olarak belirtilmiş sabitlerin birden çok iç içe geçmiş işlevler en küçük sayısal değeri döndürür: `min(x,y,...)`. Min işlevi de "üst sınırı" bir sabit kullanarak bir işlev sağlamak için yararlı olabilir. Kullanım `field(myfield,min)` tek değerli bir alanı en küçük değeri seçmek için sözdizimi. |`min(myfield,myotherfield,0)` |
| mod |Modulus işlevi y ile x işlevinin hesaplar. |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))` |
| MS |Bağımsız değişkenler arasındaki farkı milisaniye olarak döndürür. Tarihleri UNIX ya da POSIX zaman dönem, gece yarısı, 1 Ocak 1970 UTC göreli ' dir. Bağımsız değişkenler bir dizinlenmiş TrieDateField ya da sabit tarih temelli tarih matematik adı olması veya artık olabilir. `ms()`eşdeğer olan `ms(NOW)`, dönem itibaren milisaniye sayısı. `ms(a)`bağımsız değişkeni temsil eden dönem itibaren milisaniye sayısını döndürür. `ms(a,b)`Bu b milisaniye sayısını döndürür önce oluşur olduğu `a - b`. |`ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| değil |Mantıksal değeri kadar çevrilerek sarmalanmış işlev. |`not(exists(author))`Yalnızca TRUE olduğunda `exists(author)` false olur. |
| or |Mantıksal ayrım. |`or(value1,value2)`TRUE ya da value1 veya value2 doğrudur. |
| POW |Belirtilen güç belirtilen tabanda başlatır. `pow(x,y)`başlatır y gücünü x. |`pow(x,y)`<br>`pow(x,log(y))`<br>`pow(x,0.5)`Aynı sqrt. |
| Ürün |Birden çok değerler veya virgülle ayrılmış bir listede belirtilen işlevler çarpımını döndürür. `mul(...)`Ayrıca bir diğer ad olarak bu işlev için kullanılabilir. |`product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| Alıcı |İle karşılıklı bir işlev gerçekleştirir `recip(x,m,a,b)` uygulama `a/(m*x+b)`, burada m, a, b olan sabitleri ve x, herhangi bir rastgele karmaşık işlev. Zaman bir ve b eşittir ve x > = 0, bu işlev bir maksimum değer olarak artar x bırakır 1 sahiptir. Değerini artırmayı bir ve b birlikte tüm işlev eğri daha düz bir parçası için hareketini sonuçlanır. Bu özellikler bu x olduğunda daha yeni belgeleri artırmanın için ideal bir işlev yapabilirsiniz `rord(datefield)`. |`recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| RAD |Derece radyan için dönüştürür. |`rad(x)` |
| Yazdır |En yakın tamsayıya yuvarlar. |`rint(x)`  <br> `rint(5.6)`6'yı döndürür |
| sin |Bir açının sinüsünü döndürür. |`sin(x)` |
| SİNH |Bir açının hiperbolik sinüsünü döndürür. |`sinh(x)` |
| Ölçek |İşlevin x değerleri belirtilen minTarget ve maxTarget (bunlar dahil) arasında olacak şekilde ölçeklendirir. Geçerli tüm doğru ölçek seçebilirsiniz şekilde min ve Mak edinme işlevi değerleri erişir. Belgeleri sildikten sonra geçerli ayırt edemez ya da herhangi bir değer olan belgeler. Bu durumlarda 0.0 değerleri kullanır. Bu değerler normalde 0. 0 ' tüm büyük ise, bir hala 0,0 ile eşlenecek en küçük değer olarak düşebilir olduğunu anlamına gelir. Bu durumda, uygun bir `map()` işlevi kullanılabilirdi geçici bir çözüm olarak 0,0 gerçek aralıktaki bir değere değiştirmek için aşağıda gösterildiği gibi:`scale(map(x,0,0,5),1,2)` |`scale(x,minTarget,maxTarget)`<br>`scale(x,1,2)`X değerleri tüm 1 ve 2 (bunlar dahil) arasında değerler şekilde ölçeklendirir. |
| Sqrt |Belirtilen değer ya da işlevin kare kökünü döndürür. |`sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| strdist |İki dizeyi arasındaki uzaklığı hesaplar. Lucene yazım denetleyicisi StringDistance arabirimini kullanır ve tüm bu pakette kullanılabilir uygulamaları destekler. Kendi Solr'ın kaynak aracılığıyla yükleme özellikleri takın uygulamaların da sağlar. strdist geçen `(string1, string2, distance measure)`. Uzaklık ölçü için olası değerler şunlardır:<ul><li>jw: Jaro Winkler</li><li>Düzenle: Levenstein veya düzenleme uzaklığı</li><li>ngram: NGramDistance belirtilmişse, isteğe bağlı olarak iletebilir ngram boyutu çok. Varsayılan 2'dir.</li><li>FQN: Sınıf adı StringDistance arabirimi bir uygulama için tam. Hayır arg oluşturucuya sahip olmalıdır.</li></ul> |`strdist("SOLR",id,edit)` |
| Sub |X-y döndürür `sub(x,y)`. |`sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| TOPLA |Birden çok değerler veya virgülle ayrılmış bir listede belirtilen işlevler toplamını döndürür. `add(...)`Bu işlev için bir diğer ad olarak kullanılabilir. |`sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)` |
| termfreq |Bu belge için alanında terimi görünür sayısı döndürür. |termfreq(Text,'memory') |
| tan |Bir açının tanjantını döndürür. |`tan(x)` |
| TANH |Bir açının hiperbolik tanjantını döndürür. |`tanh(x)` |

## <a name="search-field-and-facet-reference"></a>Arama alanı ve modeli başvurusu
Verileri bulmak üzere günlük arama kullandığınızda, sonuçlar çeşitli alan ve modelleri görüntüler. Bazı bilgiler çok açıklayıcı görünmeyebilir. Sonuçları anlamanıza yardımcı olması için aşağıdaki bilgileri kullanın.

| Alan | Arama türü | Açıklama |
| --- | --- | --- |
| Tenantıd |Tümü |Bölüm verileri için kullanılır. |
| TimeGenerated |Tümü |Zaman çizelgesi, timeselectors (arama ve diğer ekranlar) sürücü için kullanılır. Veri parçası (genellikle aracı üzerinde) oluşturulduğunda temsil eder. Süre ISO biçiminde ifade edilir ve her zaman UTC değil. Varolan Araçları'nı (diğer bir deyişle, olay günlüğü'ndeki) temel alan türleri söz konusu olduğunda, bu genellikle günlük girişi/satır/kaydı günlüğe gerçek zamanlı olur. Bazı yönetim paketleri aracılığıyla veya bulutta (örneğin, önerileri veya Uyarıları) üretilen diğer türlerini zaman farklı bir şey temsil eder. Bu zaman bu yeni veri parçası çeşit yapılandırmanın bir anlık görüntüsünü ile toplanan veya bir öneri/Uyarısı, göre üretilmiştir zamandır. |
| Olay Kimliği |Olay |Windows olay günlüğünde olay kimliği. |
| Olay günlüğü |Olay |Olay günlüğü olayı Windows tarafından burada günlüğe kaydedildi. |
| EventLevelName |Olay |Kritik/Uyarı/bilgi/başarılı |
| eventLevel |Olay |Kritik/Uyarı/bilgi/başarı ilişkin sayısal değer (EventLevelName daha kolay/daha okunabilir sorgular için bunun yerine kullanın). |
| SourceSystem |Tümü |Verilerin nereden geldiği (cinsinden modu hizmetine ekleme). Örnek Microsoft System Center Operations Manager ve Azure Storage verilebilir. |
| ObjectName |PerfHourly |Windows performans nesnesi adı. |
| InstanceName |PerfHourly |Windows performans sayacı örneği adı. |
| CounteName |PerfHourly |Windows performans sayacı adı. |
| ObjectDisplayName |PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty |Operations Manager bir performans toplama kuralı tarafından hedeflenen nesnenin adını görüntüler. Operasyonel Öngörüler veya uyarının oluşturulduğu karşı bulunan nesnenin görünen adını da olabilir. |
| RootObjectName |PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty |Üst (çift bir barındırma ilişkisi) Operations Manager bir performans toplama kuralı tarafından hedeflenen nesnenin üst görünen adı. Operasyonel Öngörüler veya uyarının oluşturulduğu karşı bulunan nesnenin görünen adını da olabilir. |
| Bilgisayar |Çoğu türleri |Veri ait olduğu bilgisayar adı. |
| DeviceName |ProtectionStatus |Bilgisayar adı verileri ("Bilgisayar" ile aynı) ait. |
| DetectionId |ProtectionStatus | |
| ThreatStatusRank |ProtectionStatus |İş parçacığı durumu derece tehdit durum sayısal bir gösterimi ' dir. HTTP yanıt kodları benzeyen, derecelendirme sayılar arasında boşluk sahiptir (herhangi bir tehdit neden olduğu 150 ve değil 100 veya 0 değil), yeni durum eklemek için yeriniz çıkılıyor. İş parçacığı durumu ve koruma durumu toplaması için amacınız bilgisayarın seçili dönemde düzeltilme en kötü durumu göstermektir. Kayıt için en yüksek sayıyı görebilecekleri sayıları farklı durumları rank. |
| ThreatStatus |ProtectionStatus |ThreatStatus, açıklaması, 1:1 ThreatStatusRank ile eşler. |
| TypeofProtection |ProtectionStatus |Bilgisayarda Algılanan kötü amaçlı yazılımdan koruma ürün: none, Microsoft kötü amaçlı yazılım kaldırma aracını, Forefront ve benzeri. |
| ComputerName |ProtectionStatus | |
| SourceHealthServiceId |ProtectionStatus, RequiredUpdate |Bu bilgisayarın aracı için sistem sağlığı hizmeti kimliği. |
| HealthServiceId |Çoğu türleri |Bu bilgisayarın aracı için sistem sağlığı hizmeti kimliği. |
| ManagementGroupName |Çoğu türleri |Operations Manager bağlı aracılar için yönetim grubu adı. Aksi takdirde, null/boş olur. |
| ObjectType |ConfigurationObject |Günlük analizi yapılandırması değerlendirme tarafından bulunan bu nesne için (Operations Manager Yönetim Paketi türü/sınıfı olduğu gibi) yazın. |
| UpdateTitle |RequiredUpdate |Adı bulundu güncelleştirmesi yüklü değil. |
| PublishDate |RequiredUpdate |Ne zaman güncelleştirme Microsoft Update sitesinde yayımlandı. |
| Sunucu |RequiredUpdate |Bilgisayar adı verileri ("Bilgisayar" ile aynı) ait. |
| Ürün |RequiredUpdate |Güncelleştirmenin geçerli ürün. |
| UpdateClassification |RequiredUpdate |Güncelleştirme (örneğin, güncelleştirme paketi veya hizmet paketi) türü. |
| KBID |RequiredUpdate |Bu en iyi yöntem veya güncelleştirmeyi açıklayan KB makalesi kimliği. |
| WorkflowName |ConfigurationAlert |Kural veya İzleyici uyarıyı üretilen adı. |
| Önem Derecesi |ConfigurationAlert |Uyarının önem derecesi. |
| Öncelik |ConfigurationAlert |Uyarı önceliği. |
| IsMonitorAlert |ConfigurationAlert |Bu uyarı bir izleyici (true) veya bir kural (false) tarafından oluşturulur? |
| AlertParameters |ConfigurationAlert |XML günlük analizi uyarı parametrelere sahip. |
| Bağlam |ConfigurationAlert |XML ile günlük analizi uyarı bağlamı. |
| İş yükü |ConfigurationAlert |Teknoloji veya uyarı başvurduğu iş yükü. |
| AdvisorWorkload |Öneri |Teknoloji veya öneri başvurduğu iş yükü. |
| Açıklama |ConfigurationAlert |Uyarı açıklaması (kısa). |
| DaysSinceLastUpdate |UpdateAgent |Kaç gün (göre bu kaydın TimeGenerated) önce bu aracının herhangi bir güncelleştirme Windows Server Update Service (WSUS) veya Microsoft Update yüklediniz mi? |
| DaysSinceLastUpdateBucket |UpdateAgent |DaysSinceLastUpdate, kategori, ne kadar zaman önce bir bilgisayar son herhangi bir güncelleştirme WSUS/Microsoft Update sitesinden yüklenen zaman demet içinde temel. |
| AutomaticUpdateEnabled |UpdateAgent |Otomatik Güncelleştirme denetimi etkinleştirildiğini veya bu Aracısı'nı devre dışı? |
| AutomaticUpdateValue |UpdateAgent |Otomatik güncelleştirme otomatik olarak indirmeniz ve yüklemeniz, yalnızca yükleyin veya yalnızca denetlemek için kümesi denetliyor? |
| WindowsUpdateAgentVersion |UpdateAgent |Microsoft Update Aracı sürüm numarası. |
| WSUSServer |UpdateAgent |Hangi WSUS sunucusu, bu güncelleştirme Aracısı hedeflediği? |
| OSVersion |UpdateAgent |Bu güncelleştirme Aracısı çalışan işletim sistemi sürümü. |
| Ad |Öneri, ConfigurationObjectProperty |Ad/başlığı öneri veya günlük analizi yapılandırması değerlendirme özelliğinden adı. |
| Değer |ConfigurationObjectProperty |Günlük analizi yapılandırması değerlendirme özelliğinden değeri. |
| KBLink |Öneri |Bu en iyi yöntem veya güncelleştirmeyi açıklayan KB makalesine URL. |
| RecommendationStatus |Öneri |Arama dizini eklemiş kayıtlarını güncelleştirilmesi birkaç türleri arasında önerilerdir. Bu durum, öneri etkin/açık olup olmadığını veya günlük analizi çözümlendiğini doğrulamaktadır algılarsa değiştirir. |
| RenderedDescription |Olay |Bir Windows olayı (doldurulmuş parametrelerle yeniden kullanılan metin) açıklaması çizilir. |
| ParameterXml |Olay |XML Windows (Olay Görüntüleyicisi'nde görüldüğü gibi) olay verileri bölümünde parametrelere sahip. |
| EventData |Olay |Windows (Olay Görüntüleyicisi'nde görüldüğü gibi) olay tüm veri bölümünü içeren XML. |
| Kaynak |Olay |Olayı oluşturan olay günlüğü kaynağı. |
| EventCategory |Olay |Windows olay günlüğünden doğrudan olay kategorisi. |
| Kullanıcı adı |Olay |Windows olay (genellikle, NT AUTHORITY\LOCALSYSTEM) kullanıcı adı. |
| Görüntülendiğinden |PerfHourly |Saatlik toplama, bir performans sayacının ortalama değeri. |
| Min |PerfHourly |Bir performans sayacı saatlik toplama saatlik aralığı en düşük değer. |
| Maks. |PerfHourly |Bir performans sayacı saatlik toplama saatlik aralığı en büyük değeri. |
| Percentile95 |PerfHourly |Bir performans sayacı saatlik toplama saatlik aralığı 95 yüzdelik değer. |
| SampleCount |PerfHourly |Kaç tane ham performans sayacı örneklerinin saatlik bu birleşik kayıt oluşturmak için kullanılmıştır. |
| Tehdit |ProtectionStatus |Kötü amaçlı yazılım bulundu adı. |
| StorageAccount |W3CIISLog |Azure depolama hesabı günlüğü okuma. |
| AzureDeploymentID |W3CIISLog |Bulut hizmetinin günlük Azure dağıtım kimliği ait. |
| Rol |W3CIISLog |Azure bulut hizmeti günlük rolüne ait. |
| RoleInstance |W3CIISLog |Günlük ait Azure rol RoleInstance. |
| sSiteName |W3CIISLog |Günlük (metatabanı gösterimine); ait olduğu IIS Web sitesi özgün günlüğü s-sitename alanında. |
| sComputerName |W3CIISLog |Özgün günlüğü s-computername alanında. |
| SIP |W3CIISLog |Sunucu IP adresi HTTP isteği için giderilmiştir. Özgün günlüğü s-ip alanında. |
| csMethod |W3CIISLog |HTTP isteği istemci tarafından kullanılan HTTP yöntemini (örneğin, GET/POST). Cs-yöntem özgün günlüğü. |
| CIP |W3CIISLog |İstemci IP adresi HTTP isteği geldi. Özgün günlüğü c-ip alanında. |
| csUserAgent |W3CIISLog |HTTP User-Agent istemci tarafından bildirilen (tarayıcı veya aksi halde). Cs-user-agent özgün günlüğünde. |
| scStatus |W3CIISLog |Sunucu tarafından istemciye döndürülen HTTP durum kodu (örneğin, 200/403/500). Özgün günlüğü cs-durum. |
| TimeTaken |W3CIISLog |Nasıl isteği tamamlamak için harcanan uzun süre (milisaniye cinsinden). Özgün günlüğü timetaken alanında. |
| csUriStem |W3CIISLog |Göreli URI (ana bilgisayar adresi olmadan, / arama) işlemi istendi. Özgün günlüğü cs bulunamadı.%n alanında. |
| csUriQuery |W3CIISLog |URI sorgusu. Bu alan bir tire statik sayfaları için genellikle içerecek şekilde URI sorgular yalnızca ASP sayfalarının gibi dinamik sayfalar için gereklidir. |
| Spor |W3CIISLog |HTTP isteğinin gönderildiği (ve bu toplanma beri için IIS'in dinler) sunucu bağlantı noktası. |
| csUserName |W3CIISLog |İstek Kimliği doğrulanmış ve değil anonim ise kullanıcı adı kimlik doğrulaması. |
| csVersion |W3CIISLog |(Örneğin, HTTP/1.1) istekte kullanılan HTTP protokolü sürümü. |
| csCookie |W3CIISLog |Tanımlama bilgileri. |
| csReferer |W3CIISLog |Kullanıcının son ziyaret sitesi. Bu site geçerli siteye bir bağlantı sağladı. |
| csHost |W3CIISLog |İstenen ana bilgisayar üst bilgisi (örneğin, www.mysite.com). |
| scSubStatus |W3CIISLog |Alt durum hata kodu. |
| scWin32Status |W3CIISLog |Windows durum kodu. |
| csBytes |W3CIISLog |İstekte istemciden sunucuya gönderilen bayt sayısı. |
| scBytes |W3CIISLog |Sunucudan gelen yanıtı istemciye geri döndürülen bayt sayısı. |
| ConfigChangeType |ConfigurationChange |Değişiklik (örneğin, WindowsServices/yazılım) türü. |
| ChangeCategory |ConfigurationChange |(Değiştirilen/eklenen/kaldırıldı) değişiklik kategorisi. |
| SoftwareType |ConfigurationChange |Yazılım (güncelleştirme/uygulama) türü. |
| SoftwareName |ConfigurationChange |(Yalnızca yazılım değişiklikleri için geçerlidir) yazılım adı. |
| Yayımcı |ConfigurationChange |(Yalnızca yazılım değişiklikleri için geçerlidir) yazılım yayımlar satıcı. |
| SvcChangeType |ConfigurationChange |Bir Windows hizmeti (durumu/StartupType/yol/HizmetHesabı) uygulandı değişiklik türü. Bu yalnızca Windows hizmet değişiklikleri için geçerlidir. |
| SvcDisplayName |ConfigurationChange |Değiştirilen hizmet görünen adı. |
| SvcName |ConfigurationChange |Değiştirildi hizmetin adı. |
| SvcState |ConfigurationChange |Hizmetinin yeni (geçerli) durumu. |
| SvcPreviousState |ConfigurationChange |(Yalnızca hizmet durumu değişirse geçerlidir) hizmetinin durumunu bilinen önceki. |
| SvcStartupType |ConfigurationChange |Hizmet başlangıç türü. |
| SvcPreviousStartupType |ConfigurationChange |Önceki hizmet başlatma türünü (yalnızca hizmet başlatma türünü değiştirdiyseniz geçerlidir). |
| SvcAccount |ConfigurationChange |Hizmet hesabı. |
| SvcPreviousAccount |ConfigurationChange |Önceki hizmet hesabı (yalnızca hizmet hesabı değiştirdiyseniz geçerlidir). |
| SvcPath |ConfigurationChange |Windows hizmeti yürütülebilir dosya yolu. |
| SvcPreviousPath |ConfigurationChange |Yürütülebilir dosya (yalnızca onu değiştirdiyseniz geçerlidir) Windows hizmeti için önceki yolu. |
| SvcDescription |ConfigurationChange |Hizmet açıklaması. |
| Önceki |ConfigurationChange |Bu yazılımı (yüklü/değil yüklü/önceki sürüm) önceki durumu. |
| Geçerli |ConfigurationChange |Son durum bu yazılımın (yüklü/değil yüklü/geçerli sürüm). |

## <a name="next-steps"></a>Sonraki adımlar
Günlük aramaları hakkında ek bilgi için:

* Çözümler tarafından toplanan ayrıntılı bilgileri görüntülemek için [günlük aramaları](log-analytics-log-searches.md) hakkında bilgi edinin.
* Kullanım [günlük analizi içinde özel alanlar](log-analytics-custom-fields.md) günlük aramaları genişletmek için.
