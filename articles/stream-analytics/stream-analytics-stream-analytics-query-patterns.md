---
title: Azure Stream Analytics ortak sorgu kalıpları
description: Bu makalede, bir dizi ortak sorgu kalıpları ve Azure Stream Analytics işlerine yararlı tasarımları açıklanmaktadır.
services: stream-analytics
author: jseb225
manager: kfile
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/08/2017
ms.openlocfilehash: 417517cbbd187d32b84cc0a78f7b68a5fcf8eb23
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Örnekler ortak Stream Analytics kullanım desenlerini için sorgu

## <a name="introduction"></a>Giriş
Azure Stream Analytics sorgularda bir SQL benzeri sorgu dili ifade edilir. Dil yapıları belgelenmiştir [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx) Kılavuzu. 

Sorgu Tasarım olay verilerini bir giriş akışından başka bir çıkış veri deposuna taşımak için basit bir geçiş mantık hızlı. Veya TollApp örnek olduğu gibi çeşitli zaman pencereleri üzerinden toplamları hesaplamak için zengin desen eşleştirme ve zamana bağlı analizi yapabilirsiniz. Akış olaylarını birleştirmek ve olay değerleri zenginleştirmek için statik başvuru verileri karşı arama yapmak için birden çok girişi verileri birleştirebilirsiniz. Ayrıca, birden çok çıkış veri yazabilirsiniz.

Bu makalede gerçek senaryolarını temel alarak, birçok ortak sorgu kalıpları çözümleri özetlenmektedir. Bu iş sürüyor ve düzenli olarak yeni desenlerle güncelleştirilmesi devam eder.

## <a name="query-example-convert-data-types"></a>Sorgu örnek: veri türlerini dönüştürme
**Açıklama**: Giriş akışı üzerinde özelliklerin türleri tanımlayın.
Örneğin, araba ağırlık giriş akışta dize olarak geliyor ve dönüştürülmesi gerekiyor **INT** gerçekleştirmek için **toplam** bunun.

**Giriş**:

| Yapma | Zaman | Ağırlık |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |"1000" |
| Honda |2015-01-01T00:00:02.0000000Z |"2000" |

**Çıktı**:

| Yapma | Ağırlık |
| --- | --- |
| Honda |3000 |

**Çözüm**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Açıklama**: kullanım bir **CAST** deyiminde **ağırlık** alanın veri türünü belirtin. Desteklenen veri türleri listesini görmek [veri türleri (Azure akış analizi)](https://msdn.microsoft.com/library/azure/dn835065.aspx).

## <a name="query-example-use-likenot-like-to-do-pattern-matching"></a>Sorgu örnek: kullanımı gibi/değil ister eşleştirme deseni
**Açıklama**: olay bir alanın değerini belirli bir desene eşleşip eşleşmediğini denetleyin.
Örneğin, sonuç ile başlamalı ve 9 ile bitmelidir lisans kalıplarını verir denetleyin.

**Giriş**:

| Yapma | LicensePlate | Zaman |
| --- | --- | --- |
| Honda |ABC-123 |2015-01-01T00:00:01.0000000Z |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC-369 |2015-01-01T00:00:03.0000000Z |

**Çıktı**:

| Yapma | LicensePlate | Zaman |
| --- | --- | --- |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC-369 |2015-01-01T00:00:03.0000000Z |

**Çözüm**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Açıklama**: kullanım **gibi** denetlemek için deyimi **LicensePlate** alan değer. Bu bir ile başlayan sonra sıfır veya daha fazla karakterden oluşan herhangi bir dize olması ve ardından 9 ile bitmesi gerekir. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Sorgu örnek: farklı durumlarda/değerleri (CASE deyimleri) için mantığı belirtin
**Açıklama**: belirli bir ölçütü temel alan bir alan için farklı bir hesaplama sağlar.
Örneğin, 1 için özel bir durum ile aynı kaç araba yapmak için dize açıklamasını geçirilen sağlar.

**Giriş**:

| Yapma | Zaman |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Çıktı**:

| CarsPassed | Zaman |
| --- | --- | --- |
| 1 Honda |2015-01-01T00:00:10.0000000Z |
| 2 Toyotas |2015-01-01T00:00:10.0000000Z |

**Çözüm**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Açıklama**: **durumda** yan tümcesi (Bu örnekte, toplama penceresinde araba sayısı) bazı ölçütlere göre farklı bir hesaplama sağlamamız sağlar.

## <a name="query-example-send-data-to-multiple-outputs"></a>Sorgu örnek: birden çok çıkış veri gönderme
**Açıklama**: veri gönderme için birden çok çıktı hedefi tek bir işten.
Örneğin, eşik tabanlı bir uyarı için verileri çözümlemek ve blob depolama alanına tüm olayların arşivlenmesini.

**Giriş**:

| Yapma | Zaman |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output1**:

| Yapma | Zaman |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output2**:

| Yapma | Zaman | Sayı |
| --- | --- | --- |
| Toyota |2015-01-01T00:00:10.0000000Z |3 |

**Çözüm**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Açıklama**: **INTO** yan tümcesi söyler akış analizi, bu deyim için verileri yazmak için çıktı.
İlk sorguyu biz adlı bir çıktı aldık verilerin bir doğrudan sorgudur **ArchiveOutput**.
Bazı basit toplama ve filtreleme ve ikinci sorguyu mu sonuçları bir aşağı akış uyarı sisteme gönderir.

Not ortak tablo ifadelerinde (Cte'lerin) sonuçlarını da kullanabilirsiniz (gibi **ile** deyimleri) birden çok çıktı deyimlerinde. Bu seçenek daha az giriş kaynağı okuyucularına açma avantaj vardır.
Örneğin: 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-count-unique-values"></a>Sorgu örnek: Say benzersiz değerler
**Açıklama**: bir zaman penceresi içinde akışında görünür benzersiz alan değerleri sayar.
Örneğin, kaç tane benzersiz bir 2 saniyelik penceresinde Ücretli Stand geçtiğini araçların yapar?

**Giriş**:

| Yapma | Zaman |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Çıktı:**

| CountMake | Zaman |
| --- | --- |
| 2 |2015-01-01T00:00:02.000Z |
| 1 |2015-01-01T00:00:04.000Z |

**Çözüm:**

````
SELECT
     COUNT(DISTINCT Make) AS CountMake,
     System.TIMESTAMP AS TIME
FROM Input TIMESTAMP BY TIME
GROUP BY 
     TumblingWindow(second, 2)
````


**Açıklama:**
**sayısı (ayrı olun)** ayrı değerleri sayısını döndürür **olun** sütunu bir zaman penceresi içinde.

## <a name="query-example-determine-if-a-value-has-changed"></a>Sorgu örnek: bir değer değişip değişmediğini belirleyen
**Açıklama**: geçerli değerden farklı olup olmadığını belirlemek için önceki bir değeri bakın.
Örneğin, önceki araba Ücretli yolda geçerli araba olarak aynı marka mi?

**Giriş**:

| Yapma | Zaman |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Çıktı**:

| Yapma | Zaman |
| --- | --- |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Çözüm**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Açıklama**: kullanım **GECİKME** Giriş akışı bir olay geri gözatma ve almak için **olun** değeri. Kendisine karşılaştırmak **olun** değeri geçerli olayında ve bunlar farklıysa olay çıktı.

## <a name="query-example-find-the-first-event-in-a-window"></a>Sorgu örnek: bir pencerede ilk olay Bul
**Açıklama**: ilk arabanızın her 10 dakika arayla bulun.

**Giriş**:

| LicensePlate | Yapma | Zaman |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Çıktı**:

| LicensePlate | Yapma | Zaman |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |

**Çözüm**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Şimdi şimdi sorun değiştirin ve belirli olun, ilk arabanızın her 10 dakika arayla bulabilirsiniz.

| LicensePlate | Yapma | Zaman |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Çözüm**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-the-last-event-in-a-window"></a>Sorgu örnek: bir penceresinde son olayı bulun
**Açıklama**: her 10 dakika arayla son araba bulun.

**Giriş**:

| LicensePlate | Yapma | Zaman |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Çıktı**:

| LicensePlate | Yapma | Zaman |
| --- | --- | --- |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Çözüm**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Açıklama**: sorguda iki adımı vardır. İlk 10 dakikalık Windows'da son zaman damgası bulur. İkinci adım her penceresinde son zaman damgalarının eşleşmesi olaylarını bulmak için özgün akış ile ilk sorgu sonuçları birleştirir. 

## <a name="query-example-detect-the-absence-of-events"></a>Sorgu örnek:, olay eksikliklerini Algıla
**Açıklama**: bir akış belirli bir ölçütle eşleşen herhangi bir değer olup olmadığını denetleyin.
Örneğin, aynı marka gelen 2 ardışık araba Ücretli yol Son 90 saniye içinde girdiğiniz?

**Giriş**:

| Yapma | LicensePlate | Zaman |
| --- | --- | --- |
| Honda |ABC-123 |2015-01-01T00:00:01.0000000Z |
| Honda |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Toyota |DEF 987 |2015-01-01T00:00:03.0000000Z |
| Honda |GHI 345 |2015-01-01T00:00:04.0000000Z |

**Çıktı**:

| Yapma | Zaman | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda |2015-01-01T00:00:02.0000000Z |AAA-999 |ABC-123 |2015-01-01T00:00:01.0000000Z |

**Çözüm**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Açıklama**: kullanım **GECİKME** Giriş akışı bir olay geri gözatma ve almak için **olun** değeri. Kendisine karşılaştırmak **olun** değeri geçerli olayında ve aynı olmaları durumunda olay çıktı. Aynı zamanda **GECİKME** önceki araba ilişkin veri almak için.

## <a name="query-example-detect-the-duration-between-events"></a>Sorgu örnek: olaylar arasındaki süreyi Algıla
**Açıklama**: belirli bir olayın süresi bulun. Örneğin, bir web clickstream göz önüne alındığında, bir özellik harcanan zamanı belirler.

**Giriş**:  

| Kullanıcı | Özellik | Olay | Zaman |
| --- | --- | --- | --- |
| user@location.com |RightMenu |Başlatma |2015-01-01T00:00:01.0000000Z |
| user@location.com |RightMenu |End |2015-01-01T00:00:08.0000000Z |

**Çıktı**:  

| Kullanıcı | Özellik | Süre |
| --- | --- | --- |
| user@location.com |RightMenu |7 |

**Çözüm**:

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Açıklama**: kullanım **son** son almak için işlevi **zaman** değer olay türünü olduğu zaman **Başlat**. **Son** işlev kullandığı **[kullanıcı] bölümü tarafından** sonucu benzersiz kullanıcı başına hesaplanır belirtmek için. 1 saat maksimum eşik arasındaki zaman farkı için sorgu sahip **Başlat** ve **durdurmak** olayları, ancak gerektiği gibi yapılandırılabilir **(sınırı DURATION(hour, 1)**.

## <a name="query-example-detect-the-duration-of-a-condition"></a>Sorgu örnek: bir koşul süresini Algıla
**Açıklama**: Bul ne kadar giden bir koşul oluştu.
Örneğin, bir hata (yukarıda 20.000 sterlin) yanlış bir ağırlık sahip tüm araba kaynaklandığını varsayalım. İşlem süresi hatanın istiyoruz.

**Giriş**:

| Yapma | Zaman | Ağırlık |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |2000 |
| Toyota |2015-01-01T00:00:02.0000000Z |25000 |
| Honda |2015-01-01T00:00:03.0000000Z |26000 |
| Toyota |2015-01-01T00:00:04.0000000Z |25000 |
| Honda |2015-01-01T00:00:05.0000000Z |26000 |
| Toyota |2015-01-01T00:00:06.0000000Z |25000 |
| Honda |2015-01-01T00:00:07.0000000Z |26000 |
| Toyota |2015-01-01T00:00:08.0000000Z |2000 |

**Çıktı**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z |2015-01-01T00:00:07.000Z |

**Çözüm**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Açıklama**: kullanım **GECİKME** 24 saat için giriş akışı görüntülemek ve aramak için örnekler where **StartFault** ve **StopFault** ağırlığa göre yayılmış < 20000.

## <a name="query-example-fill-missing-values"></a>Sorgu örnek: eksik değerleri doldurma
**Açıklama**: eksik değerleri olan olayları akış için düzenli aralıklarla olaylarla akışı üretir.
Örneğin, en son görülen veri noktası raporları 5 saniyede bir olayı oluşturur.

**Giriş**:

| T | değer |
| --- | --- |
| "2014-01-01T06:01:00" |1 |
| "2014-01-01T06:01:05" |2 |
| "2014-01-01T06:01:10" |3 |
| "2014-01-01T06:01:15" |4 |
| "2014-01-01T06:01:30" |5 |
| "2014-01-01T06:01:35" |6 |

**Çıktı (ilk 10 satır)**:

| windowend | lastevent.t | lastevent.Value |
| --- | --- | --- |
| 2014-01-01T14:01:00.000Z |2014-01-01T14:01:00.000Z |1 |
| 2014-01-01T14:01:05.000Z |2014-01-01T14:01:05.000Z |2 |
| 2014-01-01T14:01:10.000Z |2014-01-01T14:01:10.000Z |3 |
| 2014-01-01T14:01:15.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:20.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:25.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:30.000Z |2014-01-01T14:01:30.000Z |5 |
| 2014-01-01T14:01:35.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:40.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:45.000Z |2014-01-01T14:01:35.000Z |6 |

**Çözüm**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Açıklama**: Bu sorguyu her 5 saniyede olaylar oluşturur ve daha önce alındı son olay çıkarır. [Hopping penceresi](https://msdn.microsoft.com/library/dn835041.aspx "Hopping penceresi--Azure akış analizi") süre belirler (Bu örnekte 300 saniye) en son olay bulmak için ne kadar geri sorgu arar.


## <a name="query-example-correlate-two-event-types-within-the-same-stream"></a>Sorgu örnek: iki olay türleri aynı akışındaki ilişkilendirmek
**Açıklama**: bazen belirli bir zaman aralığı içinde oluştu birden çok olay türlerine dayanan uyarıları oluşturmak ihtiyacımız.
Örneğin, ev fırınlar IOT senaryosunda, fan sıcaklık değerinden 40 olduğunda ve son 3 dakika sırasında maksimum güç 10'dan az olduğu bir uyarı oluşturmadan istiyoruz.

**Giriş**:

| time | deviceId | sensorName | değer |
| --- | --- | --- | --- |
| "2018-01-01T16:01:00" | "Oven1" | "temp" |120 |
| "2018-01-01T16:01:00" | "Oven1" | "power" |15 |
| "2018-01-01T16:02:00" | "Oven1" | "temp" |100 |
| "2018-01-01T16:02:00" | "Oven1" | "power" |15 |
| "2018-01-01T16:03:00" | "Oven1" | "temp" |70 |
| "2018-01-01T16:03:00" | "Oven1" | "power" |15 |
| "2018-01-01T16:04:00" | "Oven1" | "temp" |50 |
| "2018-01-01T16:04:00" | "Oven1" | "power" |15 |
| "2018-01-01T16:05:00" | "Oven1" | "temp" |30 |
| "2018-01-01T16:05:00" | "Oven1" | "power" |8 |
| "2018-01-01T16:06:00" | "Oven1" | "temp" |20 |
| "2018-01-01T16:06:00" | "Oven1" | "power" |8 |
| "2018-01-01T16:07:00" | "Oven1" | "temp" |20 |
| "2018-01-01T16:07:00" | "Oven1" | "power" |8 |
| "2018-01-01T16:08:00" | "Oven1" | "temp" |20 |
| "2018-01-01T16:08:00" | "Oven1" | "power" |8 |

**Çıktı**:

| EventTime | deviceId | Temp | alertMessage | maxPowerDuringLast3mins |
| --- | --- | --- | --- | --- | 
| "2018-01-01T16:05:00" | "Oven1" |30 | "Kısa öğeleri ısıtma hattı" |15 |
| "2018-01-01T16:06:00" | "Oven1" |20 | "Kısa öğeleri ısıtma hattı" |15 |
| "2018-01-01T16:07:00" | "Oven1" |20 | "Kısa öğeleri ısıtma hattı" |15 |

**Çözüm**:

````
WITH max_power_during_last_3_mins AS (
    SELECT 
        System.TimeStamp AS windowTime,
        deviceId,
        max(value) as maxPower
    FROM
        input TIMESTAMP BY t
    WHERE 
        sensorName = 'power' 
    GROUP BY 
        deviceId, 
        SlidingWindow(minute, 3) 
)

SELECT 
    t1.t AS eventTime,
    t1.deviceId, 
    t1.value AS temp,
    'Short circuit heating elements' as alertMessage,
    t2.maxPower AS maxPowerDuringLast3mins
    
INTO resultsr

FROM input t1 TIMESTAMP BY t
JOIN max_power_during_last_3_mins t2
    ON t1.deviceId = t2.deviceId 
    AND t1.t = t2.windowTime
    AND DATEDIFF(minute,t1,t2) between 0 and 3
    
WHERE
    t1.sensorName = 'temp'
    AND t1.value <= 40
    AND t2.maxPower > 10
````

**Açıklama**: ilk sorgu `max_power_during_last_3_mins`, kullanan [hareketli penceresi](https://msdn.microsoft.com/azure/stream-analytics/reference/sliding-window-azure-stream-analytics) son 3 dakika içinde güç algılayıcı her cihaz için en büyük değeri bulmak için. İkinci sorguyu güç değeri en son penceresinde ilgili geçerli olayı için bulmak için ilk sorgu için birleştirilir. Ve daha sonra koşullar sağlanan aygıt için bir uyarı üretilir.


## <a name="get-help"></a>Yardım alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

