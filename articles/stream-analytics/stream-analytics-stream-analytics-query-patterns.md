---
title: Azure Stream analytics'te ortak sorgu desenleri
description: Bu makalede, bir dizi ortak sorgu kalıpları ve Azure Stream Analytics işlerinde yararlı olan tasarımlar açıklanır.
services: stream-analytics
author: jseb225
manager: kfile
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/08/2017
ms.openlocfilehash: 7f171fa1eb8c91b55119d0308b57fe3d3e70261b
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578900"
---
# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Örnekler için sık kullanılan Stream Analytics kullanım desenlerini sorgulama

## <a name="introduction"></a>Giriş
Azure Stream Analytics sorguları bir SQL benzeri bir sorgu dilinde ifade edilir. Dil yapıları bölümünde belgelendirilen [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx) Kılavuzu. 

Sorgu Tasarım olay verilerini başka bir çıkış veri deposuna bir giriş akışından taşımak için basit bir geçiş mantıksal ifade edebilirsiniz. Veya TollApp örnek olduğu gibi çeşitli zaman pencereleri üzerinden toplamları hesaplamak için zengin deseni eşleştirme ve geçici analiz gerçekleştirebilirsiniz. Akış olaylarını birleştirin ve olay değerleri zenginleştirmek için statik başvuru verileri karşı aramaları yapmak için birden fazla giriş verilerinden katılabilirsiniz. Ayrıca için birden çok çıkış veri yazabilirsiniz.

Bu makalede, gerçek dünya senaryoları tabanlı, birkaç ortak sorgu kalıpları çözümleri özetlenmektedir. Bu bir süreç ve düzenli olarak yeni desenlerle güncelleştirilmesi devam eder.

## <a name="work-with-complex-data-types-in-json-and-avro"></a>JSON ve AVRO'da karmaşık Veri Türleri ile çalışma 
Azure Stream Analytics, olayları işlemeyi CSV, JSON ve Avro veri biçimlerini destekler.
İç içe geçmiş nesnelerde (kayıtlar) veya diziler gibi karmaşık türler, hem JSON hem de Avro içerebilir. Bu karmaşık veri türleriyle çalışmak için başvurmak [JSON ayrıştırma ve AVRO verileri](stream-analytics-parsing-json.md) makalesi.


## <a name="query-example-convert-data-types"></a>Sorgu örnek: veri türlerini dönüştürme
**Açıklama**: özelliklerin türleri giriş akışında tanımlayın.
Örneğin, araba ağırlık giriş akışında dize olarak geliyor ve dönüştürülmesi gerekiyor **INT** gerçekleştirmek için **toplam** hazır.

**Giriş**:

| Olun | Zaman | Ağırlık |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |"1000" |
| Honda |2015-01-01T00:00:02.0000000Z |"2000" |

**Çıkış**:

| Olun | Ağırlık |
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

**Açıklama**: kullanım bir **ATAMA** deyiminde **ağırlık** alanını kendi veri türü belirtin. Desteklenen veri türleri listesini [veri türleri (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).

## <a name="query-example-use-likenot-like-to-do-pattern-matching"></a>Sorgu örnek: kullanımı gibi/değil ister desen eşleştirme
**Açıklama**: olay bir alan değerini belirli bir desenle eşleşip eşleşmediğini denetleyin.
Örneğin, sonucu bir ile başlayıp 9 ile lisans kalıplarını döndürür denetleyin.

**Giriş**:

| Olun | LicensePlate | Zaman |
| --- | --- | --- |
| Honda |ABC-123 |2015-01-01T00:00:01.0000000Z |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC-369 |2015-01-01T00:00:03.0000000Z |

**Çıkış**:

| Olun | LicensePlate | Zaman |
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

**Açıklama**: kullanım **gibi** denetlemek için bildirimi **LicensePlate** alan değer. Bir ile başlayan ardından sıfır veya daha fazla karakter dizesi sahip ve ardından 9 ile bitmesi gerekir. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Sorgu örnek: mantıksal farklı durumları/değerleri (CASE deyimleri) için belirtin
**Açıklama**: belirli bir ölçütü temel alan bir alan için farklı bir hesaplama sağlayın.
Örneğin, bir özel durum için 1 ile aynı kaç otomobiller yapmak için bir dize açıklamasını geçirilen sağlar.

**Giriş**:

| Olun | Zaman |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Çıkış**:

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

**Açıklama**: **çalışması** ifade sonucu belirlemek için basit bir ifade kümesi bir ifadeyi karşılaştırır. Bu örnekte, 1 sayısı 1 dışında olan vehicle yapar daha farklı dize açıklamasını döndürülen sayısı olan araç sağlar. 

## <a name="query-example-send-data-to-multiple-outputs"></a>Sorgu örnek: birden çok çıkış için veri gönderme
**Açıklama**: veri gönderme için birden çok çıkış hedefi tek bir iş.
Örneğin, eşik tabanlı bir uyarı için verileri analiz etmek ve tüm blob depolama olaylarına arşivleyin.

**Giriş**:

| Olun | Zaman |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output1**:

| Olun | Zaman |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output2**:

| Olun | Zaman | Sayı |
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

**Açıklama**: **INTO** yan tümcesi, Stream Analytics söyler, bu ifade için verileri yazmak için çıktı.
İlk sorgu adlı bir çıktı alınan verilerin bir geçiş diski olduğundan **ArchiveOutput**.
İkinci sorgu bazı basit toplama ve filtreleme ve bunu yapan bir aşağı akış Uyarı sistemine sonuçları gönderir.

Not ortak tablo ifadeleri (Cte'lerin) sonuçlarını da kullanabilirsiniz (gibi **ile** deyimleri) birden çok çıkış deyimlerinde. Bu seçenek, daha az giriş kaynağı okuyucularına açma eklenen avantajına sahiptir.
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

## <a name="query-example-count-unique-values"></a>Sorgu örnek: benzersiz değerleri Say
**Açıklama**: akış bir zaman penceresi içinde görünen benzersiz alan değerleri sayar.
Örneğin, kaç tane benzersiz 2 saniyelik penceresinde Ücretli standına geçtiğini arabaların birçoğu yapar?

**Giriş**:

| Olun | Zaman |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Çıkış:**

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
**COUNT (DISTINCT olun)** benzersiz değerleri döndürür **olun** sütunu bir zaman penceresi içinde.

## <a name="query-example-determine-if-a-value-has-changed"></a>Sorgu örnek: bir değer değişip değişmediğini belirlemek
**Açıklama**: geçerli değerden farklı olup olmadığını belirlemek için bir önceki değerine bakın.
Örneğin, önceki araba Ücretli yolda geçerli araba olarak aynı yap mi?

**Giriş**:

| Olun | Zaman |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Çıkış**:

| Olun | Zaman |
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

**Açıklama**: kullanım **LAG** Giriş akışı bir olaya geri Özet ve almak için **olun** değeri. Kendisine karşılaştırma **olun** değeri geçerli olayı ve farklı olmaları durumunda olay çıktı.

## <a name="query-example-find-the-first-event-in-a-window"></a>Sorgu örnek: bir pencerede ilk olayı bulun
**Açıklama**: her 10 dakikalık aralığındaki ilk araba bulun.

**Giriş**:

| LicensePlate | Olun | Zaman |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Çıkış**:

| LicensePlate | Olun | Zaman |
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

Şimdi şimdi sorun değiştirin ve her 10 dakikalık zaman aralığını belirli olun, ilk araba bulun.

| LicensePlate | Olun | Zaman |
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

## <a name="query-example-find-the-last-event-in-a-window"></a>Sorgu örnek: bir pencerede son olayı bulun
**Açıklama**: her 10 dakikalık aralık son araba bulun.

**Giriş**:

| LicensePlate | Olun | Zaman |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Çıkış**:

| LicensePlate | Olun | Zaman |
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

**Açıklama**: sorguda iki adım vardır. İlk 10 dakikalık windows en son zaman damgasını bulur. İkinci adım, her penceresindeki Son zaman damgaları eşleşen olayları bulmak için özgün stream ile ilk sorgusunun sonuçları birleştirir. 

## <a name="query-example-detect-the-absence-of-events"></a>Örnek Sorgu: var olmayan olayları algılama
**Açıklama**: bir akışı belirli bir ölçütle eşleşen herhangi bir değer olup olmadığını denetleyin.
Örneğin, aynı olun 2 ardışık otomobilleri Ücretli yol Son 90 saniye içinde girdiğiniz?

**Giriş**:

| Olun | LicensePlate | Zaman |
| --- | --- | --- |
| Honda |ABC-123 |2015-01-01T00:00:01.0000000Z |
| Honda |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Toyota |DEF 987 |2015-01-01T00:00:03.0000000Z |
| Honda |GHI 345 |2015-01-01T00:00:04.0000000Z |

**Çıkış**:

| Olun | Zaman | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
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

**Açıklama**: kullanım **LAG** Giriş akışı bir olaya geri Özet ve almak için **olun** değeri. Karşılaştırmak için **olun** değeri geçerli olay ve aynı olmaları durumunda olay çıktıda. Ayrıca **LAG** önceki araba hakkındaki verileri almak için.

## <a name="query-example-detect-the-duration-between-events"></a>Sorgu örnek: olaylar arasındaki süreyi algılayın
**Açıklama**: belirli bir olayın süresi bulun. Örneğin, web tıklama dizisi göz önünde bulundurulduğunda, bir özellik üzerinde harcanan zamanı belirler.

**Giriş**:  

| Kullanıcı | Özellik | Olay | Zaman |
| --- | --- | --- | --- |
| user@location.com |RightMenu |Başlatma |2015-01-01T00:00:01.0000000Z |
| user@location.com |RightMenu |Son |2015-01-01T00:00:08.0000000Z |

**Çıkış**:  

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

**Açıklama**: kullanım **son** son almak için işlevi **zaman** değer olay türü olduğunda **Başlat**. **Son** işlevini kullanan **PARTITION BY [kullanıcı]** sonucu benzersiz kullanıcı başına hesaplanır belirtmek için. Sorguyu 1 saatlik en yüksek eşik arasındaki zaman farkı için sahip **Başlat** ve **Durdur** olayları, ancak gerektiğinde yapılandırılabilir **(sınırı DURATION(hour, 1)**.

## <a name="query-example-detect-the-duration-of-a-condition"></a>Sorgu örnek: süresi bir koşul algılama
**Açıklama**: bulma ne kadar giden bir koşul oluştu.
Örneğin, yanlış bir ağırlık (yukarıda 20.000 pound) sahip tüm otomobiller içinde bir hata ile sonuçlandı ve o hatanın süresi hesaplanan değer olduğunu varsayalım.

**Giriş**:

| Olun | Zaman | Ağırlık |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |2000 |
| Toyota |2015-01-01T00:00:02.0000000Z |25000 |
| Honda |2015-01-01T00:00:03.0000000Z |26000 |
| Toyota |2015-01-01T00:00:04.0000000Z |25000 |
| Honda |2015-01-01T00:00:05.0000000Z |26000 |
| Toyota |2015-01-01T00:00:06.0000000Z |25000 |
| Honda |2015-01-01T00:00:07.0000000Z |26000 |
| Toyota |2015-01-01T00:00:08.0000000Z |2000 |

**Çıkış**:

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

**Açıklama**: kullanım **LAG** 24 saat için giriş akışı görüntülemek ve aramak için örnekleri nereden **StartFault** ve **StopFault** ağırlığa göre dağıtılmış < 20000.

## <a name="query-example-fill-missing-values"></a>Sorgu örnek: eksik değerleri doldurun
**Açıklama**: eksik değerleri olan olay akışı için düzenli aralıklarla ile akışı üretir.
Örneğin, en son görülen veri noktası raporları 5 saniyede bir olay oluşturur.

**Giriş**:

| T | değer |
| --- | --- |
| "2014-01-01T06:01:00" |1 |
| "2014-01-01T06:01:05" |2 |
| "2014-01-01T06:01:10" |3 |
| "2014-01-01T06:01:15" |4 |
| "2014-01-01T06:01:30" |5 |
| "2014-01-01T06:01:35" |6 |

**Çıkış (ilk 10 satırı)**:

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


**Açıklama**: Bu sorguyu her 5 saniyede bir olay oluşturur ve daha önce alınan son olayın çıkarır. [Hopping penceresi](https://msdn.microsoft.com/library/dn835041.aspx "Hopping penceresi--Azure Stream Analytics") süresini belirler (Bu örnekte 300 saniye) en son olayı bulmak için ne kadar bekleyen sorgu arar.


## <a name="query-example-correlate-two-event-types-within-the-same-stream"></a>Sorgu örnek: iki olay türleri aynı akışta içinde ilişkilendirin
**Açıklama**: bazen uyarılar oluşturulacak belirli bir zaman aralığında gerçekleşen birden çok olay türlere göre gerekir.
Örneğin, ev fırınlar bir IOT senaryoda, fan sıcaklık 40'tan küçük ve maksimum güç son 3 dakika boyunca 10'dan küçük olduğunda bir uyarı oluşturulmalıdır.

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

**Çıkış**:

| eventTime | deviceId | Temp | alertMessage | maxPowerDuringLast3mins |
| --- | --- | --- | --- | --- | 
| "2018-01-01T16:05:00" | "Oven1" |30 | "Öğeleri ısıtma kısa devre" |15 |
| "2018-01-01T16:06:00" | "Oven1" |20 | "Öğeleri ısıtma kısa devre" |15 |
| "2018-01-01T16:07:00" | "Oven1" |20 | "Öğeleri ısıtma kısa devre" |15 |

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

**Açıklama**: ilk sorgu `max_power_during_last_3_mins`, kullandığı [hareketli penceresi](https://msdn.microsoft.com/azure/stream-analytics/reference/sliding-window-azure-stream-analytics) son 3 dakika içinde power algılayıcı her cihaz için en büyük değeri bulunacak. İkinci sorgu, güç değeri en son penceresinde ilgili için geçerli olay bulmak için ilk sorgu için birleştirilir. ' İ tıklatın ve ardından, koşullar karşılandığında sağlanan, cihaz için bir uyarı üretilir.

## <a name="query-example-process-events-independent-of-device-clock-skew-substreams"></a>Sorgu örnek: cihaz saat eğriltme (alt akışları) bağımsız olayları işleme
**Açıklama**: olayları geç gelebileceğinden veya olay üreticilerinden arasında saat farklarından kaynaklanan sipariş dışında saat eğriltir bölümleri veya ağ gecikme süresi arasında. Aşağıdaki örnekte, arkasında TollID 1 cihaz saati TollID 2 on saniyedir ve arkasındaki TollID 1 cihaz saati TollID 3 beş saniyedir. 


**Giriş**:
| LicensePlate | Olun | Zaman | TollID |
| --- | --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:01.0000000Z | 1 |
| YHN 6970 |Toyota |2015-07-27T00:00:05.0000000Z | 1 |
| QYF 9358 |Honda |2015-07-27T00:00:01.0000000Z | 2 |
| GXF 9462 |BMW |2015-07-27T00:00:04.0000000Z | 2 |
| VFE 1616 |Toyota |2015-07-27T00:00:10.0000000Z | 1 |
| RMV 8282 |Honda |2015-07-27T00:00:03.0000000Z | 3 |
| MDR 6128 |BMW |2015-07-27T00:00:11.0000000Z | 2 |
| YZK 5704 |Ford |2015-07-27T00:00:07.0000000Z | 3 |

**Çıkış**:
| TollID | Sayı |
| --- | --- |
| 1 | 2 |
| 2 | 2 |
| 1 | 1 |
| 3 | 1 |
| 2 | 1 |
| 3 | 1 |

**Çözüm**:

````
SELECT
      TollId,
      COUNT(*) AS Count
FROM input
      TIMESTAMP BY Time OVER TollId
GROUP BY TUMBLINGWINDOW(second, 5), TollId

````

**Açıklama**: [TIMESTAMP BY OVER](https://msdn.microsoft.com/azure/stream-analytics/reference/timestamp-by-azure-stream-analytics#over-clause-interacts-with-event-ordering) yan tümcesi kullanılarak ayrı alt akışları her cihaz Zaman Çizelgesi'arar. Bunlar, olayların sırasına göre sıralanan yerine tüm cihazlar aynı düzende değilmiş gibi her TollID olduğu anlamına hesaplanır gibi her TollID çıkış olayları üretilir.


## <a name="get-help"></a>Yardım alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

