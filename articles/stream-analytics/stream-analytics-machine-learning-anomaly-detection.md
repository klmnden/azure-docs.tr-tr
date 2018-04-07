---
title: Azure Stream Analytics (Önizleme) anomali algılama
description: Bu makalede Azure akış analizi ve Azure Machine Learning birlikte anormallikleri algılamak için nasıl kullanılacağını açıklar.
services: stream-analytics
author: dubansal
ms.author: dubansal
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/12/2018
ms.openlocfilehash: cda5c26d4256720a8cf9af0e9abd604c979422a7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="anomaly-detection-in-azure-stream-analytics"></a>Azure Stream Analytics anomali algılama

> [!IMPORTANT]
> Bu işlevselliği Önizleme'de, üretim iş yükleri ile kullanarak öneri değil.

**AnomalyDetection** işleci anormallikleri farklı türler olay akış algılamak için kullanılır. Örneğin, boş bellek yavaş bir düşüş uzun bir süre içinde bir bellek sızıntısı göstergesi olabilir veya bir aralık içinde kararlı web hizmeti isteklerinin sayısını önemli ölçüde artırmak veya azaltmak.  

AnomalyDetection işleci anormallikleri üç tür algılar: 

* **Çift yönlü düzeyi değişiklik**: bir sürekli artırma veya azaltma değerlerin, yukarı ve aşağı düzeyi. Bu değer, ani ve anlık veya kısa süreli değişiklikler dıps farklıdır.  

* **Pozitif eğilimi yavaş**: eğilim yavaş bir artış Zamanla.  

* **Negatif eğilimi yavaş**: yavaş düşüş eğilimi Zamanla.  

AnomalyDetection işleci kullanırken belirtmelisiniz **Limit Duration** yan tümcesi. Bu yan tümce zaman aralığı (ne kadar geri geçerli olay geçmişinden) anormallikleri algılama göz önünde bulundurulması belirtir. Bu işleç isteğe bağlı olarak belirli bir özellik veya koşulu kullanarak eşleşen olaylar için sınırlı olabilir **zaman** yan tümcesi. Bu işleç olayları ayrı ayrı belirtilen anahtarı temel alan ve grupları ayrıca isteğe bağlı olarak işleyebilir **tarafından bölüm** yan tümcesi. Eğitim ve tahmin bağımsız olarak her bölüm için gerçekleşir. 

## <a name="syntax-for-anomalydetection-operator"></a>AnomalyDetection işleci için sözdizimi

`ANOMALYDETECTION(<scalar_expression>) OVER ([PARTITION BY <partition key>] LIMIT DURATION(<unit>, <length>) [WHEN boolean_expression])` 

**Örnek Kullanım**  

`SELECT id, val, ANOMALYDETECTION(val) OVER(PARTITION BY id LIMIT DURATION(hour, 1) WHEN id > 100) FROM input`

### <a name="arguments"></a>Bağımsız Değişkenler

* **scalar_expression** -anomali algılama gerçekleştirilen bir skaler ifade. Bu parametre Float eklemek veya bu dönüş tek bir (sayı) değerini Bigint veri türleri için izin verilen değerler. Joker karakter ifadesini **\*** verilmez. Skaler ifade diğer analitik işlevler veya dış işlevler içeremez. 

* **partition_by_clause** - `PARTITION BY <partition key>` yan tümcesi, ayrı bölümler öğrenme ve eğitim böler. Diğer bir deyişle, ayrı bir model değerini kullanılacak `<partition key>` ve bu değer yalnızca olaylarla öğrenme ve bu modelde eğitim için kullanılabilir. Örneğin, aşağıdaki sorgu trenler ve yalnızca aynı algılayıcı başka okumalar karşı okuma yapan:

  `SELECT sensorId, reading, ANOMALYDETECTION(reading) OVER(PARTITION BY sensorId LIMIT DURATION(hour, 1)) FROM input`

* **limit_duration yan tümcesi** `DURATION(<unit>, <length>)` -zaman aralığı (ne kadar geri geçerli olay geçmişinden) olarak kabul edilmelidir anormallikleri tespit edilirken belirtir. Bkz: [DATEDIFF](https://msdn.microsoft.com/azure/stream-analytics/reference/datediff-azure-stream-analytics) desteklenen birimleri ve bunların kısaltmalar ayrıntılı bir açıklaması için. 

* **when_clause** -anomali algılama hesaplamanın kabul olaylar için bir Boole koşulu belirtir.

### <a name="return-types"></a>Dönüş türleri

AnomalyDetection işleci, tüm üç puanları çıktı olarak içeren bir kayıt döndürür. Anomali algılayıcılar farklı türleriyle ilişkili özellikleri şunlardır:

- BiLevelChangeScore
- SlowPosTrendScore
- SlowNegTrendScore

Kaydı dışında değerlerini ayrı ayrı ayıklamak için kullanma **getrecordpropertyvalue'öğesinin** işlevi. Örneğin:

`SELECT id, val FROM input WHERE (GetRecordPropertyValue(ANOMALYDETECTION(val) OVER(LIMIT DURATION(hour, 1)), 'BiLevelChangeScore')) > 3.25` 

Anomali puanları birini bir eşik kestiği zaman anomali türü algılandı. Eşik herhangi bir kayan noktalı sayı olabilir > = 0. Eşik kolaylığını duyarlılık güvenirlik arasındaki ise. Örneğin, düşük bir Eşikte algılama daha önemli değişiklikler yapmak ve daha yüksek bir eşik algılama daha az hassas ve daha emin olun ancak bazı anormallikleri maske ancak daha fazla uyarılar oluşturur. Kullanılacak tam eşik değeri senaryoya bağlıdır. Üst sınır yoktur ancak önerilen aralık 3,25-5'tir. 

## <a name="anomaly-detection-algorithm"></a>Anomali algılama algoritması

* AnomalyDetection işlecini kullanan bir **Denetimsiz öğrenme** yaklaşım burada bunu değil varsayın olayları dağıtımlarında herhangi bir türde. Genel olarak, iki modeli burada bunlardan birini Puanlama için kullanılır ve diğer arka planda eğitildi verilen zaman, paralel olarak saklanır. Anomali algılama modelleri veri geçerli akıştan yerine bir bant dışı mekanizması kullanılarak eğitilmiş olup. Limit Duration parametre içinde kullanıcı tarafından belirtilen pencere boyutu d eğitim için kullanılan veri miktarına bağlıdır. Her model olayları 2B tutarında d dayanan eğitilmiş yukarı sona erer. En iyi sonuçlar için her penceresinde en az 50 olayları olması önerilir. 

* AnomalyDetection işlecini kullanan **kayan pencere semantiği** modelleri ve puanı olayları eğitmek için. Her olay için anomali değerlendirilir ve bir puan döndürülen anlamına gelir. Puanı bu anomali güven düzeyinin göstergesidir. 

* AnomalyDetection işleci sağlayan bir **Yinelenebilirlik garantisi** her zaman giriş aynı iş çıktısı bakılmaksızın aynı puan başlangıç zamanı üretir. Proje çıktı başlangıç zamanı ilk çıkış olayı iş tarafından üretilen süresini temsil eder. (İş çıktısı daha önce oluşturduysa) geçerli saati, özel bir değer veya son çıkış zamanı kullanıcı tarafından ayarlanır. 

### <a name="training-the-models"></a>Model eğitimi 

Zaman ilerledikçe, modelleri farklı veriler ile eğitildi. Puanları anlamlı olarak modeller eğitilmiş temel mekanizmasını anlamak için yardımcı olabilir. Burada, **t<sub>0</sub>**  olan **iş çıktısı başlangıç zamanı** ve **d** olan **pencere boyutu** Limit Duration'den parametre. Varsayın içine zaman bölünmüş **atlama boyutu d**, başlayıp `01/01/0001 00:00:00`. Aşağıdaki adımlar, olaylar puan ve modeli eğitmek için kullanılır:

* Bir iş başlatıldığında zaman t başlayan veri okuyan<sub>0</sub> – 2B.  
* Saati sonraki atlama ulaştığında yeni bir model M1 oluşturulur ve eğitilmiş başlatır.  
* Saat başka bir atlama ilerler, yeni bir model M2 oluşturulur ve eğitilmiş başlatır.  
* Zaman t ulaştığında<sub>0</sub>M1 etkin hale ve onun puan yüzdelik başlatır.  
* Sonraki atlamada aynı anda üç şey olur:  

  * M1 artık gerekli değildir ve atılır.  
  * Puanlama için kullanıldığı şekilde M2 yeterince eğitilmiş.  
  * Yeni bir model M3 oluşturulur ve arka planda eğitilmiş başlatır.  

* Bu döngü her atlama için tekrarlar etkin model atılır burada paralel modeline geçiş ve eğitim üçüncü modeli arka planda başlatın. 

Diagrammatically, adımları aşağıdaki gibi görünür: 

![Eğitim modelleri](media/stream-analytics-machine-learning-anomaly-detection/training_model.png)

|**modeli** | **Eğitim başlangıç zamanı** | **Kendi puan kullanmaya başlama zamanı** |
|---------|---------|---------|
|M1     | 11:20   | 11:33   |
|M2     | 11:30   | 11:40   |
|M3     | 11:40   | 11:50   |

* Model M1 okuma 11: 13'da iş başlatıldıktan sonra sonraki atlama olduğu 11:20:00, eğitim başlatır. İlk çıkış M1 ' 11:33 eğitim ile veri 13 dakika sonra da oluşturulur. 

* Yeni bir model M2 de eğitim 11: 30'da başlar ancak kendi puan 11:40 ile 10 dakika veri eğitilmiş sonra olan am kadar kullanılmayan. 

* M3 M2 olarak aynı düzeni izler. 

### <a name="scoring-with-the-models"></a>Modelleriyle Puanlama 

Machine Learning düzeyinde bir geçmiş penceresinde olaylarla karşılaştırarak anomali algılama algoritması gelen her olay için bir strangeness değer hesaplar. Strangeness hesaplama anomali her tür için farklıdır.  

Şimdi ayrıntılı strangeness hesaplama gözden geçirin (Geçmiş windows olayları ile bir dizi var. varsayılmıştır): 

1. **Çift yönlü düzey değişikliği:** Geçmiş penceresinde bağlı olarak, normal işletim aralığı [10 yüzdebirlik, 90] hesaplanan alt sınır ve 90'ıncı yüzdelik değer üst sınırı olarak olarak başka bir deyişle, 10 yüzdelik değer. Geçerli olay strangeness değeri olarak hesaplanır:  

   - event_value normal işletim aralıkta yoksa 0  
   - event_value/90th_percentile, eğer event_value > 90th_percentile  
   - 10th_percentile/event_value event_value ise, < 10th_percentile  

2. **Yavaş pozitif eğilimi:** bir eğilim çizgisi Geçmiş penceresinde olay değerler üzerinden hesaplanır ve biz satır pozitif bir eğilim arayın. Strangeness değeri olarak hesaplanır:  

   - Eğim pozitifse eğimi  
   - Aksi takdirde 0 

1. **Yavaş negatif eğilimi:** bir eğilim çizgisi Geçmiş penceresinde olay değerler üzerinden hesaplanır ve biz satır negatif eğilimi arayın. Strangeness değeri olarak hesaplanır: 

   - Eğim negatifse eğimi  
   - Aksi takdirde 0  

Gelen olay strangeness değeri hesaplanan sonra martingale değeri strangeness değere göre hesaplanır (bkz [Machine Learning blog](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/) martingale değerin nasıl hesaplanır ile ilgili ayrıntılar için). Bu martingale değeri anomali puan döndürülen. Martingale değer yavaş algılayıcısı durumlarıyla değişiklikler sağlam kalmasına izin verir ve yanlış uyarılar azaltan yanıtta garip değerlere artar. Ayrıca, kullanışlı bir özelliği vardır: 

Olasılık [t tür vardır, M<sub>t</sub> > λ] < 1/λ, burada M<sub>t</sub> anlık t martingale değerdir ve λ gerçek bir değerdir. Örneğin, biz ne zaman uyarı M<sub>t</sub>> 100 hatalı pozitif sonuç olasılığını olduğundan 1/100'den küçük.  

## <a name="guidance-for-using-the-bi-directional-level-change-detector"></a>Çift yönlü düzeyi kullanmaya yönelik kılavuz algılayıcısı değiştirme 

Çift yönlü düzey değişikliği algılayıcısı güç kesintisi ve kurtarma veya sağladığından saat trafiği, vb. gibi senaryolarda kullanılabilir. Ancak, bir model belirli verilerle eğitildi sonra yeni değer önceki üst sınırından daha yüksektir ve yalnızca, başka bir düzey değişikliği anormal şekilde çalışır (yukarı düzeyini değiştirmek çalışması) veya önceki alt sınırı (aşağı düzey değerinden daha düşük Durum değişikliği). Bu nedenle, bir model, eğitim penceresinde bunları anormal olarak kabul edilmesi veri değerleri yeni düzeyi (yukarı veya aşağı) aralığında görülmemelidir. 

Bu algılayıcısı kullanırken aşağıdaki noktaları bulundurulmalıdır: 

1. Zaman serisi aniden gördüğünde artışı veya değerinde bırakın, algıladığı AnomalyDetection işleci. Ancak dönüş normal algılama daha fazla planlama gerektirir. Bir zaman serisinin AnomalyDetection işlecin algılama aralığı en çok uzunluğunun yarısı anomali için ayarlanarak sağlanabilir anomali önce kararlı durumda olduğunda. Bu senaryo varsayar anomali en az süresi önceden tahmin edilebilir ve o zaman çerçevesinde yeterince modeli eğitmek için yeterli olay vardır (en az 50 olayları). 

   Bu Şekil 1 ve 2 (alt sınırı değişiklik aynı mantığı uygular) bir üst sınır değişikliği kullanarak aşağıda gösterilmektedir. Her iki rakamlar dalga biçimleri anormal bir düzey değişikliği ' dir. Dikey turuncu satırlar atlama sınırlar belirtmek ve atlama boyutu AnomalyDetection işlecinde belirtilen algılama aralığı ile aynıdır. Çizgiler eğitim pencere boyutunu belirtin. Şekil 1'de atlama boyutu hangi anormallik lasts süredir ile aynıdır. Şekil 2'de, anomali sürer sürenin yarısından atlama boyutudur. Puanlama için kullanılan model üzerinde normal veri eğitilmiş çünkü her durumda, Yukarı herhangi bir değişiklik algılandı. Ancak, çift yönlü düzey değişikliği algılayıcısı nasıl çalıştığını bağlı olarak, biz normal değerleri normal dönün puanlar modeli için kullanılan eğitim penceresinden hariç tutmanız gerekir. Şekil 1'de Puanlama modelinin eğitimi normal bazı olaylar içerir, böylece geri dönmek için normal algılanamıyor. Ancak Şekil 2'de eğitim yalnızca normal dönün algılandığında sağlar anormal bölümü içerir. Daha büyük bir şey biraz normal olaylar dahil olmak üzere yukarı sona erer ancak yarısı küçük bir şey de aynı nedenden dolayı çalışır. 

   ![Pencere boyutu eşit anomali uzunluğa sahip AD](media/stream-analytics-machine-learning-anomaly-detection/windowsize_equal_anomaly_length.png)

   ![AD pencere boyutu ile anomali uzunluğu yarısı eşittir](media/stream-analytics-machine-learning-anomaly-detection/windowsize_equal_half_anomaly_length.png)

2. Burada anomali uzunluğunu tahmin edilemez durumlarda bu algılayıcısı en iyi çaba çalışır. Ancak, eğitim verileri daha dar bir zaman penceresi sınırlar seçme, hangi dönüş normal algılama olasılığı artacaktır. 

3. Eğitim penceresi zaten bir anomali aynı yüksek değerin içeren olarak aşağıdaki senaryoda, uzun anomali algılanan değil. 

   ![Aynı boyutta ile daha fazla bilgi](media/stream-analytics-machine-learning-anomaly-detection/anomalies_with_same_length.png)

## <a name="example-query-to-detect-anomalies"></a>Örnek sorgu anormallikleri algılamak için 

Aşağıdaki sorgu, bir anomali algılanırsa bir uyarı çıktısını almak için kullanılabilir.
Giriş akışı Tekdüzen olmadığı durumlarda, toplama adım Tekdüzen zaman serisi dönüştürme yardımcı olabilir. Ortalama örnek kullanır ancak toplama türünü kullanıcı senaryoya bağlıdır. Ayrıca, bir zaman serisinin boşluklar toplama penceresinden daha büyük olduğunda, yok herhangi bir olayı (göredir penceresi semantiği kayan) tetikleyici anormallik algılama zaman serisinde. Sonuç olarak, sonraki olay geldiğinde bütünlüğünü varsayım bozulur. Böyle durumlarda, zaman serisinde boşlukları dolu olması gerekir. Olası bir yaklaşım son olay her atlama penceresinde, aşağıda gösterildiği gibi almaktır.

```sql
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
```

## <a name="performance-guidance"></a>Performans rehberi

* Altı akış birimleri işleri kullanın. 
* Olayları en az bir saniye birbirinden gönderin.
* AnomalyDetection işlecini kullanarak bölümlenmemiş bir sorgu, sonuçlar için ortalama hesaplama hakkında 25 ms gecikmeyle ortaya çıkabilir.
* Hesaplamalar sayısı daha yüksek olduğu gibi bölümlendirilmiş bir sorgu tarafından yaşadı gecikme biraz bölüm sayısı ile değişir. Ancak, gecikme süresini bölümlenmemiş durumda bir karşılaştırılabilir toplam olay sayısı için aynı tüm bölümler hakkındadır.
* Gerçek zamanlı veri okunurken büyük miktarda veri hızlı bir şekilde alınan neden. Bu veri işleme şu anda yavaştır. Bu tür senaryoların gecikme pencere boyutu veya olay aralığı yerine pencere veri noktası sayısı ile doğrusal olarak artırmak için bulunamadı. Gerçek zamanlı durumlarda gecikme süresini azaltmak için daha küçük bir pencere boyutu kullanmayı düşünün. Alternatif olarak, işinizi geçerli saatten başlayarak göz önünde bulundurun. Gecikme bölümlenmemiş sorguda bazı örnekler: 
    - Algılama aralığı 60 veri noktalarına 3 MB/sn verimde 10 saniye gecikmeyle sonuçlanabilir. 
    - 600 veri noktalarda gecikme süresi yaklaşık 80 saniye 0,4 MBps verimi ile ulaşabilirsiniz.

## <a name="limitations-of-the-anomalydetection-operator"></a>AnomalyDetection işleci sınırlamaları 

* Bu işleç depo ve DIP algılama şu anda desteklemiyor. Ani ve dıps spontaneous veya kısa süreli zaman serisinde değişir. 

* Bu işleç mevsimselliğin desenleri şu anda işlemez. Mevsimselliğin desenleri verileri, örneğin farklı web trafiği davranışını hafta sonları sırasında veya değil anormallikleri ancak beklenen bir davranış modelinde olan farklı alışveriş eğilimleri siyah Cuma sırasında yinelenen düzenleri alır. 

* Bu işleç Giriş zaman serisi tekdüzen olmasını bekler. Bir olay akışı dönen üzerinden toplama veya pencere atlamalı Tekdüzen yapılabilir. Olaylar arasındaki boşluk her zaman toplama penceresinden daha küçük olduğu senaryolarda atlayan pencere zaman serisi Tekdüzen yapmak yeterli olur. Boşluk daha büyük olabilir, boşlukları atlamalı pencerenin kullanarak son değeri tekrarlayarak doldurulabilir. 

## <a name="references"></a>Başvurular

* [Anomali algılama – kullanma makine anormal durumları zaman serisi verileri algılamak öğrenme](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/)
* [Machine learning API anomali algılama](https://docs.microsoft.com/en-gb/azure/machine-learning/machine-learning-apps-anomaly-detection-api)
* [Zaman serisi anomali algılama](https://msdn.microsoft.com/library/azure/mt775197.aspx)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

