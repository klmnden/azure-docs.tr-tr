---
title: Azure Stream Analytics'te sorgu paralelleştirme ve ölçek kullanın
description: Bu makalede, giriş bölümlerini yapılandırma, Sorgu tanımını ayarlayarak ve akış birimleri iş ayarlama Stream Analytics işlerini ölçeklendirme açıklar.
services: stream-analytics
author: JSeb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: 5eba5601a50640261fa1b488d959f606d4514737
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612215"
---
# <a name="leverage-query-parallelization-in-azure-stream-analytics"></a>Azure Stream analytics'te sorgu paralelleştirmesinden
Bu makalede, Azure Stream Analytics'te paralelleştirme yararlanmak işlemini göstermektedir. Giriş bölümlerini yapılandırma ve analytics Sorgu tanımını ayarlayarak Stream Analytics işlerini ölçeklendirmeyi öğrenin.
Bir önkoşul olarak açıklandığı akış birimi kavramı hakkında bilgi sahibi olmasını isteyebilirsiniz [anlayın ve akış birimi Ayarla](stream-analytics-streaming-unit-consumption.md).

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Bir Stream Analytics işi bölümlerini nelerdir?
Stream Analytics iş tanımı girişleri, sorgu ve çıkış içerir. Burada işi veri akışından okur girişlerdir. Veri giriş akışını dönüştürmek için kullanılan sorgu ve iş için iş sonuçlarını göndereceği yeri çıkış alınır.

Bir iş akış verileri için en az bir giriş kaynağı gerektirir. Veri akışı giriş kaynağı, bir Azure olay hub'ı veya Azure blob depolama alanında depolanabilir. Daha fazla bilgi için [Azure Stream analytics'e giriş](stream-analytics-introduction.md) ve [Azure Stream Analytics'i kullanmaya başlama](stream-analytics-real-time-fraud-detection.md).

## <a name="partitions-in-sources-and-sinks"></a>Kaynaklar ve havuzlar bölümler
Bir Stream Analytics işi ölçeklendirme, giriş veya çıkış bölümlerinde avantajlarından yararlanır. Verileri bölüm anahtarına göre alt kümelerini böler sağlar bölümleme. Verileri (örneğin, bir akış analizi işi) kullanan bir işlem kullanabilir ve aktarım hızını artıran paralel olarak, farklı bölümler yazma. 

### <a name="inputs"></a>Girişler
Tüm Azure Stream Analytics giriş bölümleme yararlanabilirsiniz:
-   EventHub (bölüm anahtarı PARTITION BY anahtar sözcüğü ile açıkça ayarlamak için gereklidir)
-   IOT hub'ı (bölüm anahtarı PARTITION BY anahtar sözcüğü ile açıkça ayarlamak için gereklidir)
-   Blob depolama

### <a name="outputs"></a>Çıkışlar

Stream Analytics ile çalışırken, yapılandırma çıkışları bölümleme yararlanabilirsiniz:
-   Azure Data Lake Storage
-   Azure İşlevleri
-   Azure Tablosu
-   Blob depolama (bölüm anahtarı, açıkça ayarlayabilirsiniz)
-   Cosmos DB (bölüm anahtarı açıkça ayarlamak için gereklidir)
-   Event hubs'ı (bölüm anahtarı açıkça ayarlamak için gereklidir)
-   IOT hub'ı (bölüm anahtarı açıkça ayarlamak için gereklidir)
-   Service Bus
- SQL ve isteğe bağlı bölümleme ile SQL veri ambarı: hakkında daha fazla bilgi [çıktı Azure SQL veritabanı sayfasına](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-sql-output-perf).

Power BI, bölümleme desteklemiyor. Bununla birlikte, yine de giriş bölümünde anlatıldığı gibi bölümleyebilirsiniz [bu bölümü](#multi-step-query-with-different-partition-by-values) 

Bölümleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Event Hubs özelliklerine genel bakış](../event-hubs/event-hubs-features.md#partitions)
* [Veri bölümleme](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning)


## <a name="embarrassingly-parallel-jobs"></a>Utandırıcı derecede paralel işleri
Bir *utandırıcı derecede paralel* işi, Azure Stream Analytics'te sahip olduğumuz en ölçeklenebilir senaryo. Bu çıkış bir bölüm için bir bölüm bir örnek sorgu girişi bağlanır. Bu paralellik aşağıdaki gereksinimlere sahiptir:

1. Sorgu mantığınızı aynı sorgu örneği tarafından işlenmekte olan aynı anahtara bağlı olduğu durumlarda olayları girişlerinizin aynı bölüme gideceği emin olmanız gerekir. Event Hubs veya IOT hub'ı için bu olay verileri olması gerektiğini anlamına gelir **PartitionKey** değer kümesi. Alternatif olarak, bölümlenmiş Gönderenler kullanabilirsiniz. BLOB Depolama için bu olaylar aynı bölüm klasörüne gönderilir anlamına gelir. Sorgu mantığınızı aynı sorgu örneği tarafından işlenmek üzere aynı anahtar gerektirmiyorsa, bu gereksinim yoksayabilirsiniz. Bu mantık örneği basit bir select proje filtresi sorgusu olacaktır.  

2. Veri giriş tarafında düzenlendiğini sonra sorgunuzu bölümlenen emin olmanız gerekir. Bu kullanmanızı gerekli hale getirmiş **PARTITION BY** tüm adımlarda. Birden çok adım izin verilir, ancak bunların tümü aynı anahtarla bölümlenmesi gerekir. Uyumluluk düzeyi altında 1.0 ve 1.1, bölümleme anahtarı ayarlamak **PartitionID** sırada işin tam olarak paralel olması. İşleri compatility düzeyiyle 1.2 ve üzeri için özel bir sütun bölüm anahtarı olarak giriş ayarlarında belirtilebilir ve iş paralellized automoatically PARTITION BY yan tümcesi olmadan dahi olacaktır.

3. Bizim çıkış çoğunu bölümleme avantajından yararlanmak, bir çıkış türü kullanıyorsanız, bölümleme desteklemez ancak işinizi tam olarak paralel olmayacaktır. Başvurmak [çıkış bölümü](#outputs) daha fazla ayrıntı için.

4. Giriş bölüm sayısı, çıkış bölüm sayısına eşit olmalıdır. BLOB Depolama çıkışı bölümler destekleyebilir ve Yukarı Akış sorgunun bölümleme düzeni devralır. Blob Depolama belirtilmişse, veri için bölüm anahtarı giriş bölümünü bölümlenmiş, bu nedenle yine de tam olarak paralel sonucudur. Tam olarak paralel bir iş çalıştırılmasına izin bölüm değerlerini örnekleri aşağıda verilmiştir:

   * Bölüm 8 olay hub'ı giriş bölümleri ve 8 olay hub'ı çıkışı
   * 8 olay hub'ı giriş bölümleri ve blob depolama çıkışı
   * 8 olay hub'ı giriş bölümleri ve özel bir alana göre rastgele kardinaliteyle bölümlenmiş blob depolama çıkışı
   * 8 blob depolama giriş bölüm ve blob depolama çıkışı
   * Depolama giriş bölümleri ve 8 olay hub'ı çıkış bölüm 8 blob

Aşağıdaki bölümlerde utandırıcı derecede paralel bazı örnek senaryolar açıklanmaktadır.

### <a name="simple-query"></a>Basit sorgu

* Giriş: Olay hub'ı 8 bölüm ile
* Çıktı: Olay hub'ı 8 bölüm ile

Sorgu:

```SQL
    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100
```

Bu sorguyu basit bir filtredir. Bu nedenle, olay hub'ına gönderilen giriş bölümlendirme hakkında endişe etmeniz gerekmez. 1\.2 içermelidir önce uyumluluk düzeyi ile işleri dikkat edin **PARTITION BY PartitionID** gereksinim #2'öğesinden daha önce karşıladığı şekilde yan tümcesi. Bölüm anahtarı olarak ayarlanmış olan işin olay hub'ı çıkışı yapılandırmak ihtiyacımız çıkış için **PartitionID**. Bir son onay giriş bölüm sayısı çıkış bölüm sayısına eşit olduğundan emin olmaktır.

### <a name="query-with-a-grouping-key"></a>Bir gruplandırma anahtar ile sorgulama

* Giriş: Olay hub'ı 8 bölüm ile
* Çıktı: Blob depolama

Sorgu:

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Bu sorgu, bir gruplandırma anahtarına sahiptir. Bu nedenle, gruplandırılmış olayları olay hub'ı aynı bölüme gönderilmesi gerekir. Bu örnekte biz tarafından TollBoothID grubunda olduğundan, biz olayları olay Hub'ına gönderildiğinde TollBoothID bölüm anahtarı olarak kullanıldığından emin olmalıdır. ASA kullanabiliriz sonra **PARTITION BY PartitionID** Bu bölüm düzeni devralır ve tam paralelleştirme etkinleştirin. Çıktı blob depolama olduğundan, gereksinim #4 ilişkin bir bölüm anahtarı değerini yapılandırma hakkında endişe etmeniz gerekmez.

## <a name="example-of-scenarios-that-are-not-embarrassingly-parallel"></a>Senaryoların örneği *değil* utandırıcı derecede paralel

Önceki bölümde, biz utandırıcı derecede paralel bazı senaryolar gösterilmiştir. Bu bölümde, utandırıcı derecede paralel olarak tüm gereksinimlerini senaryoları ele alır. 

### <a name="mismatched-partition-count"></a>Uyuşmayan bölüm sayısı
* Giriş: Olay hub'ı 8 bölüm ile
* Çıktı: Olay hub'ı ile 32 bölümlü

Bu durumda, sorgu nedir önemi yoktur. Giriş bölüm sayısı olan çıkış bölüm sayısı eşleşmezse, topoloji utandırıcı derecede değil paralel. + ancak biz yine de bazı düzeyi veya paralelleştirme elde edebilirsiniz.

### <a name="query-using-non-partitioned-output"></a>Bölümlenmemiş çıkış kullanarak sorgulama
* Giriş: Olay hub'ı 8 bölüm ile
* Çıktı: Power BI

Power BI çıkışı bölümleme şu anda desteklemiyor. Bu nedenle, bu senaryo utandırıcı derecede paralel değil.

### <a name="multi-step-query-with-different-partition-by-values"></a>Çok adımlı sorgunun PARTITION BY farklı değerlerle
* Giriş: Olay hub'ı 8 bölüm ile
* Çıktı: Olay hub'ı 8 bölüm ile

Sorgu:

```SQL
    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

Gördüğünüz gibi ikinci adım kullanır **TollBoothId** bölümleme anahtarı olarak. Bu adım ilk adım ile aynı değildir ve bu nedenle bize bir karışık yapmanız gerekir. 

Önceki örneklerde utandırıcı derecede paralel bir topoloji uygun (veya yok) bazı Stream Analytics işleri gösterir. Uygun, en yüksek ölçek olası sahiptirler. Bir kılavuz ölçeklendirme, bu profillerin uygun olmayan işler gelecekte kullanıma sunulacak yönelik güncelleştirmeler. Şu an için genel kılavuz aşağıdaki bölümlerde kullanın.

### <a name="compatibility-level-12---multi-step-query-with-different-partition-by-values"></a>1\.2 - çok adımlı sorgunun PARTITION BY farklı değerlerle uyumluluk düzeyi 
* Giriş: Olay hub'ı 8 bölüm ile
* Çıktı: Olay hub'ı 8 bölüm ile

Sorgu:

```SQL
    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

Uyumluluk düzeyi 1.2, paralel sorgu yürütme varsayılan olarak etkinleştirir. Örneğin, önceki bölümde sorgu "TollBoothId" sütunu giriş bölüm anahtarı olarak ayarlı sürece parttioned olacaktır. BÖLÜM tarafından ParttionId yan tümcesi gerekli değildir.

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Akış birimleri, bir işin en büyük hesaplayın
Stream Analytics işi tarafından kullanılan akış birimlerinin toplam sayısı, iş ve her adım için bölüm sayısı için tanımlanan sorgu, adım sayısı bağlıdır.

### <a name="steps-in-a-query"></a>Sorgu adımları
Bir sorgu, bir veya daha fazla adım olabilir. Alt sorgu tarafından tanımlanan her adım, **ile** anahtar sözcüğü. Dışında sorgu **ile** anahtar sözcüğü (yalnızca bir sorgu) de bir adım olarak, aşağıdakiler gibi sayılır **seçin** aşağıdaki sorgu deyimi:

Sorgu:

```SQL
    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId
```

Bu sorgu, iki adımı vardır.

> [!NOTE]
> Bu sorgu, makalenin ilerleyen bölümlerinde daha ayrıntılı olarak ele alınmıştır.
>  

### <a name="partition-a-step"></a>Bir adım bölümleme
Bir adım bölümleme aşağıdaki koşulları gerektirir:

* Giriş kaynağı bölümlenmiş olması gerekir. 
* **Seçin** sorgu deyimi bölümlenmiş bir giriş kaynağından okuma gerekir.
* Sorgu adımı içinde olmalıdır **PARTITION BY** anahtar sözcüğü.

Bir sorgu bölümlenmiş, işlenen ve toplu olarak ayrı bölüm grupları giriş olaylardır ve olayları çıkışlar grupların her biri için oluşturulur. Birleşik toplama istiyorsanız, bölümlenmemiş bir ikinci adım oluşturmalısınız toplanacak.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Akış birimleri, bir iş için en fazla Hesapla
Tüm adımları bölümlenmemiş en fazla altı akış birimleri (su) için bir Stream Analytics işi birlikte ölçeklendirebilirsiniz. Buna ek olarak, bölümlenmiş bir adımda her bölüm 6 SUs ekleyebilirsiniz.
Bazı gördüğünüz **örnekler** aşağıdaki tabloda.

| Sorgu                                               | İş için en çok SUs |
| --------------------------------------------------- | ------------------- |
| <ul><li>Sorgu bir adım içerir.</li><li>Adım bölümlenmiş değil.</li></ul> | 6 |
| <ul><li>Giriş veri akışını 16 göre bölümlenir.</li><li>Sorgu bir adım içerir.</li><li>Adım bölümlenir.</li></ul> | 96 (6 * 16 bölümler) |
| <ul><li>Sorgu iki adımı içeriyor.</li><li>Adımları hiçbiri bölümlenir.</li></ul> | 6 |
| <ul><li>Giriş veri akışını 3 göre bölümlenir.</li><li>Sorgu iki adımı içeriyor. Giriş adım bölümlenmiş ve ikinci adım değil.</li><li><strong>Seçin</strong> deyimi bölümlenmiş girdiden okur.</li></ul> | 24 (bölümlenmiş adımları 18 + bölümlenmemiş adımları için 6 |

### <a name="examples-of-scaling"></a>Ölçeklendirme örnekleri

Aşağıdaki sorgu, üç tollbooths sahip Ücretli istasyonu giderek bir üç dakikalık penceresi içinde otomobiller sayısını hesaplar. Bu sorgu, altı SUs kadar ölçeklendirilebilir.

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Sorgu için daha fazla SUs kullanmak için giriş veri akışını hem de sorgu bölümlenmiş olması gerekir. Veri akışı bölüm 3 olarak ayarlandığından, aşağıdaki değiştirilen sorguyu en fazla 18 SUs ölçeklendirilebilir:

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Bir sorgu bölümlenmiş, giriş olayları işlenir ve ayrı bölüm grupları içinde toplanır. Çıkış olayları da grupların her biri için oluşturulur. Bölümleme bazı beklenmeyen sonuçlara neden durumlarda **GROUP BY** alan bir giriş veri akışını bölüm anahtarı değil. Örneğin, **TollBoothId** önceki sorguyu alanı bölüm anahtarı değil **Input1**. Birden çok bölüm gişe #1 verilerden yayılabilen sonucudur.

Her biri **Input1** bölümleri işlenmeyecek ayrı olarak Stream Analytics tarafından. Sonuç olarak, araba sayısı aynı atlayan pencere içinde aynı gişe için birden çok kayıt oluşturulur. Giriş bölüm anahtarı değiştirilemez, aşağıdaki örnekte olduğu gibi bölümler arasında toplama değerleri için bir bölüm olmayan adım ekleyerek bu sorun düzeltilebilir:

```SQL
    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

Bu sorgu için 24 SUs ölçeklendirilebilir.

> [!NOTE]
> İki birleştirilecekse, akışları birleştirmeler oluşturmak için kullandığınız bir sütun bölüm anahtarı bölümlenir emin olun. Ayrıca her iki akış bölümleri aynı sayıda sahip olduğunuzdan emin olun.
> 
> 

## <a name="achieving-higher-throughputs-at-scale"></a>Uygun ölçekte, yüksek aktarım hızı elde edin

Bir [utandırıcı derecede paralel](#embarrassingly-parallel-jobs) iş gerekli ancak uygun ölçekte daha yüksek bir aktarım hızı sürdürmek yeterli değil. Her depolama sistemi ve ilgili Stream Analytics çıktısını olası yazma en iyi performans sağlamak nasıl farklılıklara sahiptir. Gibi doğru yapılandırmaları kullanarak çözülebilir bazı zorluklar herhangi ölçekli senaryosuyla vardır. Bu bölümde, birkaç ortak çıkış yapılandırmaları açıklar ve saniye başına 1 K, 5 K ve 10 bin olay alımı ücretlerine dayanıklılık geçtikten için örnekleri sağlar.

Aşağıdaki gözlemlere, durum bilgisi olmayan (doğrudan geçiş) sorgusu, temel bir olay hub'ı, Azure SQL DB veya Cosmos DB yazan bir JavaScript UDF ile bir Stream Analytics işi kullanın.

#### <a name="event-hub"></a>Olay Hub'ı

|Alma oranı (olay / saniye) | Akış Birimleri | Çıkış kaynakları  |
|--------|---------|---------|
| 1K     |    1\.    |  2 İŞLEME BİRİMİ   |
| 5K     |    6    |  6 İŞLEME BİRİMİ   |
| 10.000    |    12   |  10 İŞLEME BİRİMİ  |

[Olay hub'ı](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-eventhubs) birimleri (SU) ve aktarım hızı, en verimli hale getirir ve etkili bir yolla çözümlemek ve Stream Analytics dışında veri akışı için akış açısından çözüm doğrusal olarak ölçeklendirir. İşleri, kabaca 200 MB/sn veya 19 trilyon olayları günde en fazla işleme için çeviren 192 SU kadar ölçeklendirilebilir.

#### <a name="azure-sql"></a>Azure SQL
|Alma oranı (olay / saniye) | Akış Birimleri | Çıkış kaynakları  |
|---------|------|-------|
|    1K   |   3  |  S3   |
|    5K   |   18 |  P4   |
|    10.000  |   36 |  P6   |

[Azure SQL](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-azuresql) paralel olarak yazmayı destekler, bölümleme çağrılan devralır, ancak değil varsayılan olarak etkindir. Ancak, tam olarak paralel sorgu yanı sıra, etkinleştirme devral bölümleme, daha yüksek aktarım hızı elde etmek yeterli olmayabilir. SQL yazma aktarım hızı, SQL Azure veritabanı yapılandırması ve tablo şemanızı üzerinde önemli ölçüde bağlıdır. [SQL çıkış performansı](./stream-analytics-sql-output-perf.md) makale yazma aktarım hızınızı en üst düzeye çıkarabilirsiniz parametreleri hakkında ayrıntılı bilgilere sahiptir. Belirtilen [Azure SQL veritabanı için Azure Stream Analytics çıkış](./stream-analytics-sql-output-perf.md#azure-stream-analytics) makalesi, bu çözüm 8 bölüm ötesinde tam olarak paralel bir işlem hattı olarak doğrusal olarak ölçeklendirilemez ve SQL çıktı önce yeniden bölümlenmesi gerekebilir (bkz [ İÇİNE](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count)). Premium SKU'ları ek günlük yedeklerinin her birkaç'olmuyor yükünden yanı sıra yüksek g/ç oranları sürdürmek için gereken dakika.

#### <a name="cosmos-db"></a>Cosmos DB
|Alma oranı (olay / saniye) | Akış Birimleri | Çıkış kaynakları  |
|-------|-------|---------|
|  1K   |  3    | 20K RU  |
|  5K   |  24   | 60K RU  |
|  10.000  |  48   | 120K RU |

[Cosmos DB](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb) Stream Analytics'ten alınan çıkış altında yerel tümleştirmesini kullanacak şekilde güncelleştirildi [uyumluluk düzeyi 1.2](./stream-analytics-documentdb-output.md#improved-throughput-with-compatibility-level-12). Uyumluluk düzeyi 1.2, önemli ölçüde daha yüksek aktarım hızı sağlar ve yeni projeler için varsayılan uyumluluk düzeyi 1.1 ile karşılaştırıldığında RU kullanımını azaltır. Çözüm üzerinde /deviceId bölümlenmiş CosmosDB kapsayıcıları kullanır ve çözümün geri kalanı aynı şekilde yapılandırılır.

Tüm [ölçek azure örnekleri akış](https://github.com/Azure-Samples/streaming-at-scale) iletilir ve test istemcilerinin giriş olarak benzetimi yük tarafından bir olay Hub'ı kullanın. Her giriş olayı işleme hızları (1 MB/sn, 5 MB/s ve 10 MB/sn) için yapılandırılmış alımı ücretlerine bir kolayca çeviren bir 1 KB JSON belgesidir. Olaylar, en fazla 1 K cihazlar için (kısaltılmış içinde) aşağıdaki JSON verilerini göndermeden bir IOT cihazının simülasyonunu gerçekleştirme:

```
{
    "eventId": "b81d241f-5187-40b0-ab2a-940faf9757c0",
    "complexData": {
        "moreData0": 51.3068118685458,
        "moreData22": 45.34076957651598
    },
    "value": 49.02278128887753,
    "deviceId": "contoso://device-id-1554",
    "type": "CO2",
    "createdAt": "2019-05-16T17:16:40.000003Z"
}
```

> [!NOTE]
> Yapılandırmalar çözümde kullanılan çeşitli bileşenler nedeniyle değiştirilebilir. Daha doğru bir tahmin için örnekleri kendi senaryonuza uyacak şekilde özelleştirin.

### <a name="identifying-bottlenecks"></a>Performans sorunlarını tanımlama

Ölçümleri bölmesinde Azure Stream Analytics işinizi işlem hattınızı performans sorunlarını tanımlamak için kullanın. Gözden geçirme **giriş/çıkış olaylarını** için aktarım hızı ve ["Eşik gecikmesi"](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/) veya **biriktirme listesine alınan olaylar** iş giriş oranı ile tutmaktan olmadığını görmek için. Olay hub'ı ölçümler için aranacak **istekler daraltıldı** ve eşik birimleri uygun şekilde ayarlayın. Cosmos DB ölçümleri için gözden geçirin **bölüm anahtar aralığı başına tüketilen RU/sn en fazla** , bölüm anahtar aralığı emin olmak için aktarım hızı altında aynı şekilde kullanılır. Azure SQL DB için izleme **günlük GÇ** ve **CPU**.

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: https://support.microsoft.com
[azure.event.hubs.developer.guide]: https://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301

