---
title: Azure Stream Analytics sorgu paralelleştirme ve ölçek kullanın
description: Bu makalede, giriş bölümlerini yapılandırma sorgu tanımı ayarlama ve iş akış birimleri ayarlama Stream Analytics işlerini ölçeklendirme açıklar.
services: stream-analytics
author: JSeb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: 44a7c0721d8a0683162d2219bff0e4a4ecb117e6
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="leverage-query-parallelization-in-azure-stream-analytics"></a>Azure Stream Analytics içinde sorgu paralelleştirme yararlanın
Bu makalede Azure akış analizi paralelleştirme yararlanmak nasıl gösterir. Giriş bölümlerini yapılandırma ve analizi sorgu tanımı ayarlama Stream Analytics işlerini ölçeklendirme öğrenin.
Bir önkoşul olarak açıklandığı akış birimi kavramı tanımanız isteyebilirsiniz [anlayın ve akış birimleri ayarlamak](stream-analytics-streaming-unit-consumption.md).

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Akış analizi işi bölümlerini nelerdir?
Stream Analytics iş tanımı girişleri, sorgu ve çıktıyı içerir. Burada iş veri akışından okuma girdileridir. Sorgu Veri Giriş akışı dönüştürmek için kullanılan ve burada iş iş sonuçları gönderir çıkış alınır.  

Bir işi veri akış için en az bir giriş kaynağı gerektirir. Veri akışı giriş kaynağı, bir Azure olay hub'ı veya Azure blob depolama depolanabilir. Daha fazla bilgi için bkz: [Azure Stream Analytics'e giriş](stream-analytics-introduction.md) ve [Azure Stream Analytics ile çalışmaya başlamak](stream-analytics-real-time-fraud-detection.md).

## <a name="partitions-in-sources-and-sinks"></a>Kaynakları ve havuzlarını bölümleri
Akış analizi işi ölçeklendirme giriş veya çıkış bölüm yararlanır. Bir bölüm anahtarına göre alt kümeleri veri bölmek sağlar bölümleme. (Örneğin, bir akış analizi işine) veri tüketen bir işlem kullanabilir ve verimliliğini artırır paralel olarak farklı bölümleri yazma. 

### <a name="inputs"></a>Girişler
Tüm Azure Stream Analytics giriş bölümleme yararlanabilirsiniz:
-   EventHub (açıkça PARTITION BY anahtar sözcüğüyle bölüm anahtarını ayarlamak için gereklidir)
-   IOT hub'ı (açıkça PARTITION BY anahtar sözcüğüyle bölüm anahtarını ayarlamak için gereklidir)
-   Blob depolama

### <a name="outputs"></a>Çıkışlar

Akış Analizi ile çalışırken, çıktılarında bölümleme yararlanabilirsiniz:
-   Azure Data Lake Storage
-   Azure İşlevleri
-   Azure Tablosu
-   Blob storage'ı (bölüm anahtarı açıkça ayarlayabilirsiniz)
-   CosmosDB (bölüm anahtarı açıkça ayarlamak için gereklidir)
-   EventHub (bölüm anahtarı açıkça ayarlamak için gereklidir)
-   IOT hub'ı (bölüm anahtarı açıkça ayarlamak için gereklidir)
-   Service Bus

Powerbı, SQL ve SQL veri ambarı çıkışları bölümleme desteklemez. Ancak, yine giriş bölümünde açıklandığı gibi bölüm [Bu bölümde](#multi-step-query-with-different-partition-by-values) 

Bölümleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Olay hub'ları özelliklere genel bakış](../event-hubs/event-hubs-features.md#partitions)
* [Veri bölümlendirme](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning#partitioning-azure-blob-storage)


## <a name="embarrassingly-parallel-jobs"></a>Utandırıcı derecede paralel işi
Bir *utandırıcı derecede paralel* iş Azure akış analizi sahibiz en ölçeklenebilir senaryo. Bu giriş sorgu bir örneği için bir bölüm için çıktının bir bölüm bağlanır. Bu paralellik aşağıdaki gereksinimlere sahiptir:

1. Sorgu mantığınızı aynı sorgu örneği tarafından işlenen aynı anahtara bağlıysa, olayları girişinizi aynı bölüme gidin emin olmanız gerekir. Olay hub'ları veya IOT hub'ı için bu olay verileri olmalıdır gelir **PartitionKey** değerinin ayarlanmış. Alternatif olarak, bölümlenmiş Gönderenler kullanabilirsiniz. BLOB Depolama için bu olayları aynı bölüm klasöre gönderilir anlamına gelir. Sorgu mantığınızı aynı sorgu örneği tarafından işlenmek üzere aynı anahtarı gerektirmiyorsa, bu gereksinim yoksayabilirsiniz. Bu mantık örneği basit bir select proje filtresi sorgu olacaktır.  

2. Veriler giriş tarafında düzenlendiğini sonra sorgunuzu bölümlenen emin olmanız gerekir. Bu kullanmanızı gerektirir **bölüm tarafından** tüm adımda. Birden çok adımı izin verilir, ancak bunların tümü aynı anahtar ile bölümlenmiş olması gerekir. Bölümleme anahtarı şu anda ayarlanmalıdır **PartitionID** tam olarak paralel olarak iş için sırayla.  

3. Bizim çıkış çoğunu bölümleme yararlanabilirsiniz, ancak, bir çıktı türü kullanırsanız, bölümleme desteklemiyor işinizi tam olarak paralel olmayacaktır. Başvurmak [çıkış bölüm](#outputs) daha fazla ayrıntı için.

4. Giriş bölüm sayısı çıktı bölüm sayısı eşit olmalıdır. BLOB Depolama çıkış bölümleri destekleyebilir ve Yukarı Akış sorgunun bölümleme düzeni devralır. Blob Depolama belirtilirse, veri için bir bölüm anahtarı giriş bölüm başına bölümlenmiş, böylece hala tam paralel sonucudur. Örnek bir tam olarak paralel iş izin bölüm değerler şunlardır:

   * 8 olay hub'ı giriş bölümleri ve 8 olay hub'ı bölümleri çıkış
   * 8 olay hub'ı giriş bölümleri ve blob depolama çıktı
   * 8 olay hub'ı giriş bölümleri ve rasgele önem düzeyi ile özel bir alan tarafından bölümlenmiş blob depolama çıktı
   * 8 blob depolama giriş bölümleri ve blob depolama çıkış
   * Depolama giriş bölümleri ve 8 olay hub'ı çıkış bölümleri 8 blob

Aşağıdaki bölümlerde utandırıcı derecede paralel bazı örnek senaryolar açıklanmaktadır.

### <a name="simple-query"></a>Basit sorgu

* Giriş: 8 bölümlerle olay hub'ı
* Çıktı: 8 bölümlerle olay hub'ı

Sorgu:

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Bu sorgu basit bir filtredir. Bu nedenle, biz event hub'ına gönderilen giriş bölümlendirme hakkında endişelenmeniz gerekmez. Sorgu içeren dikkat edin **bölüm tarafından PartitionID**, gereksinimden #2 önceki yerine getirir. Olay hub'ı çıkışı bölüm anahtarı olarak ayarlanmış olan iş yapılandırmak ihtiyacımız çıktı için **PartitionID**. Bir son onay giriş bölüm sayısı çıktı bölüm sayısına eşit olduğundan emin olmaktır.

### <a name="query-with-a-grouping-key"></a>Gruplandırma anahtarı ile sorgulama

* Giriş: 8 bölümlerle olay hub'ı
* Çıktı: Blob storage'ı

Sorgu:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Bu sorgu gruplandırma anahtarı yok. Bu nedenle, bir arada gruplandırılmış olayları aynı olay hub'ı bölümünde gönderilmelidir. Bu örnekte, biz TollBoothID tarafından grubunda olduğundan, şu olay Hub'ına olayları gönderildiğinde TollBoothID bölüm anahtarı olarak kullanılan emin olmanız gerekir. ASA kullanırız sonra **bölüm tarafından PartitionID** Bu bölüm düzeni devralır ve tam paralelleştirme etkinleştirmek için. Çıktı blob depolama olduğundan, biz gereksinim #4 göre bir bölüm anahtarı değerini yapılandırma hakkında endişelenmeniz gerekmez.

## <a name="example-of-scenarios-that-are-not-embarrassingly-parallel"></a>Senaryoların örneği *değil* utandırıcı derecede paralel

Önceki bölümde ki utandırıcı derecede paralel bazı senaryolar gösterilmiştir. Bu bölümde, utandırıcı derecede paralel olması için tüm gereksinimleri karşılayan olmayan senaryolar açıklanmaktadır. 

### <a name="mismatched-partition-count"></a>Uyuşmayan bölüm sayısı
* Giriş: 8 bölümlerle olay hub'ı
* Çıkış: Olay hub'ı 32 bölümlerle

Bu durumda, sorgu nedir önemli değildir. Giriş bölüm sayısı çıktı bölüm sayısı eşleşmiyorsa, topoloji utandırıcı derecede değil paralel. + ancak biz hala bazı düzeyi veya paralelleştirme elde edebilirsiniz.

### <a name="query-using-non-partitioned-output"></a>Çıktı bölümlenmemiş kullanarak sorgulama
* Giriş: 8 bölümlerle olay hub'ı
* Çıkış: Powerbı

Powerbı çıkış bölümleme şu anda desteklemiyor. Bu nedenle, bu senaryo utandırıcı derecede paralel değil.

### <a name="multi-step-query-with-different-partition-by-values"></a>Çok adımlı sorgu bölüm tarafından farklı değerlere sahip
* Giriş: 8 bölümlerle olay hub'ı
* Çıktı: 8 bölümlerle olay hub'ı

Sorgu:

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Gördüğünüz gibi ikinci adım kullanır **TollBoothId** bölümleme anahtarı olarak. Bu adım ilk adım olarak aynı değildir ve bu nedenle bir karışık yapacağımız gerektirir. 

Önceki örneklerde utandırıcı derecede paralel bir topoloji uygun (veya yok) bazı Stream Analytics işleri gösterir. Uygun değilse olası en büyük ölçek sahiptirler. Kılavuzu ölçeklendirme bu profillerinden birini uymayan işleri gelecekte kullanılabilecek yönelik güncelleştirmeler. Şimdilik, genel kılavuz aşağıdaki bölümleri kullanın.

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Akış birimleri bir işin en fazla Hesapla
Akış analizi işi tarafından kullanılan akış birimleri toplam sayısı, iş ve bölüm her adımı için tanımlanan sorgusu adım sayısını bağlıdır.

### <a name="steps-in-a-query"></a>Sorguda adımları
Sorguda bir veya daha fazla adım olabilir. Her bir adımdır tarafından tanımlanan alt sorgu **ile** anahtar sözcüğü. Dışında sorgu **ile** anahtar sözcüğü (yalnızca bir sorgu) de bir adım olarak, aşağıdaki gibi sayılan **seçin** aşağıdaki sorguyu deyiminde:

Sorgu:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Bu sorgu iki adımı vardır.

> [!NOTE]
> Bu sorgu, makalenin sonraki bölümlerinde daha ayrıntılı ele alınmıştır.
>  

### <a name="partition-a-step"></a>Bir adımı bölüm
Bölümleme bir adımı aşağıdaki koşulları gerektirir:

* Giriş kaynağının bölümlenmiş olması gerekir. 
* **Seçin** sorgu deyimi bölümlenmiş bir giriş kaynağından okuma gerekir.
* Sorgu adımı içinde olmalıdır **bölüm tarafından** anahtar sözcüğü.

Bir sorgu bölümlenmiş, işlenen ve toplu olarak ayrı bölüm grupları giriş olaylardır ve çıkışları olayları gruplarının her biri için oluşturulur. Birleştirilmiş bir toplama istiyorsanız, ikinci bir bölümlenmemiş adım oluşturmalısınız toplanacak.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Akış birimleri bir iş için en fazla Hesapla
Tüm bölümlenmemiş adımları birlikte en fazla altı akış birimleri (SUs) Stream Analytics işi için ölçeklendirebilirsiniz. Buna ek olarak, her bölüm için 6 SUs bölümlenmiş bir adımda ekleyebilirsiniz.
Bazı görebilirsiniz **örnekler** aşağıdaki tabloda.

| Sorgu                                               | İş için en çok SUs |
| --------------------------------------------------- | ------------------- |
| <ul><li>Sorgu, bir adım içeriyor.</li><li>Adım bölümlenmiş değil.</li></ul> | 6 |
| <ul><li>Giriş veri akışı 16 x bölümlenmiş.</li><li>Sorgu, bir adım içeriyor.</li><li>Adım bölümlenmiş.</li></ul> | 96 (6 * 16 bölümler) |
| <ul><li>Sorgu iki adımı içerir.</li><li>Adımları hiçbiri bölümlenmiş.</li></ul> | 6 |
| <ul><li>Giriş veri akışı 3 ile bölümlenmiş.</li><li>Sorgu iki adımı içerir. Giriş adım bölümlenmiş ve ikinci adım değildir.</li><li><strong>Seçin</strong> deyimi bölümlenmiş girişten okur.</li></ul> | 24 (18 bölümlenmiş adımlar için + bölümlenmemiş adımları için 6 |

### <a name="examples-of-scaling"></a>Ölçeklendirme örnekleri

Aşağıdaki sorgu üç tollbooths sahip Ücretli istasyonu giderek üç dakikalık penceresi içinde araba sayısı hesaplar. Bu sorgu, en fazla altı SUs genişletilebilir.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Sorgu için birden çok SUs kullanmak için giriş veri akışı ve sorgu bölümlenmiş olması gerekir. Veri akışı bölüm 3 olarak ayarlandığından, aşağıdaki değiştirilmiş sorgu en fazla 18 SUs Genişletilebilir:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Bir sorgu bölümlenmiş, giriş olaylarını işlenir ve ayrı bölüm grupları bir araya getirilir. Çıkış olayları da gruplarının her biri için oluşturulur. Bölümleme bazı beklenmeyen sonuçlara neden olabilir, **GROUP BY** alan giriş veri akışında bölüm anahtarı değil. Örneğin, **TollBoothId** önceki sorgu alanı bölüm anahtarı değil **Input1**. Gişe #1 verileri birden çok bölüm yayılabilir sonucudur.

Her biri **Input1** bölümleri işlenmeyecek ayrı olarak akış analizi tarafından. Sonuç olarak, birden fazla kayıt aynı dönen penceresinde aynı gişe araba sayısı oluşturulur. Giriş bölüm anahtarı değiştirilemez, bu sorun aşağıdaki örnekteki gibi bölümleri arasında toplama değerlerini bölüm dışı adım ekleyerek sabit:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Bu sorgu için 24 SUs genişletilebilir.

> [!NOTE]
> İki akışları birleştirilecekse akışlar birleşimler oluşturmak için kullandığınız sütunda bölüm anahtarı tarafından bölümlenir emin olun. Ayrıca her iki akış bölümler aynı sayıda olduğundan emin olun.
> 
> 





## <a name="get-help"></a>Yardım alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

