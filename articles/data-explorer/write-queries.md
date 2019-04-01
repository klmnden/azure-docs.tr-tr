---
title: Azure Veri Gezgini için sorguları yazma
description: Bu nasıl yapılır makalesinde Azure Veri Gezgini için temel ve daha gelişmiş sorguları gerçekleştirmeyi öğrenin.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 8afb829f806ab55a069ded9cb7198f66368e8720
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758701"
---
# <a name="write-queries-for-azure-data-explorer"></a>Azure Veri Gezgini için sorguları yazma

Bu makalede, en yaygın işleçli temel sorguları gerçekleştirmek için Azure veri Gezgini'nde sorgu dili kullanmayı öğrenin. Dil daha gelişmiş özelliklerinden bazılarını maruz ayrıca Al

## <a name="prerequisites"></a>Önkoşullar

Bu makalede iki yoldan biriyle sorgular çalıştırabilirsiniz:

- Azure Veri Gezgini üzerindeki *Yardım kümesi* biz öğrenme yardımcı olmak için ayarladığınız.
    [Küme oturum](https://dataexplorer.azure.com/clusters/help/databases/samples) ile Azure Active directory üyesi olan bir kuruluş e-posta hesabı.

- Kendi kümenizi, StormEvents örnek veriler içerir. Daha fazla bilgi için [hızlı başlangıç: Bir Azure Veri Gezgini kümesi ile veritabanı oluşturma](create-cluster-database-portal.md) ve [örnek verileri Azure veri Gezgini'ne alma](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="overview-of-the-query-language"></a>Sorgu dili genel bakış

Azure veri Gezgini'nde bir sorgu dili, verilerini işleyebilir ve sonuçları döndürmek için salt okunur bir istektir. İsteği okumak, yazmak ve otomatik hale getirmek söz dizimi kolaylaştırmak için tasarlanmış bir veri akışı modelini kullanarak düz metin olarak belirtilir. SQL'e benzer bir hiyerarşideki düzenlenir şema varlıkları sorgusunu kullanır: veritabanı, tablolar ve sütunlar.

Sorgu ifadeleri, noktalı virgülle ayrılmış bir dizi sorgu oluşur (`;`), bir tablo ifadesi ifade edilen en az bir deyimi ile olduğu bir tablo gibi ağ sütun ve satır içinde düzenlenmiş veriler üretir bir ifade. Sorgunun tablosal ifade deyimleri, sorgunun sonuçlarını üretir.

Tablosal bir ifade deyimi sözdizimi tablosal verileri içeren bir tablo sorgu işleci bir akıştan diğerine, veri kaynağı (örneğin, bir veritabanı veya veri üreten bir işleç tablosu) ile başlayan ve ardından veri dönüştürme bir dizi aracılığıyla akar. kanal kullanılarak birbirine bağlı işleçleri (`|`) sınırlayıcı.

Örneğin, aşağıdaki sorguyu bir tablo ifadesi deyimi tek bir deyimde vardır. Adlı bir tablo başvurusu ifadesi başlayan `StormEvents` (Bu tablonun ana veritabanı örtük burada ve bağlantı bilgilerini bir parçası olan). Bu tablo için verileri (satır), ardından değeri tarafından filtrelenir `StartTime` sütun ve ardından değeri tarafından filtrelenen `State` sütun. Sorgu, ardından "satırları geri kalan" sayı döndürür.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVWws1VISSxJLQGyNYwMDMx1DQ11DQw1FRLzUpBU2aArMgIpQjGvJFXB1lZByc3HP8jTxVFJQQEkm5xfmlcCAHoR9euCAAAA)**\]**

```Kusto
StormEvents
| where StartTime >= datetime(2007-11-01) and StartTime < datetime(2007-12-01)
| where State == "FLORIDA"  
| count
```

Bu durumda, sonuç olur:

|Sayı|
|-----|
|   23|
| |

Daha fazla bilgi için [sorgu dili başvurusu](https://aka.ms/kustolangref).

## <a name="most-common-operators"></a>En yaygın işleçleri

Bu bölümde yer alan işleçler, Azure veri Gezgini'ndeki sorguları anlamak için yapı taşlarıdır. Yazdığınız sorguların çoğu bu işleçlerden bazıları içerir.

Yardım kümesinde sorguları çalıştırmak için: seçin **sorgu çalıştırmak için tıklayın** yukarıda her sorgu.

Kendi kümenizi sorguları çalıştırmak için:

1. Her sorgu, sorgu web tabanlı uygulamaya kopyalayın ve ardından sorguyu seçin veya sorguda imleci yerleştirin.

1. Uygulamanın üstünde seçin **çalıştırma**.

### <a name="count"></a>count

[**sayısı**](https://docs.microsoft.com/azure/kusto/query/countoperator): Tablodaki satır sayısını döndürür.

Aşağıdaki sorguyu StormEvents tablodaki satır sayısını döndürür.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSspVqhRSM4vzSsBALU2eHsTAAAA)**\]**

```Kusto
StormEvents | count
```

### <a name="take"></a>sınav zamanı

[**ele**](https://docs.microsoft.com/azure/kusto/query/takeoperator): Veri satırı belirtilen sayıya kadar döndürür.

Aşağıdaki sorguda StormEvents tablosundan beş satırları döndürür. Anahtar sözcüğü *sınırı* için bir diğer addır *yararlanın.*

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSspVqhRKEnMTlUwBQDEz2b8FAAAAA%3d%3d)**\]**

```Kusto
StormEvents | take 5
```

> [!TIP]
> Kaynak veriler sürece hangi kayıtlar döndürülür bir garanti yoktur.

### <a name="project"></a>Proje

[**Proje**](https://docs.microsoft.com/azure/kusto/query/projectoperator): Bir sütun alt kümesi seçer.

Aşağıdaki sorgu, belirli bir sütun kümesini döndürür.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUShJzE5VMAWxCorys1KTSxSCSxKLSkIyc1N1FFzzUiAMoFgJiA%2fSFlJZAGS6JOYmpqcGFOUXpBaVVAKlCjKL81NS%2fRKLihJLMstSAY%2buIINnAAAA)**\]**

```Kusto
StormEvents
| take 5
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

### <a name="where"></a>nereye

[**Burada**](https://docs.microsoft.com/azure/kusto/query/whereoperator): Bir koşulu karşılayan satırların alt tablo filtreler.

Aşağıdaki sorgu verileri göre filtreler `EventType` ve `State`.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAEWMPQvCMBCGd8H%2fcFuWro4dBOvHkgoJOB%2fm0KjJhetRKfjjNe3g9n49r1OW1I2UdVivPvC%2bkxDM3k%2bFoG3B7F%2fMwQDmAE5Rl%2fCydceTPfjemsopPgk2VRXhB121TkV9TNRAl8MiZrz53zeww4Q3OgsXEp1%2bVYkDB7IoghpH%2bgI9OH8WnwAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| take 5
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

### <a name="sort"></a>Sıralama

[**Sıralama**](https://docs.microsoft.com/azure/kusto/query/sortoperator): Giriş tablosunun satırları sırada bir veya daha fazla sütuna göre sıralayın.

Aşağıdaki sorguda göre azalan düzende verileri sıralar `DamageProperty`.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2NPQvCMBCGd8H%2fcFuXrI4dBOvHEoUGnM%2fm0KjphctRKfjjNe0guL0fvM%2fbKktsBuo1LxdveN1ICCbvxkRQ11Btn8y%2bAuw9tIo6h%2bd1uz%2fYnTvaquwyi8JlhA1GvNJJOJHoCJ5yV2rFB8GqqCR8p04LSdSFSAaa3s9iopvfu%2fnDfasUMnuyKIIaBvoAtvGMsb4AAAA%3d)**\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| sort by DamageProperty desc
| take 5
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

> [!NOTE]
> İşlemlerin sırası önemlidir. Yerleştirmeyi deneyin `take 5` önce `sort by`. Farklı sonuçlar elde ederim?

### <a name="top"></a>Sayfanın Üstü

[**üst**](https://docs.microsoft.com/azure/kusto/query/topoperator): İlk döndürür *N* kayıtları belirtilen sütunlara göre sıralanır.

Aşağıdaki sorgu, aynı sonuçları üzerinde daha az bir işleci ile döndürür.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2NOwvCMBSFd8H%2fcLcsWR07CNbHkgoJOMfmohGTG24vlYA%2fXtsOgtt5cL5jhTi1I2YZ1qs3vO7ICLN3tSA0Daj9kygo8DmAFS9LeNna48kcXGfUtBMqsIFrhZ1P%2foZnpoIsFQIO%2fdQXpgf2MgFYXEyooc1hETNU%2f071H%2bRblThQQOOZvcQRP1rSng21AAAA)**\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| top 5 by DamageProperty desc
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

### <a name="extend"></a>Genişletme

[**genişletme**](https://docs.microsoft.com/azure/kusto/query/extendoperator): Hesaplar sütunları türetilmiş.

Aşağıdaki sorgu, her satırda bir değer bilgi işlem tarafından yeni bir sütun oluşturur.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2OvQ7CMAyEdyTewVuWMDJ2QGr5WQJSKzGHxoIiEkeuKVTi4WmooBKbfXeffaUQ%2b6LDIO189oLHBRnhs1d9RMgyUOsbkVNgg4NSrIzicVVud2ZT7Y1KnFCEJZx6yK23ZzwwRWTpwWFbJx%2bfggOf39lKQwEyKIKrGo%2bwSEdZ0pyCkemKtUyi%2fib1j9ZjDz311H9%2fBys2LTk0lhPT4RvwA3pn6AAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| top 5 by DamageProperty desc
| extend Duration = EndTime - StartTime
| project StartTime, EndTime, Duration, State, EventType, DamageProperty, EpisodeNarrative
```

İfadeler, normal tüm işleçleri içerebilir (+, -, *, /, %), ve bir dizi çağırabilirsiniz kullanışlı işlevi yoktur.

### <a name="summarize"></a>Özetleme

[**Özetleme**](https://docs.microsoft.com/azure/kusto/query/summarizeoperator): Satır gruplarını toplar.

Aşağıdaki sorgu olayların sayısını döndürür `State`.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIBYnFJ%2beX5pUo2CqAaQ1NhaRKheCSxJJUAB%2fedDI3AAAA)**\]**

```Kusto
StormEvents
| summarize event_count = count() by State
```

**Özetlemek** işleci grupları birlikte aynı değerlere sahip satırları **tarafından** yan tümcesi ve ardından toplama işlevini kullanıyor (gibi **sayısı**) her grubu birleştirmek için tek bir satıra. Bu nedenle, bu durumda, yoktur her durum için bir satır ve bu durumda bulunan satır sayısı için bir sütun.

Toplama işlevleri çeşitli yoktur ve birkaç tanesi birinde kullanabilirsiniz **özetlemek** birkaç oluşturmak için işleç hesaplanan sütunları. Örneğin, fırtınalarını sayısı her durum ve fırtınalarını durumu başına benzersiz sayısını almak, ardından kullanmak **üst** en storm etkilenmeyen durumlarını almak için.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlUIBkk455fmlSjYKiSDaA1NHYWQyoJU%2fzSwXDFQPAUiAdYPktJUSKoE6kwsSQUZVpJfoGAKEYGblZJanAwAgbFb73QAAAA%3d)**\]**

```Kusto
StormEvents
| summarize StormCount = count(), TypeOfStorms = dcount(EventType) by State
| top 5 by StormCount desc
```

Sonucu bir **özetlemek** işlem var:

- Her bir sütunun adlı **tarafından**

- Her hesaplanan bir ifadenin bir sütun

- Her kombinasyonu değerlere göre satır

### <a name="render"></a>İşleme

[**işleme**](https://docs.microsoft.com/azure/kusto/query/renderoperator): Sonuçları bir grafik çıktı işler.

Aşağıdaki sorgu, bir sütun grafiği görüntüler.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWMsQ7CQAxDdyT%2bIWMrdSgbSxmQ2Nj6Aei4Ru0hkqA0VwTi49uUBRZL9rPdmiidJmQbt5sPjJkoaHojoGeXKJmtWbUoK6DUQQNh6osj9onPwUq4vqC1YLjORc2Dpef2OaD%2bPcEBdvu6dvZQuWG077b6LTlV5A4VotwzcRyC2gxU6ktSqQAAAA%3d%3d)**\]**

```Kusto
StormEvents
| summarize event_count=count(), mid = avg(BeginLat) by State
| sort by mid
| where event_count > 1800
| project State, event_count
| render columnchart
```

Aşağıdaki sorgu, bir basit zaman grafiği görüntüler.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIBYnFJ%2beX5pXYgkkNTYWkSoWkzDyN4JLEopKQzNxUHQXDFE2QtqLUvJTUIoUSoFhyBlASAAyXWQJWAAAA)**\]**

```Kusto
StormEvents
| summarize event_count=count() by bin(StartTime, 1d)
| render timechart
```

Aşağıdaki sorgu, saatlerine binned süresi bir gün, modül olayları sayar ve zaman grafiği görüntüler.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEADWNQQqDMBRE90LvMBtBwY0HcNkT2L2k8UuEJh9%2bfqSWHt4k4GZghpk3s7L450FB46P5g75KYYXjJJiwfZilm9WIvnZPaDGuGDC6vnRj8t7I%2fiNQ2S%2bWU9CpatfjfVZKLbLo7WGiLZnkGxJoxlqX%2bRf81ZbyiAAAAA%3d%3d)**\]**

```Kusto
StormEvents
| extend hour = floor(StartTime % 1d , 1h)
| summarize event_count=count() by hour
| sort by hour asc
| render timechart
```

Aşağıdaki sorgu, birden çok günlük bir zaman grafiği serisinin karşılaştırır.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEACWPSwvCMBCE74L%2fYSgIFXrpD%2bihaKzxkUBTXyeputKCbSCmvvDHm9TL7gwzsN8qq03DHtTa%2b3DwBb0stRdUujMJrjetTQhlS2OLuiGMEF8QIa7GvvusyJBPLaFuEQbZZjWDnGHN9nwigyhYp1wwt7c8z7jgqZM7riZSKC6cFjIv5pimS1n4SLAdFixX7OCMzFkmRdAfundNU5r6QyAPejzrrrVJP8MxTu8eN%2fqT%2bL5xL5CBdcjnyrH%2fALPTSKnkAAAA)**\]**

```Kusto
StormEvents
| extend hour= floor( StartTime % 1d , 1h)
| where State in ("GULF OF MEXICO","MAINE","VIRGINIA","WISCONSIN","NORTH DAKOTA","NEW JERSEY","OREGON")
| summarize event_count=count() by hour, State
| render timechart
```

> [!NOTE]
> **İşleme** işlecidir bölümü Altyapısı'nın yerine bir istemci-tarafı özelliği. Kullanım kolaylığı için dile tümleşiktir. Web uygulaması aşağıdaki seçeneklerini destekler: barchart, columnchart, zaman grafiğini, pasta grafiği görüntüsü ve linechart. 

## <a name="scalar-operators"></a>Skaler işleçler

Bu bölümde bazı en önemli skaler işleçler kapsar.

### <a name="bin"></a>bin()

[**bin()**](https://docs.microsoft.com/azure/kusto/query/binfunction): Değerleri tamsayı aşağı yuvarlar birden çok belirli bir depo boyutu.

Aşağıdaki sorgu sayısı ile bir gün demet boyutunu hesaplar.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVWwU0hJLEktATI1jAwMzHUNjHQNTTQVEvNSkBTZYCoyMtQEGVdcmpubWJRZlaqQCrIiPjm%2fNK9EwVYBTGtoKiRVKiRl5mnAjdJRMEzRBABIhjnmkwAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where StartTime > datetime(2007-02-14) and StartTime < datetime(2007-02-21)
| summarize event_count = count() by bin(StartTime, 1d)
```

### <a name="case"></a>case()

[**case()**](https://docs.microsoft.com/azure/kusto/query/casefunction): Koşullar listesini değerlendirir ve ilk sonuç ifade olan bir koşul karşılandığında ya da son verir **başka** ifade. Bu işleç, kategorilere veya verileri gruplandırmak için kullanabilirsiniz:

Aşağıdaki sorgu yeni bir sütun döndürür `deaths_bucket` ve deaths numarasına göre gruplandırır.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAGWOwQrCQAxE74X%2bQ9hTCwX14FFBaK9e%2bgGS7gZdbFrYZEXFj7dbqgfNbfJmhml1DNzcaFDJsxdIZMbgnwSOUC8Cu%2fQq6lnUPpDVEroHtIpKKUB3pcEt7lMX7ZV0ClkUgiLPYLqlaQ%2fbdQWmx3AmU%2f2gTUJMzkf%2bYwkJY99%2fiDmuDqac545Bv3MAxb4Bic1Oy88AAAA%3d)**\]**

```Kusto
StormEvents
| summarize deaths = sum(DeathsDirect) by State
| extend deaths_bucket = case (
    deaths > 50, "large",
    deaths > 10, "medium",
    deaths > 0, "small",
    "N/A")
| sort by State asc
```

### <a name="extract"></a>extract()

[**extract()**](https://docs.microsoft.com/azure/kusto/query/extractfunction): Bir metin dizesinden bir normal ifade için bir eşleşme alır.

Aşağıdaki sorguda bir izlemesinden belirli bir öznitelik değerleri ayıklar.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAE2OwQrCMBBE74X%2bw9BTojHYagSVHJRevXkrHqJdpVBbSVew4McbFYungeXtvKmJsetzxw4WZQh2x5og9t6daIWOfdVcJIpkY1OFrc0U8rt3XLWNTbOZnhultU4UfoD5A4zRmVkovInDOo6%2bojh6gh5MTTmQwR0uQckiGb5FMZ0s9WEsQ3uo%2fixSccT9jdqz8ORqKTECV1cSaSdfq2k6L8oAAAA%3d)**\]**

```Kusto
let MyData = datatable (Trace: string) ["A=1, B=2, Duration=123.45,...", "A=1, B=5, Duration=55.256, ..."];
MyData
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s)
```

Bu sorgu kullanan bir **izin** bir adı bağlayan deyimi (Bu durumda `MyData`) bir ifade. Burada kapsamı geri kalanı için **izin** deyimi görünür (genel kapsam veya bir işlev gövdesini kapsamında), adı, ilişkili değerine başvurmak için kullanılabilir.

### <a name="parsejson"></a>parse_json()

[**parse_json()**](https://docs.microsoft.com/azure/kusto/query/parsejsonfunction): Dize JSON değeri olarak yorumlar ve dinamik olarak değeri döndürür. Kullanarak üstündür **extractjson()** işlevi, bileşik bir JSON nesnesi birden fazla öğenin ayıklamak ihtiyacınız olduğunda.

Aşağıdaki sorguda bir diziden JSON öğeleri ayıklar.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAHWPQQuCQBCF74L%2fYdmLBSJ6EGKjU17r1E0kJh1C2XZlHc0w%2f3ur1s1O896bB%2fONRGKnVwIE7MAKOwhuEtnmYiBHwRoypbpvXSf1Bl60BqjUiot04B3IFrmIol0Q%2bpPLdauIi3iyj9KWojCcNfRWx7NuqEiw48KaMRu9bO86y3HXeTPsCVXBzvg8amlpajANXqtGq4VmO5VqoyvM6dsKfkhpmAUzkf9nM9OtLi3reg79ar788AEVX8GkOAEAAA%3d%3d)**\]**

```Kusto
let MyData = datatable (Trace: string)
['{"duration":[{"value":118.0,"valcount":5.0,"min":100.0,"max":150.0,"stdDev":0.0}]}'];
MyData
| extend NewCol = parse_json(Trace)
| project NewCol.duration[0].value, NewCol.duration[0].valcount, NewCol.duration[0].min, NewCol.duration[0].max, NewCol.duration[0].stdDev
```

Aşağıdaki sorgu, JSON öğeleri ayıklar.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAE2OwQqCQBCG74LvsOzFBBE9CLHRKa916hYRkw6RbLuyO5pRvXvrGtZpvn9m4P8kEts%2bSiBga1a7QXCWyBZ7AxUKZslc1SVmh%2bjJe5AdcpHnyzRLxlTpThEXxRhvV%2bVOWeYZBseFZ0t1iT0XLryj4yoMprIweDEcCFXNdnjfaOnaWzAWT43VamqPx6fW6AYr%2bn6l3iH5S95hXjiLH8Mw82TxAQvJEB%2fsAAAA)**\]**

```Kusto
let MyData = datatable (Trace: string) ['{"value":118.0,"valcount":5.0,"min":100.0,"max":150.0,"stdDev":0.0}'];
MyData
| extend NewCol = parse_json(Trace)
| project NewCol.value, NewCol.valcount, NewCol.min, NewCol.max, NewCol.stdDev
```

Aşağıdaki sorgu, dinamik veri türü ile JSON öğeleri ayıklar.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAD2NMQvCMBBG90D%2bw5GphVLSoSARt65ubuJwJjdU0lZiWlrU%2f25MotO9x8H7LHk4bh16hAOYcDxeLUFxcqhJgdlGHHpdcnbOWDzFgnYmoZpmV8tK6GkePTmh2q8N%2fRg%2bUkbGNXAb%2beFNR4tQQd7lZc9ZGuXsBXc33Uh7iJN1jFdZcvunIf5HXCvOEqf2BwXmDCnKAAAA)**\]**

```Kusto
let MyData = datatable (Trace: dynamic)
[dynamic({"value":118.0,"counter":5.0,"min":100.0,"max":150.0,"stdDev":0.0})];
MyData
| project Trace.value, Trace.counter, Trace.min, Trace.max, Trace.stdDev
```

### <a name="ago"></a>ago()

[**ago()**](https://docs.microsoft.com/azure/kusto/query/agofunction): Geçerli saati UTC zamanından belirtilen timespan çıkarır.

Aşağıdaki sorgu, son 12 saat boyunca verileri döndürür.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA1WOQQ6CQAxF9yTc4S8hQcmQuNSNR4ALTKQyJDAlnSIuPLwzJGrctM3v+7+t684R7qMEhW6MafQUMJAnsUoIdl4mQm/VVrC+h0Z6shFOINZAIc/qOql24KIEL8nIAuWYohC6sfQB9yjtPtPA8SrhmGeLjF7RjTO1Gu+cIdYPVHjeisOpLyukKTbjYml5piuvXknwIU1lGlPm2Qvzg55L+u+b9udIyOZI6LfHZf/YNK58Ay2HrbAEAQAA)**\]**

```Kusto
//The first two lines generate sample data, and the last line uses
//the ago() operator to get records for last 12 hours.
print TimeStamp= range(now(-5d), now(), 1h), SomeCounter = range(1,121)
| mvexpand TimeStamp, SomeCounter
| where TimeStamp > ago(12h)
```

### <a name="startofweek"></a>startofweek()

[**startofweek()**](https://docs.microsoft.com/azure/kusto/query/startofweekfunction): Sağlanırsa, bir uzaklık tarafından kaydırılacağı uzaklık tarihi içeren haftanın başlangıcını döndürür

Aşağıdaki sorguyu farklı uzaklıkları ile haftanın başlangıcını döndürür.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEACtKzEtPVchPSytOLVFIK8rPVdA1VCjJVzBUKC5JLVAw5OWqUSgoys9KTS5RKE9NzQ4uSSwqUbAFygLp%2fDSQkEZefrmGpg7UEE0dCA0AdE3lv1kAAAA%3d)**\]**

```Kusto
range offset from -1 to 1 step 1
| project weekStart = startofweek(now(), offset),offset
```

Bu sorgu kullanan **aralığı** işleci değerlerinin tek sütunlu bir tablo oluşturur. Ayrıca bkz: [ **startofday()**](https://docs.microsoft.com/azure/kusto/query/startofdayfunction), [ **startofweek()**](https://docs.microsoft.com/azure/kusto/query/startofweekfunction), [ **startofyear()** ](https://docs.microsoft.com/azure/kusto/query/startofyearfunction)), [ **startofmonth()**](https://docs.microsoft.com/azure/kusto/query/startofmonthfunction), [ **endofday()**](https://docs.microsoft.com/azure/kusto/query/endofdayfunction), [ **endofweek()**  ](https://docs.microsoft.com/azure/kusto/query/endofweekfunction), [ **endofmonth()**](https://docs.microsoft.com/azure/kusto/query/endofmonthfunction), ve [ **endofyear()**](https://docs.microsoft.com/azure/kusto/query/endofyearfunction).

### <a name="between"></a>between()

[**between()**](https://docs.microsoft.com/azure/kusto/query/betweenoperator): Kapsamlı aralığı içinde bir girdiyle eşleşir.

Aşağıdaki sorgu verilerini belirtilen tarih aralığına göre filtreleyin.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVVISi0pT03NU9BISSxJLQGKaBgZGJjrApGRuaaCnp4ChrixgaYmyKTk%2fNK8EgBluyagXgAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where StartTime between (datetime(2007-07-27) .. datetime(2007-07-30))
| count
```

Aşağıdaki sorgu, belirli bir tarih aralığındaki, üç gün hafif çeşitlemesi ile tarafından verilere filtre (`3d`) başlangıç tarihinden itibaren.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVVISi0pT03NU9BISSxJLQGKaBgZGJjrApGRuaaCnp6CcYomSF9yfmleCQCGAqjRTAAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where StartTime between (datetime(2007-07-27) .. 3d)
| count
```

## <a name="tabular-operators"></a>Tablo işleçleri

Kusto bazıları, bu makalenin diğer bölümlerde ele alınan tablo işleçlerinin çoğu, sahiptir. Burada odaklanacağız **ayrıştırma**. 

### <a name="parse"></a>Ayrıştırma

[**Ayrıştırma**](https://docs.microsoft.com/azure/kusto/query/parseoperator): Bir dize ifadesi değerlendirilir ve bir veya daha fazla hesaplanmış sütunlara değeri ayrıştırır. Ayrıştırılacak üç yolu vardır: basit (varsayılan), regex ve gevşek.

Aşağıdaki sorgu bir izleme ayrıştırır ve basit ayrıştırma varsayılan ilgili değerleri ayıklar. (StringConstant adlandırılır) ifade bir normal bir dize değeridir ve bu eşleşme katı: genişletilmiş sütunları gerekli türleri eşleşmelidir.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAN2UTU%2fDMAyG75X6H6xcxlCkpRlsUNQjN6gQ2wnEoevMFsiaKk2HJvHjabqvlI91l11QLrH12vETW5Zo4H411kmKEME0MdWZSISz2yVmpvaHhdEim3V979n3OrU%2fhFgZ8boaSZHiI0pMiipEY6FKnWKcLDB6EDlKkeEoneO0lKgpGGUSWYcUER9SKOw1LhcT1BHvU5AqfR%2bLKpbxXjDscRYMgF2FFyxkwRMFvX7ngCLXuBSqLO5%2bT9S%2ftrJuh54OI7g8iMFaMdhxGOy0GJz9i25w%2fjdG0IoRHNWNNe1ph2pwEKNlqI7HsEPley83vrfZCL73CXmiq%2fr32wA%2bhJnDOZAGEQHXBNIEIq4VSpXNbAIXkbjAO8UOmuz4bWoXlrhWWO0vqyA2%2bAcw2f7B1rORd60calat3jA1TRbq1A6NxsC%2bLdCoCuj3p74AKTs4pmcFAAA%3d)**\]**

```Kusto
let MyTrace = datatable (EventTrace:string)
[
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=23, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=15, lockTime=02/17/2016 08:40:00Z, releaseTime=02/17/2016 08:40:00Z, previousLockTime=02/17/2016 08:39:00Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=20, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=22, lockTime=02/17/2016 08:41:01Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41:00Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:00Z)'
];
MyTrace
| parse EventTrace with * "resourceName=" resourceName ", totalSlices=" totalSlices:long * "sliceNumber=" sliceNumber:long * "lockTime=" lockTime ", releaseTime=" releaseTime:date "," * "previousLockTime=" previouLockTime:date ")" *  
| project resourceName ,totalSlices , sliceNumber , lockTime , releaseTime , previouLockTime
```

Aşağıdaki sorgu bir izleme ayrıştırır ve ilgili değerleri kullanarak ayıklar `kind = regex`. StringConstant normal bir ifade olabilir.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAN2UQU%2fCQBCF7036HyZ7gWKRbVHQmgY9eNPGCCcoh9KOsLK0ZLtFMf54l6LQBgUuXEyTTbP7pt3vvclwlPC47IkgRHAhCqR6Rhyher%2fAWOb7TioFi8eGrg10rZLvO%2bAlkr0su5yF%2bIwcg1SVCEyTTIToBTN0n9gcOYuxG04wyjgKE2QiA56XpK7dNiFdvXrZbITCtZsm8CSc9piqpXbDajdsarWAXjkX1KFW3wSx%2fs8exVzggiVZ%2bvD7h5rXK5lRMU%2bHYV3uxaAHMehxGPS0GDb9F2nY9t8Y1kEM66g01rSnbarWXowDTXU8xqqpdG14o2vfE0HXPmEeCHX%2fKYsjNR8EjvEdtqMB3picAKme1zrGIKh%2f3NX7w5pLoEgLt6SM56c1PzpTq6oqYpIitMOTeAxAlKb6c3Wjs3GBbAzJJUV8UjQjP91BJztuOGryKbHvGwQgxxbJK4ayTFKKBbahQCkA2DX7C29veJJmBQAA)**\]**

```Kusto
let MyTrace = datatable (EventTrace:string)
[
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=23, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=15, lockTime=02/17/2016 08:40:00Z, releaseTime=02/17/2016 08:40:00Z, previousLockTime=02/17/2016 08:39:00Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=20, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=22, lockTime=02/17/2016 08:41:01Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41:00Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:00Z)'
];
MyTrace
| parse kind = regex EventTrace with "(.*?)[a-zA-Z]*=" resourceName @", totalSlices=\s*\d+\s*.*?sliceNumber=" sliceNumber:long  ".*?(previous)?lockTime=" lockTime ".*?releaseTime=" releaseTime ".*?previousLockTime=" previousLockTime:date "\\)"  
| project resourceName , sliceNumber , lockTime , releaseTime , previousLockTime
```

Aşağıdaki sorgu bir izleme ayrıştırır ve ilgili değerleri kullanarak ayıklar `kind = relaxed`. StringConstant bir normal bir dize değeridir ve bu eşleşme gevşek: genişletilmiş sütunların kısmen gerekli türleri aynı.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAN2US0%2fCQBDH7036HSZ7wZpN2BYFrenRGzZG4KLxUNoRVpYu2W5REj%2b83fKw9QE1kYvppTOZx%2f%2b3MxmBGm5WQxXFCAEkkS6%2bsUA4uV5iqku%2fn2nF04ljWw%2b21Sr9PoRS86fVQPAY71BglBUpCjOZqxjDaI7BLV%2bg4CkO4ikmuUBFQUsdiTIlC7wehcz8hvl8jCrwOhSEjGdDXuQyr%2b322h5zu8Au%2fDPmM%2feeglr32ROxULjkMs%2f63xfqXJowp0WPh%2bGe78VgBzFYMwx2XAyP%2fYtpeN7PGO5BDLfRNNa0x12q7l6MA0vVHMMslW09XtnW5iLY1hssIlXon%2fE0CYom0SsmQP6IMxz1%2b7%2b7AnXQdX6TNXMIvHA9hVMgNYEEqiaQuj5StXwh04kpUNVLqup3ETsCsoMxpavSSdXyi7NrIohJ%2foJDtoRbzybcMeFQjkjJZ4x1nYVWtEPtleHjjaGmCujnVu%2fWU75tHgYAAA%3d%3d)**\]**

```Kusto
let MyTrace = datatable (EventTrace:string)
[
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=23, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=15, lockTime=02/17/2016 08:40:00Z, releaseTime=02/17/2016 08:40:00Z, previousLockTime=02/17/2016 08:39:00Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=20, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=22, lockTime=02/17/2016 08:41:01Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41:00Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:00Z)'
];
MyTrace
| parse kind=relaxed "Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=NULL, sliceNumber=23, lockTime=02/17/2016 08:40:01, releaseTime=NULL, previousLockTime=02/17/2016 08:39:01)" with * "resourceName=" resourceName ", totalSlices=" totalSlices:long * "sliceNumber=" sliceNumber:long * "lockTime=" lockTime ", releaseTime=" releaseTime:date "," * "previousLockTime=" previousLockTime:date ")" *  
| project resourceName ,totalSlices , sliceNumber , lockTime , releaseTime , previousLockTime
```

## <a name="time-series-analysis"></a>Zaman serisi analizi

### <a name="make-series"></a>Oluştur-serisi

[**yapma serisi**](https://docs.microsoft.com/azure/kusto/query/make-seriesoperator): gibi satır gruplarını bir araya toplar [özetlemek](https://docs.microsoft.com/azure/kusto/query/summarizeoperator), ancak oluşturur (zaman) serisi değerlere göre her birleşimi başına vektör.

Aşağıdaki sorgu, zaman serisi storm olayları her gün sayısı için bir dizi döndürür. Sorgu eksik depo 0 sabiti doldurma her durum, bir üç aylık dönemin kapsar:

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUchNzE7VLU4tykwtVsizTc4vzSvR0FRISU1LLM0psTVQyM9TCC5JLCoJycxNVcjMUyhKzEtP1UhJLEktAYpoGBkYmOsaGAKRpo4CmqixrjFI1DBFUyGpEmRKSSoAazsM0n0AAAA%3d)**\]**

```Kusto
StormEvents
| make-series n=count() default=0 on StartTime in range(datetime(2007-01-01), datetime(2007-03-31), 1d) by State
```

(Zaman) serisi kümesi oluşturduktan sonra anormal şekiller, Dönemsel desenleri ve çok daha fazlasını algılamak için serisi işlevleri uygulayabilirsiniz.

Aşağıdaki sorgu, belirli bir gün içinde en çok olayın olan üst üç durumdan ayıklar:

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2OsQoCMRBEe8F%2f2DIBAzmvsLrSLzj7EC%2brBs3mSPbkBD%2feLDYibPVmZmdGziUdn0hct5s3JH9HU7FErEDDlBdipSHgxS8PHixkgpF94VNMCJGgeLqiCp6RG1F7aw%2fGdu30Dv5ob3qhXdBwfskXRmnElZECfDtdbbgq0qJwnqEX76%2fmyCW%2ftkV1Ek9pWSwgNdOt7foAJIuybs8AAAA%3d)**\]**

```Kusto
StormEvents
| make-series n=count() default=0 on StartTime in range(datetime(2007-01-01), datetime(2007-03-31), 1d) by State
| extend series_stats(n)
| top 3 by series_stats_n_max desc
| render timechart
```

Daha fazla bilgi için tam listesini gözden geçirin [serisi işlevleri](https://docs.microsoft.com/azure/kusto/query/scalarfunctions#series-processing-functions).

## <a name="advanced-aggregations"></a>Gelişmiş toplamalar

Temel toplamalar gibi ele aldığımız **sayısı** ve **özetlemek**, bu makalenin üst kısmındaki. Bu bölümde, daha gelişmiş seçenekler sunar.

### <a name="top-nested"></a>üst iç içe geçmiş

[**üst iç içe geçmiş**](https://docs.microsoft.com/azure/kusto/query/topnestedoperator): Her bir düzey detaya gitme önceki düzeyi değerlere dayalı olduğu hiyerarşik en iyi sonuçlar üretir.

Bu işleci veya Pano görselleştirme senaryolar için aşağıdaki gibi bir soruyu yanıtlamak gerekli olduğunda yararlıdır: "(Bazı toplama kullanarak); K1 üst N değerlerini bulma her biri için üst milyon değerlerini (başka bir toplama kullanarak); K2 nelerdir Bul ..."

Aşağıdaki sorgu içeren hiyerarşik bir tablo döndürür. `State` en üst düzeyinde, arkasından `Sources`.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjJL9DNSy0uSU1RMFLIT1MILkksSVVIqlQoLs3VcEpNz8zzSSzR1OHlQlJoDFaYX1qUTEilIUila16KT35yYklmfh6GcgDrXwk5jgAAAA%3d%3d)**\]**

```Kusto
StormEvents
| top-nested 2 of State by sum(BeginLat),
top-nested 3 of Source by sum(BeginLat),
top-nested 1 of EndLocation by sum(BeginLat)
```

### <a name="pivot-plugin"></a>pivot() plugin

[**pivot() plugin**](https://docs.microsoft.com/azure/kusto/query/pivotplugin): Çıkış tablodaki birden çok sütuna Giriş tablosunda bir sütunda yalnızca benzersiz değerler açarak bir tablo döndürür. İşleci, kalan tüm sütun değerleri son Çıkışta burada gerekli toplamaları gerçekleştirir.

Aşağıdaki sorgu, bir filtre uygular ve satırları sütunlara döner.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSgoys9KTS5RCC5JLEnVUQBLhFQWpILkyjNSi1IhMgrFJYlFJcXlmSUZCkqOPkoIabgOhYzEYgWl8My8FLBsalliTilIZ0FmWX6JBtgUTQDlv21NfQAAAA%3d%3d)**\]**

```Kusto
StormEvents
| project State, EventType
| where State startswith "AL"
| where EventType has "Wind"
| evaluate pivot(State)
```

### <a name="dcount"></a>dcount()

[**dcount()**](https://docs.microsoft.com/azure/kusto/query/dcount-aggfunction): Grup içinde tahmini bir ifadenin benzersiz değerlerin sayısını döndürür. Kullanım [ **Count() işlevi** ](https://docs.microsoft.com/azure/kusto/query/countoperator) tüm değerleri saymak için.

Aşağıdaki sorguda ayrı olarak sayar `Source` tarafından `State`.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlUIzi8tSk4tVrBVSEnOL80r0YAIaCokVSoElySWpAIAFKgSBDoAAAA%3d)**\]**

```Kusto
StormEvents
| summarize Sources = dcount(Source) by State
```

### <a name="dcountif"></a>dcountif()

[**dcountif()**](https://docs.microsoft.com/azure/kusto/query/dcountif-aggfunction): Farklı değer ifadesinin kendisi için koşulu değerlendirir satırlar için tahmini, true döndürür.

Aşağıdaki sorguyu farklı değerleri sayar `Source` nerede `DamageProperty < 5000`.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSspVqhRKEnMTlUwNDDg5apRKC7NzU0syqxKVQjOLy1KTi1WsFVISc4vzSvJTNOACOkouCTmJqanBhTlF6QWlVQq2CiYGhgYaCokVSoElySWpAIAuk%2fTX14AAAA%3d)**\]**

```Kusto
StormEvents 
| take 100
| summarize Sources = dcountif(Source, DamageProperty < 5000) by State
```

### <a name="dcounthll"></a>dcount_hll()

[**dcount_hll()**](https://docs.microsoft.com/azure/kusto/query/dcount-hllfunction): Hesaplar **dcount** HyperLogLog sonuçlarından (tarafından oluşturulan [**hll**](https://docs.microsoft.com/azure/kusto/query/hll-aggfunction) veya [**hll_merge** ](https://docs.microsoft.com/azure/kusto/query/hll-merge-aggfunction).

Aşağıdaki sorgu HLL algoritması sayısı üretmek için kullanır.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlXIyMkJSi1WsAUxNFwScxPTUwOK8gtSi0oqNRWSKhWSMvM0gksSi0pCMnNTdQwNcjUx9PumFqWnpkCMiM8FcTQgpoKVFhTlZ6UmlyikJOeX5pXEg6yB69EEAKm9wyCXAAAA)**\]**

```Kusto
StormEvents
| summarize hllRes = hll(DamageProperty) by bin(StartTime,10m)
| summarize hllMerged = hll_merge(hllRes)
| project dcount_hll(hllMerged)
```

### <a name="argmax"></a>arg_max()

[**arg_max()**](https://docs.microsoft.com/azure/kusto/query/arg-max-aggfunction): Bir ifade en üst düzeye çıkarır ve başka bir ifadenin değerini döndürür grubunda bir satırı bulur (veya * tüm satırı döndürülecek).

Aşağıdaki sorguyu her durumda son aşırı istek akışları nedeniyle raporun zaman döndürür.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQDzQyoLUhVsbRWU3HLy81OUQLLFpbm5iUWZVakKiUXp8bmJFRrBJYlFJSGZuak6ClqaCkmVCkCBklSQ2oKi%2fKzU5BKIgI4CkkLXvBQoA2YNAHO1S0OFAAAA)**\]**

```Kusto
StormEvents
| where EventType == "Flood"
| summarize arg_max(StartTime, *) by State
| project State, StartTime, EndTime, EventType
```

### <a name="makeset"></a>makeset()

[**makeset()**](https://docs.microsoft.com/azure/kusto/query/makeset-aggfunction): Bir dinamik bir ifade alan farklı değerler kümesini (JSON) dizisi grubunda döndürür.

Aşağıdaki sorgu, ne zaman sel her durum rapor edilmiştir ve ayrı bir değerler kümesinden bir dizi oluşturur her zaman döndürür.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWLQQ6CQBAE7yb8ocNJE76wR3mA8IEFOxF1mM3siIHweAVPHqsq1bianCeOnovDiveNRuzczokIAWX9VL2WW80vkWjDQuzuwqTmGQESH8z0Y%2bPRvB2EJ3QzvuTcvmR6Z%2b8%2fUf3NH6ZkMFeAAAAA)**\]**

```Kusto
StormEvents
| where EventType == "Flood"
| summarize FloodReports = makeset(StartTime) by State
| project State, FloodReports
```

### <a name="mvexpand"></a>mvexpand

[**mvexpand**](https://docs.microsoft.com/azure/kusto/query/mvexpandoperator): Koleksiyondaki her değer ayrı bir satır alır, böylece birden çok değerli koleksiyonlar dinamik olarak yazılmış bir sütundan genişletir. Genişletilmiş satırdaki diğer tüm sütunlar yinelenir. Makelist tersidir.

Aşağıdaki sorguyu örnek veri kümesi oluşturma ve göstermek için kullanarak oluşturur **mvexpand** özellikleri.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWOQQ6CQAxF9yTcoWGliTcws1MPIFygyk9EKTPpVBTj4Z2BjSz%2f738v7WF06r1vD2xcp%2bCoNq9yHDFYLIsvvW5Q0JybKYCco2omqnyNTxHW7oPFckbwajFZhB%2bIsE1trNZ0gi1dpuRmQ%2baC%2bjuuthS7Fbwvi%2f%2bP8lpGvAMP7Wr3A6BceSu7AAAA)**\]**

```Kusto
let FloodDataSet = StormEvents
| where EventType == "Flood"
| summarize FloodReports = makeset(StartTime) by State
| project State, FloodReports;
FloodDataSet
| mvexpand FloodReports
```

### <a name="percentiles"></a>percentiles()

[**percentiles()**](https://docs.microsoft.com/azure/kusto/query/percentiles-aggfunction): Tahmin için belirtilen döndürür [**derece en yakın yüzde birlik**](https://docs.microsoft.com/azure/kusto/query/percentiles-aggfunction) popülasyonun bir ifadesi tarafından tanımlanan. Yüzdelik dilim bölgede popülasyonunu yoğunluğunu doğruluğu bağlıdır. Yalnızca toplama içinde bağlamında kullanılan [**özetlemek**](https://docs.microsoft.com/azure/kusto/query/summarizeoperator).

Aşağıdaki sorgu storm süre yüzdebirliklerini hesaplar.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUUitKEnNS1FIKS1KLMnMz1OwVXDNSwnJzE1V0FUILkksKgGxQQrLM1KLUhHq7BQMirEI2ygYZ4CEi0tzcxOLMqtSFQpSi5KBlmXmpBZrwJTpKJjqKBgZACkgtgBiS1NNAEC7XiaYAAAA)**\]**

```Kusto
StormEvents
| extend duration = EndTime - StartTime
| where duration > 0s
| where duration < 3h
| summarize percentiles(duration, 5, 20, 50, 80, 95)
```

Aşağıdaki sorguda yüzdebirliklerini storm süresince durumuna göre hesaplar ve beş dakikalık depo verileri normalleştirir (`5m`).

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG1NSwrCMBTcC95hli1EKEpBQd31BHUvafOgAZNI8uIPD28SEBVcDDMM8%2bnZedNdyHKYz56gG5NVUNFL1s5ih86qgzaEBXqWnrPOwetEnj65PZrwx95iNWU7RGOk1w8C5avj6KLlNF64qjHcMWhbvXsCralFPmT6rZ%2fJj2lAnyh8pwWWTaKEdcKmLYul%2fgLODFs%2b4AAAAA%3d%3d)**\]**

```Kusto
StormEvents
| extend duration = EndTime - StartTime
| where duration > 0s
| where duration < 3h
| summarize event_count = count() by bin(duration, 5m), State
| summarize percentiles(duration, 5, 20, 50, 80, 95) by State
```

### <a name="cross-dataset"></a>Veri kümesi arası

Bu bölüm, daha karmaşık sorgular oluşturmak, veritabanları ve kümelerdeki veri tabloları ve sorgu arasında JOIN olanak sağlayan öğe kapsar.

### <a name="let"></a>let

[**izin**](https://docs.microsoft.com/azure/kusto/query/letstatement): Modüler ve yeniden kullanımını artırır. **İzin** deyimi sağlar, olası karmaşık ifadeyi birden çok parçaya bölmek, her bir adına bağlı ve bu parçaların birlikte oluşturun. A **izin** deyimi de kullanıcı tanımlı işlevleri ve görünümleri (ifadelerin sonuçlarını aramak gibi yeni bir tablo tablolar üzerinden) oluşturmak için kullanılabilir. İfadeleri bağlı olarak bir **izin** deyimi skalar türü tablo türü veya kullanıcı tanımlı işlev (lambdalar) olabilir.

Aşağıdaki örnek, bir tablo türü değişken oluşturur ve bir sonraki ifade kullanır.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAMtJLVHwyUzPKMnLzEsPLskvyi1WsOXlArNcy1LzSop5uWoUyjNSi1IVwPyQyoJUBVtbBSW4LiVrXq4coDGOZYk5iXnJGakkGQPXBTIGzSUgPVn5mXkKGmhmayrk5ykElySWpIKUpGQWl2TmJZdARACul3kY0gAAAA%3d%3d)**\]**

```Kusto
let LightningStorms =
StormEvents
| where EventType == "Lightning";
let AvalancheStorms =
StormEvents
| where EventType == "Avalanche";
LightningStorms
| join (AvalancheStorms) on State
| distinct State
```

### <a name="join"></a>join

[**birleştirme**](https://docs.microsoft.com/azure/kusto/query/joinoperator): Belirtilen sütunların her tablodan eşleşen değerlere göre yeni bir tablo oluşturmak için iki tablonun satırlarını birleştirir. Kusto birleşim türleri çeşitli destekler: **fullouter**, **iç**, **innerunique**, **leftanti**, **leftantisemi **, **leftouter**, **leftsemi**, **rightanti**, **rightantisemi**, **rightouter **, **rightsemi**.

Aşağıdaki örnekte, bir iç birleştirme ile iki tabloyu birleştirir.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA8tJLVGIULBVSEksAcKknFQN79RKq+KSosy8dB2FsMSc0lRDq5z8vHRNXq5oXi4FIFBPVNcx1IGyk9R1jJDYxjB2srqOCS9XrDUvVw7Qhkj8Nhih2wA0ydAAySgjZI4xnJMCtMQAYkuEQo1CVn5mnkJ2Zl6KbWZeXmoR0Nb8PAWgZQAFPLdO5AAAAA==)**\]**

```Kusto
let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X 
| join kind=inner Y on Key
```

> [!TIP]
> Kullanım **burada** ve **proje** birleştirme önce giriş tablosu içindeki satırları ve sütunları sayıda azaltmak için işleçler. Bir tablonun her zaman diğerinden daha küçük ise birleştirme (piped) sol tarafındaki kullanın. Birleştirme eşleşme sütunları aynı ada sahip olmalıdır. Kullanım **proje** tablolardan birinde bir sütunu yeniden adlandırmak gerekirse işleci.

### <a name="serialize"></a>Seri hale getirme

[**seri hale getirme**](https://docs.microsoft.com/azure/kusto/query/serializeoperator): Serileştirilmiş veriler gibi gerektiren işlevleri kullanabilirsiniz. böylece satır serileştiren **row_number()**.

Verileri seri hale getirilmiş olduğundan aşağıdaki sorgu başarılı olur.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIzi%2fNK9HQVEiqVAguSSxJBcumFmUm5gBlQZzUipLUvBSFovzy%2bLzS3KTUIgVbJI6GJgB4pV4NWgAAAA%3d%3d)**\]**

```Kusto
StormEvents
| summarize count() by State
| serialize
| extend row_number = row_number()
```

Satır kümesi, ayrıca bir sonuç ise sıralanmış olarak değerlendirilir: **sıralama**, **üst**, veya **aralığı** işleçleri, ardından isteğe bağlı olarak **proje**, **proje koyma**, **genişletmek**, **burada**, **ayrıştırma**, **mvexpand**, veya **ele** işleçleri.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIzi%2fNK9HQVEiqVAguSSxJBcvmF5XABRQSi5NBgqkVJal5KQpF%2beXxeaW5SalFCrZIHA1NAEGimf5iAAAA)**\]**

```Kusto
StormEvents
| summarize count() by State
| sort by State asc
| extend row_number = row_number()
```

### <a name="cross-database-and-cross-cluster-queries"></a>Platformlar arası ve küme içi sorguları

[Platformlar arası ve küme içi sorguları](https://docs.microsoft.com/azure/kusto/query/cross-cluster-or-database-queries): Aynı küme üzerindeki bir veritabanı olarak başvurarak Sorgulayabileceğiniz `database("MyDatabase").MyTable`. Bir uzak kümesi üzerinde bir veritabanı olarak başvurarak Sorgulayabileceğiniz `cluster("MyCluster").database("MyDatabase").MyTable`.

Aşağıdaki sorgu, bir kümeden olarak adlandırılır ve verileri sorgular `MyCluster` kümesi. Bu sorguyu çalıştırmak için küme adınızı ve veritabanı adını kullanın.

```Kusto
cluster("MyCluster").database("Wiki").PageViews
| where Views < 2000
| take 1000;
```

### <a name="user-analytics"></a>Kullanıcı analizi

Bu bölüm, öğeleri ve kullanıcı davranış analizi Kusto içinde gerçekleştirmek için ne kadar kolay olduğunu gösteren sorguları içerir.

### <a name="activitycountsmetrics-plugin"></a>activity_counts_metrics eklentisi

[**activity_counts_metrics eklentisi**](https://docs.microsoft.com/azure/kusto/query/activity-counts-metrics-plugin): Yararlı etkinlik ölçümlerinin (toplam sayısı değerleri, ayrı sayım değerleri, yeni değerlerin ayrı sayım ve toplam ayrı sayım) hesaplar. Ölçümleri her zaman penceresi için hesaplanır ve ardından bunlar karşılaştırıldığında ve için ve tüm önceki zaman pencereleri toplanır.

Aşağıdaki sorgu, etkinlik sayısını hesaplayarak günlük kullanıcılar tarafından benimsenmesine analiz eder.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAJXSPQvCMBAG4L3Q%2f5CtFlLoFyiVDn4M6mqdREpsggTaKs1VEfzxXm0LDiEimcJz3CW8VwogClgDKWcgQFZiEvrB1PNnnh%2b4c9sqsUDUXMPxyA9Z8%2bsjDfhwz0hKsBzPuRSTgxLNlicKGllfKMmwBw6sbsnY0bWto205C4cS3Rso2tpgO4MtDbbSWvixzGD6eb1ttBYZev42%2fbzI8L%2fe9n9b3NkJQ8xs60XEnZUt1hBWgLxLeObFta1B5ZXAKAs1BPuVKO03iXb7gp36tXDfExVB%2f2ICAAA%3d)**\]**

```Kusto
let start=datetime(2017-08-01);
let end=datetime(2017-08-04);
let window=1d;
let T = datatable(UserId:string, Timestamp:datetime)
[
'A', datetime(2017-08-01),
'D', datetime(2017-08-01),
'J', datetime(2017-08-01),
'B', datetime(2017-08-01),
'C', datetime(2017-08-02),
'T', datetime(2017-08-02),
'J', datetime(2017-08-02),
'H', datetime(2017-08-03),
'T', datetime(2017-08-03),
'T', datetime(2017-08-03),
'J', datetime(2017-08-03),
'B', datetime(2017-08-03),
'S', datetime(2017-08-03),
'S', datetime(2017-08-04),
];
T
| evaluate activity_counts_metrics(UserId, Timestamp, start, end,
window)
```

### <a name="activityengagement-plugin"></a>activity_engagement eklentisi

[**activity_engagement eklentisi**](https://docs.microsoft.com/azure/kusto/query/activity-engagement-plugin): Etkinlik katılım oranı kayan bir zaman çizelgesi pencerede kimliği sütuna göre hesaplar. **activity_engagement eklentisi** günlük etkin kullanıcı, WAU ve MAU (günlük, haftalık ve aylık etkin kullanıcı) hesaplamak için kullanılabilir.

Aşağıdaki sorgu, uygulamanın toplam farklı kullanıcı uygulamayı haftalık olarak bir taşıma yedi gün penceresinde kullanarak günlük karşılaştırılan kullanarak toplam farklı kullanıcı sayısının oranı döndürür.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG1RQWrDMBC8G%2fyHvUVOHGy1lByKD6GBviDkUIoR1tpVsS0jr0MCeXxXiigpVAiBVjOzM6uigHcc0SlCcGrUdgCtSIFtYZnRgWrInA0ZnNOkR4J6JuUIKo9CMgOKp1LutqXknb1GDI76P8RzQHCXDqHW6gqt43ZRkeydNxNOIHWa3AAv5Ctei2xvx06IQNtGTlZInT0AHQN9BpFt5EO59kHmKvQVUUivX8q1y3L4c9%2fIks%2bt5LoMwsMZLxMrgtHVXcb7pOuEthWemEFvBkPARL%2fSpCjgTfXN0vuBHvbH4rQ%2fsikyNjg6q37xL3GsV47cqQ4HHEl8rIxefeZhNHmMmIehsB2dp8nunnZy9hsbiriDWuqTWqpfxdBsLb2ZGzhm8y%2f6b2i%2bWO8HLFcMGe8BAAA%3d)**\]**

```Kusto
// Generate random data of user activities
let _start = datetime(2017-01-01);
let _end = datetime(2017-01-31);
range _day from _start to _end step 1d
| extend d = tolong((_day - _start)/1d)
| extend r = rand()+1
| extend _users=range(tolong(d*50*r), tolong(d*50*r+100*r-1), 1)
| mvexpand id=_users to typeof(long) limit 1000000
// Calculate DAU/WAU ratio
| evaluate activity_engagement(['id'], _day, _start, _end, 1d, 7d)
| project _day, Dau_Wau=activity_ratio*100
| render timechart
```

> [!TIP]
> Günlük etkin kullanıcı/MAU hesaplarken son veri ve taşıma penceresi süresi (OuterActivityWindow) değiştirin.

### <a name="activitymetrics-plugin"></a>activity_metrics eklentisi

[**activity_metrics eklentisi**](https://docs.microsoft.com/azure/kusto/query/activity-metrics-plugin): Geçerli dönem pencerenin önceki dönem penceresine karşılaştırması göre yararlı etkinlik ölçümlerinin (ayrı sayım değerleri, yeni değerleri, elde tutma oranı ve karmaşıklık oranı ayrı sayım) hesaplar.

Aşağıdaki sorgu, belirli bir veri kümesi için değişim sıklığı ve elde tutma oranı hesaplar.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG2SwW7CMAyG70i8g2%2bk0KoNE%2bIwscsOe4hpqqLGQFjaVKkLVNrDLw7RxjRyqBTr%2fz%2f3t1OW8IYdekUIXnXataAVKXB7GAf0oBoyZ0MGh%2fnMIkE9kPIEO1YhmRbFupLbopJFtc6ekwY7%2fV%2bxKZ4kK0KXA0Kt1QR7H9olIrmbbyDsQer57AvwSlxhFjnruoMQ0VYkT1ZKnd0JfRByBpGt5F255iDDLvYVCaSXm2rpsxz%2b3FfrKnwLGeoygtszXvtABKN3Nwz%2fJ009ur1gYwbWtIZAVvGw53JEn%2fK9PJwSi3rvTthQlOWBPp%2bVJbwq24yWN3FB%2fLQTeAwByLgOeD8x0lnZkRVpL1PdInnTDOJ9YfTiI0%2fE24DyONIctvpB0x94zfBlSJBDcxz97509PgDCM%2bAMzTEgvwEO44wSMAIAAA%3d%3d)**\]**

```Kusto
// Generate random data of user activities
let _start = datetime(2017-01-02);
let _end = datetime(2017-05-31);
range _day from _start to _end step 1d
| extend d = tolong((_day - _start)/1d)
| extend r = rand()+1
| extend _users=range(tolong(d*50*r), tolong(d*50*r+200*r-1), 1)
| mvexpand id=_users to typeof(long) limit 1000000
| where _day > datetime(2017-01-02)
| project _day, id
// Calculate weekly retention rate
| evaluate activity_metrics(['id'], _day, _start, _end, 7d)
| project _day, retention_rate*100, churn_rate*100
| render timechart
```

### <a name="newactivitymetrics-plugin"></a>new_activity_metrics plugin

[**new_activity_metrics eklentisi**](https://docs.microsoft.com/azure/kusto/query/new-activity-metrics-plugin): Yararlı etkinlik ölçümlerinin (ayrı sayım değerleri, yeni değerleri, elde tutma oranı ve karmaşıklık oranı ayrı sayım) için yeni kullanıcılar kohortu hesaplar. Bu eklenti kavramını benzer [**activity_metrics eklentisi**](https://docs.microsoft.com/azure/kusto/query/activity-metrics-plugin), ancak yeni kullanıcılar üzerinde odaklanmaktadır.

Aşağıdaki sorguyu yeni kullanıcılar kohortu (ilk haftasında gelen kullanıcılar) için bir hafta üzerinden hafta penceresi ile bekletme ve değişim sıklığı bir oran hesaplar.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG1Ry27DIBC8W%2fI%2f7C04wbJJFeVQ5VapP9BbVVnIrGMaGyy8eVjqxxcwh1QqBx7LzCwzVBW8o0EnCcFJo%2bwISpIE28F1RgeyJX3TpHHOswEJmpmkIzgFFJIeke1rcSzrQ1mL4jVh0Kj%2fEC8R4bucEd7kAp3z3ZIg2ZU2E04gVJ79AD4oVIIU2cGaM2OBVSZKUQlVPOGcxwUHrNiJp3ITbMyn2JUlHbU91FtXcPhz3u1rP5fC10UUHm%2f4mLwiaHVaZcIzaZnQdiwQCxj0qAlEHUeeVRV8yAuCNcMC1CN02s0Ed8QLtLa33igbpK9M0skRCd3q4CaHa%2fgBg%2fcmJb40%2ft7pdmafG602XzxExpN3HsPicFQ8z1IcQWhy9htbisk2EU92XZ1vZkhb04Sv5tD2V7fufwFYtolnAgIAAA%3d%3d)**\]**

```Kusto
// Generate random data of user activities
let _start = datetime(2017-05-01);
let _end = datetime(2017-05-31);
range Day from _start to _end step 1d
| extend d = tolong((Day - _start)/1d)
| extend r = rand()+1
| extend _users=range(tolong(d*50*r), tolong(d*50*r+200*r-1), 1)
| mvexpand id=_users to typeof(long) limit 1000000
// Take only the first week cohort (last parameter)
| evaluate new_activity_metrics(['id'], Day, _start, _end, 7d, _start)
| project from_Day, to_Day, retention_rate, churn_rate
```

### <a name="sessioncount-plugin"></a>session_count eklentisi

[**session_count eklentisi**](https://docs.microsoft.com/azure/kusto/query/session-count-plugin): Oturum kimliği sütununda bir zaman çizelgesi üzerinde temel sayısı hesaplar.

Aşağıdaki sorgu oturumlarının sayısını döndürür. Bir oturum oturum geri Görünüm penceresi 41 zaman dilimini durumdayken bir kullanıcı kimliği en az bir kez 100 zamanı yuvaları bir zaman çerçevesi görünürse etkin olarak kabul edilir.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWPQYvCQAyF74X%2bh3dZUCjYgfUgMkcP3r2XoZPqaM3INK4u7I%2ffzOwiNQRC8pKPl5EEnXfiYJEcHwmHcKUxMGFI8QoDidhoYBK6wdTVD%2bgpxB5dd6FvPSuzcwyMS2BvAzMlLP5gez%2fDrNt%2fCN4Z1iwRua5Kk2GPE6WZkY%2bMsRZt1m4pnqmXl9qouK2r1Qo75cUB5RlPQ%2bAgoWDzpPj%2bcuPdCWGiaVKp6%2bOdZbH3zYxmNFuNUhp8mmU%2bTWpWv8or%2fckl%2bQXutT48NwEAAA%3d%3d)**\]**

```Kusto
let _data = range Timeline from 1 to 9999 step 1
| extend __key = 1
| join kind=inner (range Id from 1 to 50 step 1 | extend __key=1) on __key
| where Timeline % Id == 0
| project Timeline, Id;
// End of data definition
_data
| evaluate session_count(Id, Timeline, 1, 10000, 100, 41)
| render linechart
```

### <a name="funnelsequence-plugin"></a>funnel_sequence eklentisi

[**funnel_sequence eklentisi**](https://docs.microsoft.com/azure/kusto/query/funnel-sequence-plugin): Ayrı sayım durumları bir dizi almış kullanıcılar, hesaplar; neden olur ya da sırası tarafından takip önceki ve sonraki durumları dağılımını gösterir.

Aşağıdaki sorgu, önce ve sonra tüm hortum olayları 2007'de hangi olay ne olduğunu gösterir.

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAGWOPYvCQBCG%2b0D%2bw3QmEEmieIqNVQrBRgxYiMhcdqKLyWzcnQiCP95V70DuYIrh5Xk%2f0hRWxpw1H8EwbMTYtrgSiwMnKNqJrtw8DNIU1vkcticUOGHXETv4ptpYgtJYRmWAnrbFGx39QbEWsv%2fIj7YwuHsZmx6FoO6ZqTk4uvTEFUVFp51RtFSJH4hWSt1SAsqj4r9olGXTYZb7i5Mw%2bJRnvzLkKhl%2fTXzAq668dc%2bAG2Orq2g3%2bBk22MfxA23MLGQQAQAA)**\]**

```Kusto
// Looking on StormEvents statistics:
// Q1: What happens before Tornado event?
// Q2: What happens after Tornado event?
StormEvents
| evaluate funnel_sequence(EpisodeId, StartTime, datetime(2007-01-01), datetime(2008-01-01), 1d,365d, EventType, dynamic(['Tornado']))
```

### <a name="funnelsequencecompletion-plugin"></a>funnel_sequence_completion eklentisi

[**funnel_sequence_completion eklentisi**](https://docs.microsoft.com/azure/kusto/query/funnel-sequence-completion-plugin): Huni tamamlanmış dizisi adımlarının farklı süreler içinde hesaplar.

Aşağıdaki sorgu dizisi tamamlama Huni denetler: `Hail -> Tornado -> Thunderstorm -> Wind` "Genel" sürelerinin bir saate dört saat ve bir gün içinde (`[1h, 4h, 1d]`).

**\[**[**Sorguyu çalıştırmak için tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA12QTYvCMBCG74L/YW6tkIV2XT9g8SjsnlvwICKhM9JAOqlJqrj4402CW0RIIB/PPLwzmjwcnZfWwwZQevKqo/yzKFYfRRnW7Hs60ZEhxjdi/UZcFaO5VuqPAjhfLvD/w9F5IG7iM95YdqrJ99mPVDoTkNXGskSTju3ASNZ5Y7t43wVhdhj9PVll0L1aylbAV9glJqyKldsLsXfTyR3oIvUQAsNpYCY95jg2puuDUhnOt71yBukXBVRxCnVoTjwnIlLX4rUzAUlf3/pEPYViDDd7AOyqowFQAQAA)**\]**

```Kusto
let _start = datetime(2007-01-01);
let _end = datetime(2008-01-01);
let _windowSize = 365d;
let _sequence = dynamic(['Hail', 'Tornado', 'Thunderstorm', 'Wind']);
let _periods = dynamic([1h, 4h, 1d]);
StormEvents
| evaluate funnel_sequence_completion(EpisodeId, StartTime, _start, _end, _windowSize, EventType, _sequence, _periods)
```

## <a name="functions"></a>İşlevler

Bu bölümde ele alınmaktadır [ **işlevleri**](https://docs.microsoft.com/azure/kusto/query/functions): sunucuda depolanan yeniden sorgular. İşlevler, sorgular ve (özyinelemeli işlevler desteklenmez) diğer işlevler tarafından çağrılabilir.

> [!NOTE]
> Salt okunur Yardım kümede işlevleri oluşturulamıyor. Kendi test kümesi için bu bölümü kullanın.

Aşağıdaki örnek, bir durum adı alan bir işlev oluşturur (`MyState`) bağımsız değişken olarak.

```Kusto
.create function with (folder="Demo")
MyFunction (MyState: string)
{
StormEvents
| where State =~ MyState
}
```

Aşağıdaki örnek durumu Teksas'ın veri alan bir işlev çağırır.

```Kusto
MyFunction ("Texas")
| summarize count()
```

Aşağıdaki örnek ilk adımda oluşturulan işlev siler.

```Kusto
.drop function MyFunction
```

## <a name="next-steps"></a>Sonraki adımlar

[Kusto sorgu dili başvurusu](https://aka.ms/kustolangref)
