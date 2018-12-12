---
title: Azure Stream Analytics (Önizleme) anomali algılama
description: Bu makalede, Azure Stream Analytics ve Azure Machine Learning birlikte anomalileri algılamak için nasıl kullanılacağını açıklar.
services: stream-analytics
author: dubansal
ms.author: dubansal
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: df1010be8c9f41684af806885db7587bfcf1c540
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53091229"
---
# <a name="anomaly-detection-in-azure-stream-analytics"></a>Azure Stream analytics'te anomali algılama

> [!IMPORTANT]
> Bu işlev kullanım dışı aşamasında olan, ancak yeni işlevleri ile değiştirilecek. Daha fazla bilgi için ziyaret [Azure Stream analytics'te sekiz yeni özellikler](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/) blog gönderisi.

**Anomalydetectıon** işleci, olay akışları farklı türlerde anomalileri algılamak için kullanılır. Örneğin, yavaş bir düşüş boş bellek uzun bir zaman içinde bir bellek sızıntısının olabilir veya bir aralıktaki kararlı web hizmeti isteklerinin sayısını önemli ölçüde artırmak veya azaltmak.  

Anomalydetectıon işleci, üç tür anomalileri algılar: 

* **İki yönlü düzeyi değişiklik**: bir sürekli artırma veya azaltma değerleri, yukarı ve aşağı doğru düzeyde yer. Bu değer, ani ve değişiklikleri anlıktır ve kısa süreli olan dıps farklıdır.  

* **Pozitif eğilim yavaş**: zaman içindeki eğilimi yavaş bir artışı.  

* **Negatif eğilim yavaş**: yavaş bir düşüş eğilimi, zaman içinde.  

Anomalydetectıon işleci kullanırken belirtmelisiniz **Limit Duration** yan tümcesi. Bu yan tümce zaman aralığını (ne kadar geri geçerli olay geçmişinden), anomalileri tespit edilirken dikkate alınacağını belirtir. Bu işleç isteğe bağlı olarak belirli bir özellik ya da koşul kullanarak eşleşen olaylar için sınırlı olabilir **olduğunda** yan tümcesi. Bu işleç, ayrı olarak belirtilen anahtara göre olay gruplarını ayrıca isteğe bağlı olarak işleyebilir **tarafından bölüm** yan tümcesi. Eğitim ve tahmin bağımsız olarak her bölüm için gerçekleşir. 

## <a name="syntax-for-anomalydetection-operator"></a>Anomalydetectıon işleci için söz dizimi

`ANOMALYDETECTION(<scalar_expression>) OVER ([PARTITION BY <partition key>] LIMIT DURATION(<unit>, <length>) [WHEN boolean_expression])` 

**Örnek Kullanım**  

`SELECT id, val, ANOMALYDETECTION(val) OVER(PARTITION BY id LIMIT DURATION(hour, 1) WHEN id > 100) FROM input`

### <a name="arguments"></a>Bağımsız Değişkenler

* **scalar_expression** -skaler ifade anomali algılama üzerinden gerçekleştirilir. Bu parametre, Float'ı dahil etmek veya bu dönüş tek bir (sayı) değerini Bigint veri türleri için izin verilen değerler. Joker karakter ifadesini **\*** izin verilmiyor. Skaler ifade, diğer analiz işlevleri veya dış işlevler içeremez. 

* **partition_by_clause** - `PARTITION BY <partition key>` yan tümcesi öğrenme ve eğitim ayrı bölümler arasında böler. Diğer bir deyişle, ayrı bir model değerini kullanılan `<partition key>` ve bu değer yalnızca olaylarla öğrenme ve eğitim Bu modelde için kullanılabilir. Örneğin, aşağıdaki sorgu trenler ve puanları bir başka okumalar yalnızca aynı algılayıcı karşı okuma:

  `SELECT sensorId, reading, ANOMALYDETECTION(reading) OVER(PARTITION BY sensorId LIMIT DURATION(hour, 1)) FROM input`

* **limit_duration yan tümcesi**  `DURATION(<unit>, <length>)` -anomalileri tespit edilirken, zaman aralığını (ne kadar geri geçerli olay geçmişinden) sayılacağı belirtir. Bkz: [DATEDIFF](https://msdn.microsoft.com/azure/stream-analytics/reference/datediff-azure-stream-analytics) desteklenen birimler ve kısaltmalarıyla ayrıntılı bir açıklaması. 

* **when_clause** -anomali algılama hesaplamayı kabul olaylar için bir boolean koşulu belirtir.

### <a name="return-types"></a>Dönüş türleri

Anomalydetectıon işleci, tüm üç puanları çıktı olarak içeren bir kayıt döndürür. Farklı türde anomali algılayıcıları ile ilişkili özellikler şunlardır:

- BiLevelChangeScore
- SlowPosTrendScore
- SlowNegTrendScore

Bireysel kayıt değerleri ayıklamak için **GetRecordPropertyValue** işlevi. Örneğin:

`SELECT id, val FROM input WHERE (GetRecordPropertyValue(ANOMALYDETECTION(val) OVER(LIMIT DURATION(hour, 1)), 'BiLevelChangeScore')) > 3.25` 

Bir anomali puanları bir eşiği geçtiğinde anomali türü algılandı. Eşik kayan noktalı bir sayı olabilir. > = 0. Duyarlılık ve güvenle arasında bir denge eşiğin. Örneğin, düşük bir Eşikte, algılama daha hassas değişiklikler yapmak ve daha yüksek bir eşiği algılama daha az hassas ve daha fazla emin olun ancak bazı anomalileri maske ise daha fazla uyarı oluşturur. Kullanılacak tam eşik değeri, senaryoya bağlıdır. Üst sınır yoktur ancak önerilen aralık 3,25-5'tir. 

' % S'değeri örnekte gösterilen 3,25 yalnızca bir önerilen başlangıç noktasıdır. Kendi veri kümesindeki işlemler çalıştırarak değeri ayarlamanıza ve fazla eşiği ulaşana kadar çıkış değeri gözlemleyin.

## <a name="anomaly-detection-algorithm"></a>Anomali algılama algoritması

* Anomalydetectıon işleci kullanan bir **Denetimsiz öğrenme** burada bunu değil varsayar olaylar dağıtım herhangi bir türde bir yaklaşım. Genel olarak, iki modeli, herhangi bir zamanda burada bunlardan birinin Puanlama için kullanılır ve diğeri arka planda eğitildi verilen paralel olarak korunur. Anomali algılama modelleri, bant dışı bir mekanizma kullanılarak yerine geçerli akıştan verileri kullanarak eğitilir. Eğitim için kullanılan veri miktarı sınırı süre parametresini içinde kullanıcı tarafından belirtilen pencere boyutu d bağlıdır. Her model, olayları 2B değerinde d temel alınarak geliştirilen yukarı sona erer. En iyi sonuçlar için her penceresinde en az 50 olayda olması önerilir. 

* Anomalydetectıon işleci kullanır **kayan pencere semantiği** model ve puanlama olayları eğitmek için. Her olay için anomali değerlendirilir ve bir puan döndürülen anlamına gelir. Puan, anomali güvenirlik düzeyini göstergesidir. 

* Anomalydetectıon işleci sağlayan bir **yinelenebilirliği garantisi** her zaman giriş aynı iş çıktısı bağımsız olarak aynı puanı başlangıç zamanı üretir. İş çıkışı başlangıç zamanı, ilk çıkış olayı iş tarafından üretilen süresini temsil eder. (İşin çıktı daha önce oluşturduysa) geçerli zamanı, özel bir değer veya son çıkış saati için kullanıcı tarafından ayarlanır. 

### <a name="training-the-models"></a>Modellerin eğitimi 

Zaman ilerledikçe, modelleri, farklı verilerle eğitilir. Puanları anlamlı için bu temel mekanizması olarak modeli eğitilir anlamanıza yardımcı olur. Burada, **t<sub>0</sub>**  olduğu **iş çıkışı başlangıç zamanı** ve **d** olduğu **pencere boyutunu** Limit Duration'den parametre. Varsayar içine zaman bölünmüş **atlama boyutu d**, başlayıp `01/01/0001 00:00:00`. Aşağıdaki adımlar, model ve puanlama olayları eğitmek için kullanılır:

* Bir iş başlatıldığında, başlangıç saati t verileri okur<sub>0</sub> – 2B.  
* Zaman sonraki atlama ulaştığında yeni bir modeli M1 oluşturulur ve eğitim başlatır.  
* Başka bir atlama tarafından zaman ilerler, yeni bir modeli M2 oluşturulur ve eğitim başlatır.  
* Zaman t ulaştığında<sub>0</sub>M1 etkin yapılan ve kendi puanı yüzdelik başlatır.  
* Bir sonraki atlamada aynı anda üç şey olur:  

  * M1 artık gerekli değildir ve göz ardı edilir.  
  * Puanlama için kullanıldığı şekilde M2 yeterince eğitim.  
  * Yeni bir modeli M3 oluşturulur ve arka planda eğitilmiş başlatır.  

* Her bir atlama için bu döngü tekrarlanıyordu etkin bir modelle göz ardı edilir olduğunda, paralel modeline geçiş ve arka planda üçüncü modeli başlatın. 

Diagrammatically, adımları şu şekilde görünür: 

![Makine öğrenimi modellerini eğitim](media/stream-analytics-machine-learning-anomaly-detection/machine-learning-training-model.png)

|**Model** | **Eğitim başlangıç zamanı** | **Kendi puanı kullanmaya başlama zamanı** |
|---------|---------|---------|
|M1     | 11:20   | 11:33   |
|M2     | 11:30   | 11:40   |
|M3     | 11:40   | 11:50   |

* Model M1 11: 13'te okuma iş başlatıldıktan sonra sonraki atlama olduğu sabah 11:20, eğitim başlatır. İlk çıkış M1 ' sabah 11:33 13 dakikalık veri eğitimlerle sonra oluşturulur. 

* Yeni bir model M2 de eğitim 11: 30'da başlar ancak kendi puan 11:40 ile 10 dakika veri eğitilmiş sonra olan kadar kullanılmayan. 

* M3 M2 olarak aynı deseni izler. 

### <a name="scoring-with-the-models"></a>Puanlama modelleri ile 

Machine Learning düzeyinde anomali algılama algoritması, olayların bir geçmiş penceresinde ile karşılaştırarak gelen her olay için bir strangeness değer hesaplar. Strangeness hesaplama anomali her tür için farklıdır.  

Ayrıntılı strangeness hesaplama gözden geçirelim (Geçmiş windows olayları içeren bir dizi var olduğunu varsayın): 

1. **İki yönlü düzey değişikliği:** Geçmiş penceresinde bağlı olarak, normal işlem aralığı [10 yüzdelik dilim, 90. yüzdebirlik] hesaplanan üst sınırı olarak alt sınırı ve 90'ıncı yüzdelik dilim değeri olarak diğer bir deyişle, 10 yüzdelik dilim değeri. İçin geçerli olay strangeness değer olarak hesaplandı:  

   - event_value normal çalışma aralığında ise 0  
   - event_value/90th_percentile, eğer event_value > 90th_percentile  
   - 10th_percentile/event_value event_value ise, < 10th_percentile  

2. **Yavaş pozitif eğilimi:** bir eğilim çizgisi Geçmiş penceresinde olay değerleri üzerinden hesaplanır ve pozitif bir eğilim satır işlemi arar. Strangeness değeri olarak hesaplandı:  

   - Eğim pozitifse eğimi  
   - Aksi takdirde 0 

3. **Yavaş negatif eğilimi:** işlemi satır içinde negatif eğilim arar ve bir eğilim çizgisi Geçmiş penceresinde olay değerleri üzerinden hesaplanır. Strangeness değeri olarak hesaplandı: 

   - Eğim negatifse eğimi  
   - Aksi takdirde 0  

Gelen olay strangeness değeri Hesaplandı sonra martingale değer strangeness değeri temel alınarak hesaplanır (bkz [Machine Learning blog](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/) martingale değerin nasıl hesaplanır ilişkin ayrıntılar için). Bu martingale değer anomali puanı döndürülür. Martingale değeri algılayıcısı ara sıra değişiklikler sağlam kalmasına izin verir ve yanlış uyarıların azaltan yanıt olarak garip değerleri yavaş artar. Ayrıca, kullanışlı bir özellik vardır: 

Olasılık [t böyle vardır, M<sub>t</sub> > λ] < 1/λ nerede M<sub>t</sub> anlık t martingale değerdir ve λ gerçek bir değerdir. Örneğin, bir uyarı varsa, M<sub>t</sub>> 100 ve hatalı Pozitiflerin olasılığını olan 1/100'den küçük.  

## <a name="guidance-for-using-the-bi-directional-level-change-detector"></a>İki yönlü düzeyi kullanılarak Kılavuzu algılayıcısı değiştirme 

İki yönlü düzeyi değişiklik algılayıcısı, güç kesintisi ve kurtarma veya yoğun trafik, vb. gibi senaryolarda kullanılabilir. Ancak, bir modeli belirli verilerle eğitilir sonra yeni değeri önceki üst sınırından daha yüksektir ve yalnızca, başka bir düzey değişikliği anormal şekilde çalışır (yukarı düzeyini değiştirmek çalışması) veya önceki alt sınırı (aşağı düzeyi daha düşük Durum değişikliği). Bu nedenle, bir model, eğitim penceresinde anormal olarak kabul edilmesi bunları veri değerleri olan yeni bir düzeyi (yukarı veya aşağı) aralığında açmamalıdır. 

Aşağıdaki noktaları bu algılayıcısı kullanarak göz önünde bulundurulması: 

1. Zaman serisi aniden gördüğünde artışa veya değerinde bırakın, algıladığı Anomalydetectıon işleci. Ancak dönüş normal algılama daha fazla planlama gerektirir. Zaman serisi en fazla yarı uzunluğu anomali algılama penceresi Anomalydetectıon işleci'nın ayarlayarak gerçekleştirilebilir anomali önce kararlı durumda olduğunda. Bu senaryo varsayar anomali en düşük süresi önceden tahmin edilebilir ve yeterince modeli eğitmek için o zaman çerçevesi içinde yeterli olaylar vardır (en az 50 olaylar). 

   Bu Şekil 1 ve 2 (aynı mantığı alt sınırı değişiklik uygulanır) bir üst sınır değişikliği kullanarak aşağıda gösterilmektedir. Her iki şekilde, dalga biçimleri anormal bir düzey değişikliği var. Turuncu dikey çizgileri atlama sınırlar belirtmek ve atlama boyutu Anomalydetectıon işleci belirtilen algılama penceresi ile aynıdır. Yeşil satırlar eğitim penceresinin boyutunu gösterir. Şekil 1'de atlama boyutu hangi anomali bağlanabilmelerini süresi ile aynıdır. Şekil 2'de, anomali sürer yarım saat atlama boyutudur. Modeli Puanlama için kullanılan normal veriler üzerine geliştirilen çünkü her durumda, Yukarı herhangi bir değişiklik algılandı. Ancak, çift yönlü düzeyi değişiklik algılayıcısı nasıl çalıştığını bağlı olarak, bu normal değerler normal dönüş puanlar modeli için kullanılan eğitim penceresinden hariç tutmanız gerekir. Normal dön algılanamıyor bu nedenle Şekil 1'de, bazı normal olaylar Puanlama modeli eğitimi içerir. Ancak Şekil 2'de eğitim yalnızca normal dönüş algılandığını sağlar anormal bölümünü içerir. Daha büyük bir şey biraz normal olaylar da dahil olmak üzere yedekleme sona erecek ise yarısından küçük herhangi bir de aynı nedenden dolayı çalışır. 

   ![Pencere boyutuna eşit anomali uzunluğunda bir AD](media/stream-analytics-machine-learning-anomaly-detection/windowsize-equal-anomaly-length.png)

   ![Pencere boyutu AD'ye anomali uzunluğunun yarısı eşittir](media/stream-analytics-machine-learning-anomaly-detection/windowsize-equal-half-anomaly-length.png)

2. Anomali uzunluğunu tahmin edilemez olduğu durumlarda, bu algılayıcısı en iyi çaba çalışır. Ancak, daha dar bir zaman penceresi sınırlar eğitim verileri seçme, hangi dönüş normal algılama olasılığı artacaktır. 

3. Aşağıdaki senaryoda, eğitim pencere zaten aynı yüksek değerin bir anomali içeren olarak uzun anomali algılanan değil. 

   ![Algılanan anomaliler aynı boyutu](media/stream-analytics-machine-learning-anomaly-detection/anomalies-with-same-length.png)

## <a name="example-query-to-detect-anomalies"></a>Örnek sorgu anomalileri algılamak için 

Aşağıdaki sorgu, bir anomali algılanırsa bir uyarı çıktısını almak için kullanılabilir.
Giriş akışı Tekdüzen olmadığı durumlarda, toplama adım Tekdüzen zaman seri içinde dönüştürme yardımcı olabilir. Ortalama örnek kullanır ancak toplama türünü kullanıcı senaryoya bağlıdır. Ayrıca, zaman serisi boşlukları toplama penceresi büyük olduğunda, yok meydana gelen olayları zaman serisinde tetikleyici anomali algılama (göre penceresi semantiği kayan). Sonuç olarak, sonraki olay geldiğinde gerekmemesi varsayımına bozulur. Bu gibi durumlarda, zaman serisi boşlukları doldurulması gerekir. Olası bir yaklaşım son olayın her atlama penceresinde, aşağıda gösterildiği gibi gerçekleşir.

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

* Altı akış birimleri için işleri kullanın. 
* Olayları, birbirinden en az bir saniye gönderirsiniz.
* Anomalydetectıon işleci kullanarak bölümlenmemiş bir sorgu ortalama yaklaşık 25 ms hesaplama gecikme süresiyle sonuçlar üretebilir.
* Hesaplamaların sayısını daha yüksek olduğu gibi bölümlenmiş bir sorgu tarafından karşılaşılan gecikme sürelerini bölüm sayısı ile biraz farklılık gösterir. Ancak, gecikme süresi için karşılaştırılabilir olaylarının toplam sayısı bölümlenmemiş durumda aynı tüm bölümler hakkındadır.
* Gerçek zamanlı verileri okunurken, büyük miktarda veri hızlı bir şekilde alınır. Bu veri işleme şu anda yavaştır. Pencere boyutu veya olay aralığı yerine pencere veri noktası sayısı ile doğrusal olarak artırmak için gecikme süresi gibi senaryolarda bulunamadı. Gerçek zamanlı durumlarda gecikme süresini azaltmak için daha küçük bir pencere boyutu göz önünde bulundurun. Alternatif olarak, geçerli saatten işinizi başlamayı düşünün. Bölümlenmemiş bir sorgu gecikme süreleri birkaç örnek: 
    - Algılama penceresinde 60 veri noktalarının bir aktarım hızı 3 MB / 10 saniye gecikmeyle sonuçlanabilir. 
    - 600 veri noktalarını, gecikme süresi yaklaşık 80 saniye aktarım hızı 0,4 MB/sn ile ulaşabilirsiniz.

## <a name="limitations-of-the-anomalydetection-operator"></a>Anomalydetectıon işleci sınırlamaları 

* Bu işleç, depo ve DIP algılama şu anda desteklemiyor. Ani artışlar ve düşüşler spontaneous ya da kısa süreli zaman serisinde değişikliklerdir. 

* Bu işleç, şu anda mevsimsellik desenleri işlemez. Mevsimsellik desenleri yinelenen desenlerini, örneğin farklı web trafiği davranışı hafta sonları gibi veya anomalileri ancak beklenen desene davranış olmayan farklı alışveriş eğilimleri kara Cuma sırasında verileri alır. 

* Bu işleç, Giriş zaman serisi tekdüzen olmasını bekliyor. Bir olay akışında bir atlayan üzerinde toplama veya atlamalı pencere Tekdüzen yapılabilir. Olayları arasındaki boşluk her zaman bir toplama penceresi küçük olduğu senaryolarda bir atlayan pencere zaman serisi Tekdüzen yapmanız yeterlidir. Boşluk daha büyük olabilir, boşlukları atlayan bir atlamalı pencere kullanarak son değeri tekrarlayarak doldurulabilir. 

## <a name="references"></a>Başvurular

* [Anomali algılama-zaman serisi verilerinde görülen prosesler algılamak için kullanarak machine learning](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/)
* [Machine learning anomali algılama API'si](https://docs.microsoft.com/azure/machine-learning/machine-learning-apps-anomaly-detection-api)
* [Zaman serisi anomali algılama](https://msdn.microsoft.com/library/azure/mt775197.aspx)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

