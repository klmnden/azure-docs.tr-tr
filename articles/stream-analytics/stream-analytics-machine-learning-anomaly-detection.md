---
title: "Anomali algılama (Önizleme) Azure kullanım kılavuzda | Microsoft Docs"
description: "Akış analizi ve makine öğrenimi anormallikleri algılamak kullanın."
services: stream-analytics
documentationcenter: 
author: dubansal
manager: jhubbard
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: dubansal
ms.openlocfilehash: ff8571c6447f32ef9a435f5200803e76f6013ffa
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="using-the-anomalydetection-operator"></a>ANOMALYDETECTION işlecini kullanarak

> [!IMPORTANT]
> Bu işlev önizlemede değil. Üretim ortamında kullanılmasından önermiyoruz.

**ANOMALYDETECTION** işleci, olay akış anormallikleri algılamak için kullanılabilir.
Örneğin, boş bellek yavaş bir düşüş uzun bir süre içinde bir bellek sızıntısı göstergesi olabilir veya bir aralık içinde kararlı web hizmeti isteklerinin sayısını önemli ölçüde artırmak veya azaltmak.

Belirtilen süre içinde anormallikleri şu türler için denetler:

- Çift yönlü düzey değişikliği
- Yavaş pozitif eğilimi
- Yavaş negatif eğilimi

Ne kadar geçerli olaydan geçmişinde geri alınması gereken zaman aralığını kullanarak sınırlıdır **LIMIT DURATION** yan tümcesi. **ANOMALYDETECTION** isteğe bağlı olarak, belirli bir özellik eşleşmiyor veya kullanarak koşul olayları sınırlandırılabilir **zaman** yan tümcesi.

Olayları ayrı ayrı belirtilen anahtarı temel alan ve grupları ayrıca isteğe bağlı olarak işleyebilir **bölüm tarafından** yan tümcesi. Eğitim ve tahmin bağımsız olarak her bölüm gerçekleşir.

## <a name="syntax"></a>Sözdizimi

`ANOMALYDETECTION(<scalar_expression>) OVER ([PARTITION BY <partition key>] LIMIT DURATION(<unit>, <length>) [WHEN boolean_expression])` 


## <a name="example-usage"></a>Örnek Kullanım

`SELECT id, val, ANOMALYDETECTION(val) OVER(PARTITION BY id LIMIT DURATION(hour, 1) WHEN id > 100) FROM input`|


## <a name="arguments"></a>Bağımsız Değişkenler

- **scalar_expression**

  Anomali algılama gerçekleştirilen skaler ifade. Tek bir (sayı) değerini döndürür float veya bigint türü bir ifadedir. Joker karakter ifadesini  **\***  verilmez. **scalar_expression** diğer analitik işlevler veya dış işlevler içeremez.

- **([Partition_by_clause] limit_duration_clause [when_clause])**

- **partition_by_clause** 

  `PARTITION BY <partition key>` Yan tümcesi, ayrı bölümler öğrenme ve eğitim böler. Diğer bir deyişle, ayrı bir model değerini kullanılacak `<partition key>` ve bu değer yalnızca olaylarla öğrenme ve bu modelde eğitim için kullanılabilir. Örneğin,

  `SELECT sensorId, reading, ANOMALYDETECTION(reading) OVER(PARTITION BY sensorId LIMIT DURATION(hour, 1)) FROM input`

  eğitmek ve yalnızca aynı algılayıcı başka okumalar karşı okuma puan başlar.

- **limit_duration yan tümcesi** süresi (\<birim\>, \<uzunluğu\>)

  Geçerli olay geçmişinden ne kadarının kabul edilir belirtir **ANOMALYDETECTION** hesaplama. DATEDIFF desteklenen birimleri ve bunların kısaltmalar ayrıntılı bir açıklaması için bkz.

- **when_clause** 

  Olayları kabul için bir Boole koşulu belirtir **ANOMALYDETECTION** hesaplama.

## <a name="return-types"></a>Dönüş türleri

İşlevi tüm üç puanları çıktı olarak içeren bir kayıt döndürür. Anomali algılayıcılar farklı türleriyle ilişkili özellikler adlandırılır:

- BiLevelChangeScore
- SlowPosTrendScore
- SlowNegTrendScore

Kaydı dışında değerlerini ayrı ayrı ayıklamak için kullanma **getrecordpropertyvalue'öğesinin** işlevi. Örneğin:

`SELECT id, val FROM input WHERE (GetRecordPropertyValue(ANOMALYDETECTION(val) OVER(LIMIT DURATION(hour, 1)), 'BiLevelChangeScore')) > 3.25` 


Bu anomali puanları birini bir eşik kestiği zaman belirli bir türdeki bir anomali algılandı. Eşik herhangi kayan olabilir nokta sayısı \>= 0. Eşik kolaylığını duyarlılık güvenirlik arasındaki ise. Örneğin, düşük bir Eşikte algılama daha önemli değişiklikler yapmak ve daha yüksek bir eşik algılama daha az hassas ve daha emin olun ancak bazı anormallikleri maske ancak daha fazla uyarılar oluşturur. Kullanılacak tam eşik değeri senaryoya bağlıdır. Üst sınır yoktur ancak önerilen aralık 3,25-5.

## <a name="algorithm"></a>Algoritması

**ANOMALYDETECTION** , olay için hesaplama işlevi girer olay çalıştırır ve bir puan üretilen anlamına gelir penceresi semantiği kayan kullanır. Hesaplama dağıtım olay değerlerin değiştirilmesi durumunda denetleyerek çalışması Exchangeability Martingales temel alır. Bu durumda, olası anomali algılandı. Döndürülen puanı bu anomali güven düzeyinin göstergesidir. Dahili bir iyileştirme olarak **ANOMALYDETECTION** göre bir olay anomali puanı hesaplar *d* için *2B* , olayların nerede *d*belirtilen algılama pencere boyutu.

**ANOMALYDETECTION** Giriş zaman serisi tekdüzen olmasını bekler. Bir olay akışı dönen üzerinden toplama veya pencere atlamalı Tekdüzen yapılabilir. Olaylar arasındaki boşluk her zaman toplama penceresinden daha küçük olduğu senaryolarda atlayan pencere zaman serisi Tekdüzen yapmak yeterli olur. Boşluk daha büyük olabilir, boşlukları atlamalı pencerenin kullanarak son değeri tekrarlayarak doldurulabilir. Bu iki senaryoya izler örnek tarafından işlenebilir.

## <a name="performance-guidance"></a>Performans rehberi

- 6 SU işleri kullanın. 
- Olayları en az 1 saniye birbirinden gönderin.
- Kullanarak bir sorgu bölümlenmemiş **ANOMALYDETECTION** işlevi sonuçlar verebilir hakkında 25ms hesaplama gecikmeyle ortalama.
- Hesaplamalar sayısı daha yüksek olduğu gibi bölümlendirilmiş bir sorgu tarafından yaşadı gecikme biraz bölüm sayısı ile değişir. Ancak, gecikme süresini bölümlenmemiş durumda bir karşılaştırılabilir toplam olay sayısı için aynı tüm bölümler hakkındadır.
- Gerçek zamanlı veri okunurken büyük miktarda veri hızlı bir şekilde alınan neden. Bu veri işleme şu anda önemli ölçüde daha yavaştır. Bu tür senaryoların gecikme doğrusal olarak pencere boyutu veya olay aralığı yerine pencere veri noktası sayısı ile başına uzatılmasında artırmak için bulunamadı. Gerçek zamanlı durumlarda gecikme süresini azaltmak için daha küçük bir pencere boyutu kullanmayı düşünün. Alternatif olarak, işinizi geçerli saatten başlayarak göz önünde bulundurun. Gecikme bölümlenmemiş sorguda bazı örnekler: 
    - Algılama aralığı 60 veri noktalarına 3 MB/sn verimde 10 saniye gecikmeyle sonuçlanabilir. 
    - 600 veri noktalarda gecikme süresi yaklaşık 80 saniye 0,4 MBps verimi ile ulaşabilirsiniz.

## <a name="example"></a>Örnek

Aşağıdaki sorgu, bir anomali algılanırsa bir uyarı çıktısını almak için kullanılabilir.
Giriş akışı Tekdüzen olmadığı durumlarda, toplama adım Tekdüzen zaman serisi dönüştürme yardımcı olabilir. Örnek kullanır **AVG** ancak toplama türünü kullanıcı senaryoya bağlıdır. Ayrıca, bir zaman serisinin boşluklar toplama penceresinden daha büyük olduğunda, olacaktır olayı yok (göredir penceresi semantiği kayan) tetikleyici anormallik algılama zaman serisinde. Sonuç olarak, sonraki olay geldiğinde bütünlüğünü varsayım kırılır. Böyle durumlarda, zaman serisinde boşlukları doldurarak yolu gerekir. Olası bir yaklaşım son olay her atlama penceresinde, aşağıda gösterildiği gibi almaktır.

    WITH AggregationStep AS 
    (
         SELECT
               System.Timestamp as tumblingWindowEnd,

               AVG(value) as avgValue

         FROM input
         GROUP BY TumblingWindow(second, 5)
    ),

    FillInMissingValuesStep AS
    (
          SELECT
                System.Timestamp AS hoppingWindowEnd,

                TopOne() OVER (ORDER BY tumblingWindowEnd DESC) AS lastEvent

         FROM AggregationStep
         GROUP BY HOPPINGWINDOW(second, 300, 5)

    ),

    AnomalyDetectionStep AS
    (

          SELECT
                hoppingWindowEnd,
                lastEvent.tumblingWindowEnd as lastTumblingWindowEnd,
                lastEvent.avgValue as lastEventAvgValue,
                system.timestamp as anomalyDetectionStepTimestamp,

                ANOMALYDETECTION(lastEvent.avgValue) OVER (LIMIT DURATION(hour, 1)) as
                scores

          FROM FillInMissingValuesStep
    )

    SELECT
          alert = 1,
          hoppingWindowEnd,
          lastTumblingWindowEnd,
          lastEventAvgValue,
          anomalyDetectionStepTimestamp,
          scores

    INTO output

    FROM AnomalyDetectionStep

    WHERE

        CAST(GetRecordPropertyValue(scores, 'BiLevelChangeScore') as float) >= 3.25

        OR CAST(GetRecordPropertyValue(scores, 'SlowPosTrendScore') as float) >=
        3.25

       OR CAST(GetRecordPropertyValue(scores, 'SlowNegTrendScore') as float) >=
       3.25

## <a name="references"></a>Başvurular

* [Anomali algılama – kullanarak makine anormal durumları zaman serisi verileri algılamak öğrenme](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/)
* [Anomali algılama API makine](https://docs.microsoft.com/en-gb/azure/machine-learning/machine-learning-apps-anomaly-detection-api)
* [Zaman serisi Anomali algılama](https://msdn.microsoft.com/library/azure/mt775197.aspx)

## <a name="get-support"></a>Destek alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

