---
title: "Önceden yapılandırılmış Uzaktan İzleme çözümünde gezinme | Microsoft Belgeleri"
description: "Azure IOT önceden yapılandırılmış çözümü uzaktan izlemenin ve mimarisinin açıklaması."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 6338750446b33269614c404ecaad8f8192bf1ab2


---
# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Önceden yapılandırılmış uzaktan izleme çözümünde gezinme
## <a name="introduction"></a>Giriş
IoT Paketi önceden yapılandırılmış [uzaktan izleme çözümü][lnk-preconfigured-solutions], uzak konumlarda çalışan birden fazla makine için uçtan uca izleme çözümünün bir uygulamasıdır. Bu çözüm, iş senaryosunun genel bir uygulamasını sağlamak üzere temel Azure hizmetlerini bir araya getirir ve kendi uygulamanız için başlangıç noktası olarak kullanılabilir. Çözümü kendi özel iş gereksinimleriniz için [özelleştirebilirsiniz][lnk-customize].

Bu makalede uzaktan izleme çözümünün nasıl çalıştığını anlamanız için çözümün temel öğelerinden bazıları açıklanmaktadır. Bu bilgiler şunları yapmanıza yardımcı olur:

* Çözümdeki sorunları giderme.
* Çözümü kendinize özel gereksinimleri karşılayacak şekilde nasıl özelleştireceğinizi planlama. 
* Azure hizmetlerini kullanan kendi IoT çözümünüzü tasarlama.

## <a name="logical-architecture"></a>Mantıksal mimari
Aşağıdaki diyagram önceden yapılandırılmış çözümün mantıksal bileşenlerinin ana hatların vermektedir:

![Mantıksal mimari](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="simulated-devices"></a>Sanal cihazlar
Önceden yapılandırılmış çözümde, sanal cihaz bir soğutma cihazını temsil eder (örneğin, bir yapının klimaları veya bir tesisin havalandırma birimi). Önceden yapılandırılmış çözümü dağıttığınızda bir [Azure WebJob][lnk-webjobs] içinde çalışan dört sanal cihazı da otomatik olarak sağlamış olursunuz. Sanal cihazlar herhangi bir fiziksel cihaza dağıtmaya gerek olmadan çözümün davranışını keşfetmenizi kolaylaştırır. Gerçek bir fiziksel cihaz dağıtmak için [Cihazınızı önceden yapılandırılmış uzaktan izleme çözümüne bağlama][lnk-connect-rm] öğreticisine bakın.

Her sanal cihaz IoT Hub'ına aşağıdaki ileti türlerini gönderebilir:

| İleti | Açıklama |
| --- | --- |
| Başlangıç |Cihaz başlatıldığında, arka uca kendisiyle ilgili bilgiler içeren bir **device-info** iletisi gönderir. Bu veriler cihaz kimliğini, cihaz meta verilerini, cihazın desteklediği komutların bir listesini ve cihazın geçerli yapılandırmasını içerir. |
| Varlık |Bir cihaz, cihazın bir sensörün varlığını algılayıp algılamadığını bildirmek üzere düzenli aralıklarla bir **varlık** iletisi gönderir. |
| Telemetri |Bir cihaz, düzenli aralıklarla cihazın sanal sensörlerinden toplanan sıcaklık ve nem sanal değerlerini bildiren bir **telemetri** iletisi gönderir. |

Sanal cihazlar bir **cihaz bilgisi** iletisinde aşağıdaki cihaz özelliklerini gönderir:

| Özellik | Amaç |
| --- | --- |
| Cihaz Kimliği |Çözümde cihaz oluşturulduğunda sağlanan veya atanan kimlik. |
| Üretici |Cihaz üreticisi |
| Model Numarası |Cihazın model numarası |
| Seri Numarası |Cihazın seri numarası |
| Üretici yazılımı |Geçerli cihazdaki üretici yazılımı sürümü |
| Platform |Cihazın platform mimarisi |
| İşlemci |Cihazı çalıştıran işlemci |
| Yüklü RAM |Cihazda yüklü RAM miktarı |
| Hub Etkin Durumu |Cihazın IoT Hub durumu özelliği |
| Oluşturma Zamanı |Çözümde cihazın oluşturulduğu zaman |
| Güncelleştirme Zamanı |Cihaz özelliklerinin en son güncelleştirildiği zaman |
| Enlem |Cihazın enlem konumu |
| Boylam |Cihazın boylam konumu |

Benzetici, örnek değerlerle sanal cihazlarda bu özelliklerin çekirdeğini oluşturur.  Benzeticinin sanal cihazı her başlatışında, cihaz IoT Hub'ına önceden tanımlanmış meta verileri gönderir. Bunun cihaz portalında yapılmış tüm meta veri güncelleştirmelerinin üzerine yazılacağını unutmayın.

Sanal cihazlar çözüm panosundan IoT hub'ı aracılığıyla gönderilen aşağıdaki komutları işleyebilir:

| Komut | Açıklama |
| --- | --- |
| PingDevice |Canlı olup olmadığını denetlemek için cihaza bir *ping* gönderir |
| StartTelemetry |Cihazın telemetri göndermesini başlatır |
| StopTelemetry |Cihazın telemetri göndermesini durdurur |
| ChangeSetPointTemp |Çevresinde rastgele verilerin oluşturulduğu ayar noktası değerini değiştirir |
| DiagnosticTelemetry |Ek bir telemetri değeri (externalTemp) göndermek için cihaz benzeticisini tetikler |
| ChangeDeviceState |Cihazla ilgili genişletilmiş durum özelliğini değiştirir ve cihazdan cihaz bilgi iletisi gönderir |

Çözüm arka ucuna cihaz komut bildirimi IoT hub’ı aracılığıyla gönderilir.

## <a name="iot-hub"></a>IoT Hub’ı
[IoT hub][lnk-iothub], cihazlardan buluta gönderilen verileri alır ve Azure Akış Analizi (ASA) işlerinde kullanılabilir hale getirir. IoT hub ayrıca cihazlarınıza cihaz portalı adına komutlar gönderir. ASA işinin kullandığı her akış, cihazlarınızdaki ileti akışını okumak için ayrı bir IoT Hub tüketici grubu kullanır.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Uzaktan izleme çözümünde [Azure Akış Analizi ][lnk-asa] (ASA), IoT hub tarafından alınan cihaz iletilerini işleme veya depolama amacıyla diğer arka uç bileşenlerine gönderir. Farklı ASA işleri, iletilerin içeriğine göre belirli işlevler gerçekleştirir.

**İş 1: Cihaz bilgileri** gelen ileti akışından cihaz bilgileri iletilerine filtre uygular ve bunları olay hub'ı uç noktasına gönderir. Cihaz, başlangıçta ve **SendDeviceInfo** komutuna yanıt olarak cihaz bilgi iletileri gönderir. Bu iş **device-info** iletilerini tanımlamak için aşağıdaki sorgu tanımını kullanır:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Bu iş daha ayrıntılı işleme için Olay Hub’ına çıktısını gönderir.

**İş 2: Kurallar** cihaz başına eşiklere karşılık gelen sıcaklık ve nem telemetrisi değerlerini değerlendirir. Eşik değerleri çözüm panosunda yer alan kurallar düzenleyicisinde ayarlanır. Her cihaz/değer çifti, Akış Analizi’nin **Başvuru Verileri** olarak okuduğu blob’daki zaman damgası tarafından depolanır. İş, boş olmayan değerleri cihaz için ayarlanan eşikle karşılaştırır. ' >' koşulunu aşarsa, iş eşiğin aşıldığını belirten bir **alarm** olayı verir; cihaz, değer ve zaman damgası değerlerini de sağlar. Bu iş bir alarm tetiklemesi gereken telemetri iletilerini belirlemek üzere aşağıdaki sorgu tanımını kullanır:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

İş daha ayrıntılı işleme için çıktısını Olay Hub’ına gönderir ve her bir uyarının ayrıntılarını, çözüm panosunun uyarı bilgilerini okuyabileceği blob depolamaya kaydeder.

**ş 3: Telemetri**, gelen cihaz telemetrisi akışını iki yolla çalıştırır. İlk olarak tüm telemetri iletilerini cihazlardan uzun süreli depolama için kalıcı blob depolamaya gönderir. İkincisi beş dakikalık bir kayan pencerede ortalama, en düşük ve en yüksek nem değerlerini ölçer ve bu verileri blob depolamaya gönderir. Çözüm panosu blob depolama alanından telemetri verilerini okuyarak grafikleri doldurur. Bu iş şu sorgu tanımını kullanır:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Event Hubs
ASA işleri **cihaz bilgisi** ve **kurallar** verilerini, WebJob’da çalışan **Olay İşleyicisi**’ne güvenli bir şekilde iletmek üzere Event Hubs’a gönderir.

## <a name="azure-storage"></a>Azure Storage
Çözüm, çözümdeki cihazlarda bulunan tüm ham ve özet telemetri verilerini kalıcı hale getirmek için Azure Blob Depolama kullanır. Pano blob depolama alanından telemetri verilerini okuyarak grafikleri doldurur. Uyarıları görüntülemek için pano, telemetri değerleri yapılandırılmış eşik değerlerini aştığında kayıt altına alan blob depolama alanından verileri okur. Çözüm, panodaki sizin ayarladığınız eşik değerlerini kaydetmek için de blob depolama alanını kullanır.

## <a name="webjobs"></a>WebJobs
WebJobs cihaz benzeticilerini barındırmaya ek olarak çözüm içinde cihaz bilgi iletilerini ve komut yanıtlarını işleyen bir Azure WebJob içinde çalışan **Olay İşleyicisi**’ni de barındırır. Şunları kullanır:

* Cihaz kayıt defterini (DocumentDB veritabanında depolanır) güncelleştirmek için geçerli cihaz bilgilerinin bulunduğu cihaz bilgileri iletileri.
* Cihaz komut geçmişini (DocumentDB veritabanında depolanır) güncelleştirmek için komut yanıtı iletileri.

## <a name="documentdb"></a>DocumentDB
Çözüm, kendisine bağlı cihazlarla ilgili bilgileri depolamak için bir DocumentDB veritabanı kullanır. Bu bilgiler, cihaz meta verilerini ve panodan cihazlara gönderilen komutların geçmişini içerir.

## <a name="web-apps"></a>Web uygulamaları
### <a name="remote-monitoring-dashboard"></a>Uzaktan izleme panosu
Web uygulamasındaki bu sayfa, cihazlardaki telemetri verilerini görselleştirmek için PowerBI javascript denetimlerini kullanır (bkz. [PowerBI-visuals repo](https://www.github.com/Microsoft/PowerBI-visuals)). Çözüm, blob depolama alanına telemetri verilerini yazmak için ASA telemetri işini kullanır.

### <a name="device-administration-portal"></a>Cihaz yönetim portalı
Bu web uygulamasıyla şunları yapabilirsiniz:

* Yeni bir cihaz hazırlayın. Bu eylem, benzersiz cihaz kimliğini ayarlar ve kimlik doğrulaması anahtarını oluşturur. Hem IoT Hub kimlik kayıt defterine hem de çözüme özel DocumentDB veritabanına cihaz hakkındaki bilgileri yazar.
* Cihaz özelliklerini yönetin. Bu eylem, mevcut özellikleri görüntülemeyi ve yeni özelliklerle güncelleştirmeyi kapsar.
* Cihaza komut gönderme.
* Cihaz için komut geçmişini görüntüleme.
* Cihazları etkinleştirin ve devre dışı bırakın.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki TechNet blog gönderileri önceden yapılandırılmış uzaktan izleme çözümü hakkında daha ayrıntılı bilgi sağlar:

* [IoT Paketi - Başlık Altında - Uzaktan İzleme](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
* [IoT Paketi - Uzaktan İzleme - Canlı ve Sanal Cihaz Ekleme](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Aşağıdaki makaleleri okuyarak IoT Paketi ile çalışmaya başlayabilirsiniz:

* [Cihazınızı önceden yapılandırılmış uzaktan izleme çözümüne bağlama][lnk-connect-rm]
* [azureiotsuite.com sitesindeki izinler][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md



<!--HONumber=Nov16_HO2-->


