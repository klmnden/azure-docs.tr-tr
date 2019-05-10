---
title: Azure İzleyici'de günlük sorguları kullanmaya başlama | Microsoft Docs
description: Bu makalede, başlarken bir öğretici sağlar. Azure İzleyici'de günlük sorguları yazma.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/06/2018
ms.author: bwren
ms.openlocfilehash: a8da60850dae600129e0bc60fb574bfa4d3972db
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415889"
---
# <a name="get-started-with-azure-monitor-log-queries"></a>Azure İzleyici günlük sorguları kullanmaya başlama


> [!NOTE]
> Tamamlamanız gereken [Azure İzleyici Log Analytics ile çalışmaya başlama](get-started-portal.md) Bu öğreticiyi tamamlamadan önce.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu öğreticide, Azure İzleyici günlük sorguları yazma öğreneceksiniz. Size nasıl yardımcı olacak için:

- Sorguları yapısı anlama
- Sorgu sonuçlarını sıralama
- Sorgu sonuçlarını filtreleme
- Bir zaman aralığı belirtin
- Sonuçların dahil edileceği hangi alanları seçin
- Tanımlamak ve özel alanları kullanın
- Toplama ve Grup sonuçları


## <a name="writing-a-new-query"></a>Yeni bir sorgu yazma
Sorgular, bir tablo adı ile başlatabilir veya *arama* komutu. Sorgu için açık bir kapsam tanımlar ve sorgu performansı hem sonuçlarının ilgi düzeyi artıran bir tablo adı ile başlamanız gerekir.

> [!NOTE]
> Azure İzleyici tarafından kullanılan Kusto sorgu dili, büyük/küçük harf duyarlıdır. Dil anahtar sözcükleri genellikle küçük yazıldığı. Tablo veya sütun adları bir sorguda kullanırken, şema bölmesinde gösterildiği gibi doğru harf kullandığınızdan emin olun.

### <a name="table-based-queries"></a>Tablo tabanlı sorgular
Azure İzleyici günlük verileri tablolarda düzenler, her birden çok sütundan oluşan. Tüm tabloları ve sütunları analiz portalı Log analytics'te şema bölmesinde gösterilir. İlginizi çeken ve ardından veri göz atın, bir tablo tanımlayın:

```Kusto
SecurityEvent
| take 10
```

Yukarıda gösterilen sorguyu 10 sonuçlarını döndürür *SecurityEvent* tablodaki belirli bir sıraya göre. Bir bakışta bir tabloyu alıp yapısını ve içeriğini anlamak için çok yaygın bir yolu budur. Nasıl oluşturulduğunu inceleyelim:

* Sorgu tablosu adı ile başlayan *SecurityEvent* -bu bölümü sorgunun kapsamını tanımlar.
* Dikey çubuk (|) karakterini komutları, bu nedenle ayıran birincisini giriş aşağıdaki komutun çıkışı. Herhangi bir sayıda yöneltilen öğeleri ekleyebilirsiniz.
* Kanal aşağıdaki **ele** tablodan rastgele kayıtları belirli bir dizi döndüren komutu.

Biz aslında sorgu bile eklemeden çalıştırabilirsiniz `| take 10` -, hala geçerli, ancak en fazla 10.000 sonuçlar döndürebilir.

### <a name="search-queries"></a>Arama sorguları
Arama sorguları daha az yapılandırılmış ve genellikle daha sütunlarından birini belirli bir değer içeren bir kayıt bulmak için uygun şunlardır:

```Kusto
search in (SecurityEvent) "Cryptographic"
| take 10
```

Bu sorgu arar *SecurityEvent* "Şifreleme" ifadesini içeren bir kayıt tablosu. Bu kayıtları, 10 kayıt döndürdü ve görüntülenir. Biz atlarsanız `in (SecurityEvent)` parçası ve yalnızca çalıştırma `search "Cryptographic"`, aramaya Git *tüm* uzun sürer ve daha az verimli tabloların.

> [!NOTE]
> Varsayılan olarak, bir zaman aralığı _son 24 saat_ ayarlanır. Farklı bir aralık kullanmak için Saat Seçici kullanın (yanında bulunan *Git* düğmesi) veya açık bir zaman Ekle sorgunuz için Aralık filtresi.

## <a name="sort-and-top"></a>Sıralama ve üst
Sırada **ele** olan birkaç kayıtları almak yararlı sonuçlar seçilir ve belirli bir sırada görüntülenir. Sıralı bir görünüm elde edin, şunları yapabilirsiniz: **sıralama** tercih edilen bir sütuna göre:

```Kusto
SecurityEvent   
| sort by TimeGenerated desc
```

Çok fazla sonuç ancak döndürebilir ve ayrıca biraz zaman alabilir. Yukarıdaki sorguda sıralar *tamamını* TimeGenerated sütunuyla SecurityEvent tablo. Analytics portalını ardından 10. 000'yalnızca kayıt göstermek sınırlar. Bu yaklaşım Elbette en uygun değil.

Yalnızca en son 10 kayıtları almak için en iyi yolu kullanmaktır **üst**, sunucu tarafında tüm tabloyu sıralar ve ardından üst kayıtlar döndürür:

```Kusto
SecurityEvent
| top 10 by TimeGenerated
```

Azalan düzende, genellikle atlamak için sıralama varsayılan **desc** bağımsız değişken. Çıkış şuna benzeyecektir:

![İlk 10](media/get-started-queries/top10.png)


## <a name="where-filtering-on-a-condition"></a>Burada: bir koşula göre filtreleme
Filtreleri adlarına göre belirtildiği gibi belirli bir koşul tarafından verileri filtreleyin. Bu, ilgili bilgileri sorgu sonuçlarını sınırlamak için en yaygın yoludur.

Bir sorguya bir filtre eklemek için **burada** işleci, bir veya daha fazla koşul tarafından izlenen. Örneğin, aşağıdaki sorguyu yalnızca döndürür *SecurityEvent* kayıtları _düzeyi_ eşittir _8_:

```Kusto
SecurityEvent
| where Level == 8
```

Filtre koşulları yazarken, aşağıdaki ifadeler kullanabilirsiniz:

| İfade | Açıklama | Örnek |
|:---|:---|:---|
| == | Eşitlik denetleyin<br>(büyük-küçük harfe duyarlı) | `Level == 8` |
| =~ | Eşitlik denetleyin<br>(büyük-küçük harf duyarsız) | `EventSourceName =~ "microsoft-windows-security-auditing"` |
| !=, <> | Eşitsizlik denetleyin<br>(her iki ifade de aynıdır) | `Level != 4` |
| *ve*, *veya* | Koşullar arasında gerekli| `Level == 16 or CommandLine != ""` |

Birden çok koşullarına göre filtre uygulamak için kullanabilir **ve**:

```Kusto
SecurityEvent
| where Level == 8 and EventID == 4672
```

veya birden çok kanal **burada** birbiri ardına öğeleri biri:

```Kusto
SecurityEvent
| where Level == 8 
| where EventID == 4672
```
    
> [!NOTE]
> Doğru türüne karşılaştırma yapmak için bunları cast gerekebilir. böylece farklı değerleri olabilir. Örneğin, SecurityEvent *düzeyi* sütunu olduğundan, dize türünde dönüştürmelisiniz, bir sayısal türe gibi *int* veya *uzun*, sayısal işleçleri kullanabilmeniz için önce: `SecurityEvent | where toint(Level) >= 10`

## <a name="specify-a-time-range"></a>Bir zaman aralığı belirtin

### <a name="time-picker"></a>Saat Seçici
Saat Seçici Çalıştır düğmesinin yanındaki ve biz yalnızca son 24 saat kayıtları sorguladığınız gösterir. Bu, tüm sorguları uygulanan varsayılan zaman aralığıdır. Son bir saat yalnızca kayıtları ulaşmak için _son bir saat_ ve sorguyu yeniden çalıştırın.

![Saat Seçici](media/get-started-queries/timepicker.png)


### <a name="time-filter-in-query"></a>Sorgu zaman filtresi
Sorgu süresi filtre ekleyerek de kendi zaman aralığı tanımlayabilirsiniz. Tablo adı hemen sonra süresi filtre yerleştirmek idealdir: 

```Kusto
SecurityEvent
| where TimeGenerated > ago(30m) 
| where toint(Level) >= 10
```

Yukarıdaki süresi filtre `ago(30m)` bu sorgu, kayıtları yalnızca son 30 dakika döndürür. Bu nedenle "30 dakika önce" anlamına gelir. Diğer zaman birimlerini gün dahil (2B) (25m) dakika ve saniye (10 s).


## <a name="project-and-extend-select-and-compute-columns"></a>Proje ve genişlet: seçin ve sütunları işlem
Kullanım **proje** sonuçların dahil edileceği belirli sütunları seçmek için:

```Kusto
SecurityEvent 
| top 10 by TimeGenerated 
| project TimeGenerated, Computer, Activity
```

Yukarıdaki örnek aşağıdaki çıkışı üretir:

![Sorgu proje sonuçları](media/get-started-queries/project.png)

Ayrıca **proje** sütunları yeniden adlandırma ve yenilerini tanımlamak için. Aşağıdaki örnek proje, aşağıdakileri yapmak için kullanır:

* Yalnızca belirli *bilgisayar* ve *TimeGenerated* özgün sütun.
* Yeniden adlandırma *etkinlik* sütuna *EventDetails*.
* Adlı yeni bir sütun oluşturun *EventCode*. **Substring()** işlevi yalnızca ilk dört karakter etkinlik alanından almak için kullanılır.


```Kusto
SecurityEvent
| top 10 by TimeGenerated 
| project Computer, TimeGenerated, EventDetails=Activity, EventCode=substring(Activity, 0, 4)
```

**genişletme** özgün tüm sütunları sonuç kümesinde tutar ve bulunmakla tanımlar. Aşağıdaki sorguda kullandığı **genişletmek** eklemek için bir *localtime* yerelleştirilmiş TimeGenerated değeri içeren sütun.

```Kusto
SecurityEvent
| top 10 by TimeGenerated
| extend localtime = TimeGenerated -8h
```

## <a name="summarize-aggregate-groups-of-rows"></a>Özetlenmektedir: toplam satır gruplarını
Kullanım **özetlemek** kayıt gruplarını göre bir veya daha fazla sütun belirleyin ve toplamalar uygulayabilirsiniz. En yaygın kullanımı **özetlemek** olduğu *sayısı*, her grupta sonuç sayısını döndürür.

Aşağıdaki sorgu tüm incelemeleri *Perf* son bir saat kayıtlardan göre gruplar *ObjectName*ve her gruptaki kayıtları sayar: 
```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize count() by ObjectName
```

Bazen birden fazla boyuta göre grupları tanımlamak için mantıklıdır. Bu değerler her benzersiz birleşimi ayrı bir grubu tanımlar:

```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize count() by ObjectName, CounterName
```

Başka bir ortak her grubunda matematik veya istatistik hesaplamalar gerçekleştirmek için kullanılır. Örneğin, aşağıdaki ortalamasını hesaplar *Ort* her bilgisayar için:

```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize avg(CounterValue) by Computer
```

Ne yazık ki, biz farklı performans sayaçlarının arada olduğundan bu sorgunun sonuçlarının anlamsız. Bu daha anlamlı olacak şekilde, biz ayrı ayrı her bir birleşimi için ortalama hesaplamanız gerekir *CounterName* ve *bilgisayar*:

```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize avg(CounterValue) by Computer, CounterName
```

### <a name="summarize-by-a-time-column"></a>Bir zaman sütun tarafından özetleme
Sonuçları gruplandırma de saat sütunu veya başka bir sürekli değer temel alabilir. Yalnızca özetleme `by TimeGenerated` bu benzersiz değerler olduğundan, gruplar için her tek bir milisaniyeden kısa zaman aralığı üzerinde ancak oluşturursunuz. 

Sürekli değerlerine göre grupları oluşturmak için aralığı kullanılarak yönetilebilir birimler halinde bölmek en iyisidir **bin**. Aşağıdaki sorguyu analiz eder *Perf* boş bellek ölçen kayıtları (*Kullanılabilir MBayt*) belirli bir bilgisayardaki. Bu, son 7 gün içindeki her 1 saatlik dönem ortalama değerini hesaplar:

```Kusto
Perf 
| where TimeGenerated > ago(7d)
| where Computer == "ContosoAzADDS2" 
| where CounterName == "Available MBytes" 
| summarize avg(CounterValue) by bin(TimeGenerated, 1h)
```

Çıktı daha anlaşılır hale getirmek için bir zaman grafiği görüntülemek için zaman içinde kullanılabilir bellek gösteren seçin:

![Zaman içinde sorgu bellek](media/get-started-queries/chart.png)



## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [arama sorguları yazma](search-queries.md)
