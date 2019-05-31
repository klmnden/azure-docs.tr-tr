---
title: Azure İzleyici Log Analytics ile çalışmaya başlama | Microsoft Docs
description: Bu makale, Log Analytics sorguları yazma Azure Portalı'nda kullanmaya yönelik bir öğretici sağlar.
services: log-analytics
author: bwren
manager: carmonm
ms.service: log-analytics
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: bwren
ms.openlocfilehash: af01ebdc72df096b45c4ca4e755b2ed3880bab65
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255271"
---
# <a name="get-started-with-azure-monitor-log-analytics"></a>Azure İzleyici Log Analytics ile çalışmaya başlama

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu öğreticide, Azure İzleyici Log Analytics Azure portalında Azure İzleyici günlük sorguları yazma için nasıl kullanılacağını öğreneceksiniz. Size nasıl yardımcı olacak için:

- Basit Sorgu yazma
- Verilerinizin şemasını anlama
- Filtre, sıralama ve Grup sonuçları
- Bir zaman aralığı uygulayın
- Grafikler oluşturun
- Kaydet ve sorguları
- Verme ve sorguları paylaşma


## <a name="meet-log-analytics"></a>Log Analytics'e karşılamak
Log Analytics, yazma ve Azure İzleyici günlük sorguları yürütmek için kullanılan web bir araçtır. Seçerek açın **günlükleri** Azure İzleyici menüsünde. Yeni bir boş sorgu ile başlar.

![Giriş sayfası](media/get-started-portal/homepage.png)



## <a name="basic-queries"></a>Temel sorgular
Sorgular, arama terimleri, eğilimleri belirlemenize, biçimlerini çözümleme ve verilerinizi temel alan birçok öngörüden sağlamak için kullanılabilir. Temel bir sorgu başlatın:

```Kusto
Event | search "error"
```

Bu sorgu arar _olay_ herhangi bir özelliği "error" terimini içeren kayıtlar için tablo.

Sorgular, bir tablo adı ile başlatabilir veya **arama** komutu. Yukarıdaki örnekte tablo adı ile başlayan _olay_, sorgunun kapsamını tanımlar. Aşağıdaki komutun girdi olarak ilk çıktısını hizmet için çizgi (|) karakter komutları ayırır. Herhangi bir sayıda komutları için tek bir sorgu ekleyebilirsiniz.

Aynı sorgu yazmak için başka bir yolu şu şekilde olur:

```Kusto
search in (Event) "error"
```

Bu örnekte, **arama** kapsamı _olay_ tablo ve bu tablodaki tüm kayıtları "error" terimini arayan aranır.

## <a name="running-a-query"></a>Bir sorgu çalıştırma
Tıklayarak bir sorgu çalıştırın **çalıştırma** düğme veya tuşlarına basarak **Shift + Enter**. Çalıştırılacak kod ve döndürülen verileri belirleyen aşağıdaki ayrıntıları göz önünde bulundurun:

- Satır sonları: Tek bir kesme sorgunuzu daha anlaşılır hale getirir. Birden çok satır sonları ayrı sorgulara bölün.
- İmleç: İmlecinizi sorgusunda çalıştırmak üzere bir yere yerleştirin. Geçerli sorgu boş bir satır bulunana kadar kodu olarak kabul edilir.
- Zaman aralığı - bir zaman aralığı _son 24 saat_ varsayılan olarak ayarlanır. Farklı bir aralık kullanmak için Saat Seçici kullanın veya açık bir zaman Ekle sorgunuz için Aralık filtresi.


## <a name="understand-the-schema"></a>Şemayı anlama
Şema, görsel olarak mantıksal bir kategori altında gruplandırılmış bir tablo koleksiyonudur. Çeşitli kategorileri izleme çözümleri şunlardır. _LogManagement_ kategorisi, Windows ve Syslog olayları, performans verilerini ve istemci sinyal gibi sık kullanılan verileri içerir.

![Şema](media/get-started-portal/schema.png)

Her tabloda sütun adının yanındaki simge tarafından belirtildiği gibi farklı veri türleriyle sütunlardaki verileri düzenlenir. Örneğin, _olay_ ekran görüntüsünde gösterilen tablo içeren sütunlar gibi _bilgisayar_ metin olduğu _EventCategory_ bir sayı olan ve  _TimeGenerated_ olduğu tarih/saat.

## <a name="filter-the-results"></a>Sonuçları filtreleme
Her şeyi alınırken Start _olay_ tablo.

```Kusto
Event
```

Log Analytics, sonuçları otomatik olarak kapsamları:

- Zaman aralığı:  Varsayılan olarak, sorgular son 24 saat sınırlıdır.
- Sonuç sayısı: Sonuçları en fazla 10.000 kaydı için sınırlıdır.

Bu sorgu çok genel ve kullanışlı olması için çok fazla sonuç döndürür. Tablo öğeleri aracılığıyla ya da açıkça sorguya bir filtre eklemeden sonuçlarını filtreleyebilirsiniz. Sorgu filtre yeni filtre uygulanmış bir sonuç ayarlamak ve bu nedenle daha doğru sonuçlar üretebilir döndürürken Tablo öğeleri aracılığıyla sonuçları filtrelemek mevcut sonuç kümesi için geçerlidir.

### <a name="add-a-filter-to-the-query"></a>Sorguya bir filtre ekleyin
Her kaydın sol ok yoktur. Belirli bir kaydın ayrıntılarını açmak için bu oka tıklayın.

Bir sütun adı için yukarıda getirin "+" ve "-" görüntülenecek simge. Yalnızca aynı değere sahip kayıtları döndürecek bir filtre eklemek için "+" işaretine tıklayın. Tıklayın "-" Bu değere sahip kayıtları hariç tutacak ve ardından **çalıştırma** sorguyu yeniden çalıştırmayı.

![Sorgu Filtresi Ekle](media/get-started-portal/add-filter.png)

### <a name="filter-through-the-table-elements"></a>Tablo öğelerini filtreleyin
Artık bir önem derecesi ile olayları odaklanalım _hata_. Bu adlı bir sütun belirtilen _EventLevelName_. Bu sütun görmek için sağa kaydırmak gerekir.

Sütun başlığının yanında bulunan filtre simgesine tıklayın ve açılır pencerede'ı seçin, değerleri _ile başlayan_ metin _hata_:

![Filtre](media/get-started-portal/filter.png)


## <a name="sort-and-group-results"></a>Sırala ve Grupla sonuçları
Sonuçları yalnızca son 24 saat içinde oluşturulan SQL Server hata olaylarını içerecek şekilde daraltılmıştır. Ancak, sonuçları herhangi bir yolla sıralı değildir. Sonuçları gibi belirli bir sütuna göre sıralamak için _zaman damgası_ Örneğin, sütun başlığına tıklayın. Tek bir tıklamayla, ikinci tıklama azalan düzende sıralama sırasında artan düzende sıralar.

![Sıralama sütunu](media/get-started-portal/sort-column.png)

Sonuçları düzenlemek için başka bir grup tarafından yoludur. Sonuçları belirli bir sütuna yalnızca sütun üst bilgisinin diğer sütunları yukarıda Sürükle göre grup. Alt grupları oluşturmak için diğer sütunları de üst çubuğuna sürükleyin.

![Gruplar](media/get-started-portal/groups.png)

## <a name="select-columns-to-display"></a>Görüntülenecek sütunları seçin
Sonuçlar tablosu, genellikle çok sayıda sütun içerir. Geri dönen sütunlar bazıları varsayılan olarak görüntülenmez veya bazı görüntülenen sütunları kaldırmak isteyebileceğiniz bulabilirsiniz. Gösterilecek sütunları seçmek için sütunları düğmesine tıklayın:

![Sütun seçme](media/get-started-portal/select-columns.png)


## <a name="select-a-time-range"></a>Bir zaman aralığı seçin
Varsayılan olarak, Log Analytics uygular _son 24 saat_ zaman aralığı. Farklı bir aralık kullanmak için Saat Seçici başka bir değer seçin ve **çalıştırma**. Önceden oluşturulmuş değerlere ek olarak, kullandığınız _özel zaman aralığı_ sorgunuz için mutlak bir aralık seçmek için seçenek.

![Saat Seçici](media/get-started-portal/time-picker.png)

Özel bir zaman aralığı seçerken, seçilen değerleri, yerel saat diliminizi farklı olabilir, UTC biçimindedir.

Sorgu için bir filtre açıkça içeriyorsa _TimeGenerated_Seçici başlık gösterilir saati _sorguda Ayarla_. Bir çakışmayı önlemek üzere el ile seçim devre dışı bırakılır.


## <a name="charts"></a>Grafikler
Bir tablodaki sonuçları döndüren yanı sıra sorgu sonuçlarını visual biçimlerde sunulabilir. Örnek olarak aşağıdaki sorguyu kullanın:

```Kusto
Event 
| where EventLevelName == "Error" 
| where TimeGenerated > ago(1d) 
| summarize count() by Source 
```

Varsayılan olarak, bir tablodaki sonuçlar görüntülenir. Tıklayın _grafik_ sonuçları bir grafik görünümde görmek için:

![Çubuk grafik](media/get-started-portal/bar-chart.png)

Sonuçlar, yığılmış çubuk grafik olarak görüntülenir. Tıklayın _yığılmış sütun_ seçip _pasta_ sonuçları başka bir görünümünü göstermek için:

![Pasta grafiği](media/get-started-portal/pie-chart.png)

Görünümün x gibi farklı özellikleri ve y eksenleri veya gruplama ve bölme tercihleri değiştirilebilir el ile denetim çubuğundan.

İşleme işlecini kullanarak sorgunun kendisi, tercih ettiğiniz görünümü de ayarlayabilirsiniz.

### <a name="smart-diagnostics"></a>Akıllı tanılama
Bir zaman grafiğini üzerinde ani bir değişiklik veya verilerinizi adımda ise bir vurgulanan satırın noktasında görebilirsiniz. Gösterir _akıllı tanılama_ ani değişiklik filtre özelliklerinin belirlemiştir. Noktası üzerindeki filtre daha fazla ayrıntı alalım ve filtrelenmiş sürümü görmek için tıklayın. Bu değişikliğin nedenini belirlemenize yardımcı olabilir:

![Akıllı tanılama](media/get-started-portal/smart-diagnostics.png)

## <a name="pin-to-dashboard"></a>Panoya sabitle
Bir diyagram veya tabloya bir paylaşılan Azure panolarınızı sabitlemek için Raptiye simgesine tıklayın.

![Panoya sabitle](media/get-started-portal/pin-dashboard.png)

Panoya sabitleme belirli basitleştirme grafiğe uygulanır:

- Tablo sütunları ve satırları: Bir tabloyu panoya sabitlemek için dört veya daha az sütun olması gerekir. Yalnızca üst yedi satırlar görüntülenir.
- Zaman kısıtlaması: Sorgular için son 14 gün otomatik olarak sınırlıdır.
- Depo sayısı kısıtlaması: Çok sayıda ayrı bir depo olan grafik görüntülerseniz, daha az doldurulmuş depo tek bir otomatik olarak gruplandırılır _başkalarının_ depo.

## <a name="save-queries"></a>Sorguları Kaydet
Yararlı bir sorgu oluşturduktan sonra kaydedin veya başkalarıyla paylaşmak isteyebilirsiniz. **Kaydet** üst çubuğunda simgedir.

Tüm sorgu sayfası ya da tek bir sorgu işlevi olarak kaydedebilirsiniz. Diğer sorgular tarafından başvurulabilen sorguları işlevlerdir. Bir işlev olarak bir sorguyu kaydetmek için bu sorguyu diğer sorgular tarafından başvurulduğunda çağırmak için kullanılan ad bir işlev diğer adı sağlamanız gerekir.

![İşlev Kaydet](media/get-started-portal/save-function.png)

Log Analytics sorgu her zaman bir seçilen çalışma alanına kaydedilir ve bu çalışma alanının diğer kullanıcılarla paylaşılan.

## <a name="load-queries"></a>Sorguları
Sorgu Gezgini simgesine sağ üst alandır. Bu, tüm kaydedilmiş sorgular kategoriye göre listeler. Ayrıca, belirli sorguları hızla gelecekte bulmak için sık kullanılan olarak işaretlemek sağlar. Kaydedilmiş bir sorgu için geçerli pencere eklemek için çift tıklayın.

![Sorgu Gezgini](media/get-started-portal/query-explorer.png)

## <a name="export-and-share-as-link"></a>Dışarı aktarma ve bağlantı olarak paylaşın
Log Analytics'i birkaç verme yöntemleri destekler:

- Excel: Sonuçları CSV dosyası olarak kaydedin.
- Power BI: Sonuçları power BI dışarı aktarın. Bkz: [alma Azure İzleyici günlük verilerini Power bı'a](../../azure-monitor/platform/powerbi.md) Ayrıntılar için.
- Bir bağlantıyı Paylaş: Sorgu, ardından gönderilen ve aynı çalışma alanına erişimi olan diğer kullanıcılar tarafından yürütülen bir bağlantı olarak paylaşılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Azure İzleyici günlük sorguları yazma](get-started-queries.md).
