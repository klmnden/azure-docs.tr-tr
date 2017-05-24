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
ms.date: 02/15/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 57544151cc020e5170ebd231b5e4d8f424aeada0
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Önceden yapılandırılmış uzaktan izleme çözümünde gezinme
## <a name="introduction"></a>Giriş
IoT Paketi önceden yapılandırılmış [uzaktan izleme çözümü][lnk-preconfigured-solutions], uzak konumlarda çalışan birden fazla makine için uçtan uca izleme çözümünün bir uygulamasıdır. Bu çözüm, iş senaryosunun genel uygulamasını sağlamak üzere temel Azure hizmetlerini bir araya getirir. Çözümü kendi uygulamanız için bir başlangıç noktası olarak kullanabilir ve özel iş gereksinimlerinizi karşılayacak şekilde [özelleştirebilirsiniz][lnk-customize].

Bu makalede uzaktan izleme çözümünün nasıl çalıştığını anlamanız için çözümün temel öğelerinden bazıları açıklanmaktadır. Bu bilgiler şunları yapmanıza yardımcı olur:

* Çözümdeki sorunları giderme.
* Çözümü kendinize özel gereksinimleri karşılayacak şekilde nasıl özelleştireceğinizi planlama. 
* Azure hizmetlerini kullanan kendi IoT çözümünüzü tasarlama.

## <a name="logical-architecture"></a>Mantıksal mimari
Aşağıdaki diyagram önceden yapılandırılmış çözümün mantıksal bileşenlerinin ana hatların vermektedir:

![Mantıksal mimari](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="simulated-devices"></a>Sanal cihazlar
Önceden yapılandırılmış çözümde, sanal cihaz bir soğutma cihazını temsil eder (örneğin, bir yapının klimaları veya bir tesisin havalandırma birimi). Önceden yapılandırılmış çözümü dağıttığınızda bir [Azure WebJob][lnk-webjobs] içinde çalışan dört sanal cihazı da otomatik olarak sağlamış olursunuz. Sanal cihazlar herhangi bir fiziksel cihaza dağıtmaya gerek olmadan çözümün davranışını keşfetmenizi kolaylaştırır. Gerçek bir fiziksel cihaz dağıtmak için [Cihazınızı önceden yapılandırılmış uzaktan izleme çözümüne bağlama][lnk-connect-rm] öğreticisine bakın.

### <a name="device-to-cloud-messages"></a>Cihazdan buluta iletiler
Her sanal cihaz IoT Hub'ına aşağıdaki ileti türlerini gönderebilir:

| İleti | Açıklama |
| --- | --- |
| Başlangıç |Cihaz başlatıldığında, arka uca kendisiyle ilgili bilgiler içeren bir **device-info** iletisi gönderir. Bu veriler, cihaz kimliği ile cihazın desteklediği komut ve yöntemlerin listesini içerir. |
| Varlık |Bir cihaz, cihazın bir sensörün varlığını algılayıp algılamadığını bildirmek üzere düzenli aralıklarla bir **varlık** iletisi gönderir. |
| Telemetri |Bir cihaz, düzenli aralıklarla cihazın sanal sensörlerinden toplanan sıcaklık ve nem sanal değerlerini bildiren bir **telemetri** iletisi gönderir. |

> [!NOTE]
> Çözüm, cihaz tarafından desteklenen komutların listesini cihaz ikizinde değil, Cosmos DB veritabanında depolar.
> 
> 

### <a name="properties-and-device-twins"></a>Özellikler ve cihaz ikizleri
Sanal cihazlar IoT hub içindeki [ikize][lnk-device-twins] aşağıdaki cihaz özelliklerini *bildirilen özellikler* olarak gönderir. Cihaz başlangıçta ve bir **Cihaz Durumunu Değiştir** komut ya da yöntemine yanıt olarak bildirilen özellikleri gönderir.

| Özellik | Amaç |
| --- | --- |
| Config.TelemetryInterval | Cihazın telemetri gönderme sıklığı (saniye) |
| Config.TemperatureMeanValue | Sanal sıcaklık telemetrisi için ortalama değeri belirtir |
| Device.DeviceID |Çözümde cihaz oluşturulduğunda sağlanan veya atanan kimlik |
| Device.DeviceState | Cihaz tarafından bildirilen durum |
| Device.CreatedTime |Çözümde cihazın oluşturulduğu zaman |
| Device.StartupTime |Cihazın başlatıldığı saat |
| Device.LastDesiredPropertyChange |En son istenen özellik değişikliğinin sürüm numarası |
| Device.Location.Latitude |Cihazın enlem konumu |
| Device.Location.Longitude |Cihazın boylam konumu |
| System.Manufacturer |Cihaz üreticisi |
| System.ModelNumber |Cihazın model numarası |
| System.SerialNumber |Cihazın seri numarası |
| System.FirmwareVersion |Geçerli cihazdaki üretici yazılımı sürümü |
| System.Platform |Cihazın platform mimarisi |
| System.Processor |Cihazı çalıştıran işlemci |
| System.InstalledRAM |Cihazda yüklü RAM miktarı |

Benzetici, örnek değerlerle sanal cihazlarda bu özelliklerin çekirdeğini oluşturur. Simülatör sanal cihazı her başlattığında, cihaz IoT Hub'ına önceden tanımlanmış meta verileri bildirilen özellik olarak gönderir. Bildirilen özellikler yalnızca cihaz tarafından güncelleştirilebilir. Bildirilen bir özelliği değiştirmek için çözüm portalında istenen bir özelliği ayarlayın. Aşağıdaki işlemler cihazın sorumluluğundadır:
1. İstenen özellikleri IoT hub'ından düzenli olarak alma.
2. Yapılandırmasını istenen özellik değeriyle güncelleştirme.
3. Yeni değeri bildirilen özellik olarak hub’a geri gönderme.

Çözüm panosundan, [cihaz ikizini][lnk-device-twins] kullanarak bir cihaz üzerindeki özellikleri ayarlamak üzere *istenen özellikleri* kullanabilirsiniz. Genellikle, cihaz iç durumunu güncelleştirmek üzere istenen özellik değerini hub’dan okur ve değişikliği bildirilen bir özellik olarak geri bildirir.

> [!NOTE]
> Sanal cihaz kodu, IoT Hub’ına geri gönderilen bildirilen özellikleri güncelleştirmek üzere yalnızca istenen **Desired.Config.TemperatureMeanValue** ve **Desired.Config.TelemetryInterval** özelliklerini kullanır. Diğer tüm istenen özellik değişiklik istekleri, sanal cihazda yok sayılır.

### <a name="methods"></a>Yöntemler
Sanal cihazlar IoT hub aracılığıyla çözüm portalından çağrılan aşağıdaki yöntemleri ([doğrudan yöntemler][lnk-direct-methods]) işleyebilir:

| Yöntem | Açıklama |
| --- | --- |
| InitiateFirmwareUpdate |Cihazdan üretici yazılımı güncelleştirmesi yapmasını ister |
| Yeniden başlatma |Cihazdan yeniden başlatma ister |
| FactoryReset |Cihazdan fabrika sıfırlaması yapmasını ister |

Bazı yöntemler ilerleme durumunu bildirmek üzere bildirilen özellikleri kullanır. Örneğin, **InitiateFirmwareUpdate** yöntemi, güncelleştirmeyi cihaz üzerinde zaman uyumsuz olarak çalıştırma işlemini taklit eder. Yöntem cihaz üzerinde hemen döndürülürken, zaman uyumsuz görev bildirilen özellikleri kullanarak çözüm panosuna durum güncelleştirmeleri göndermeye devam eder.

### <a name="commands"></a>Komutlar 
Sanal cihazlar IoT hub aracılığıyla çözüm portalından gönderilen aşağıdaki komutları (buluttan cihaza iletiler) işleyebilir:

| Komut | Açıklama |
| --- | --- |
| PingDevice |Canlı olup olmadığını denetlemek için cihaza bir *ping* gönderir |
| StartTelemetry |Cihazın telemetri göndermesini başlatır |
| StopTelemetry |Cihazın telemetri göndermesini durdurur |
| ChangeSetPointTemp |Çevresinde rastgele verilerin oluşturulduğu ayar noktası değerini değiştirir |
| DiagnosticTelemetry |Ek bir telemetri değeri (externalTemp) göndermek için cihaz benzeticisini tetikler |
| ChangeDeviceState |Cihazla ilgili genişletilmiş durum özelliğini değiştirir ve cihazdan cihaz bilgi iletisi gönderir |

> [!NOTE]
> Bu komutlar (buluttan cihaza iletiler) ile yöntemlerin (doğrudan yöntemler) bir karşılaştırması için bkz. [Buluttan cihaza iletişim kılavuzu][lnk-c2d-guidance].
> 
> 

## <a name="iot-hub"></a>IoT Hub’ı
[IoT hub][lnk-iothub], cihazlardan buluta gönderilen verileri alır ve Azure Stream Analytics (ASA) işlerinde kullanılabilir hale getirir. ASA işinin kullandığı her akış, cihazlarınızdaki ileti akışını okumak için ayrı bir IoT Hub tüketici grubu kullanır.

IoT hub çözümde aynı zamanda şunları yapar:

- Portala bağlanmasına izin verilen tüm cihazların kimliklerini ve kimlik doğrulama anahtarlarını depolayan bir kimlik kayıt defteri tutar. Cihazları kimlik kayıt defterinden etkinleştirebilir ve devre dışı bırakabilirsiniz.
- Çözüm portalı adına cihazlarınıza komut gönderir.
- Çözüm portalı adına cihazlarınızda komut çağırır.
- Tüm kayıtlı cihazlar için cihaz ikizlerini tutar. Cihaz ikizi bir cihaz tarafından bildirilen özellik değerlerini depolar. Cihaz ikizi ayrıca cihaz bir kez daha bağlandığında alabilmesi için çözüm portalında ayarlanmış istenen özellikleri depolar.
- Birden fazla cihaza ait özellikleri ayarlamak veya birden fazla cihaz üzerinde yöntem çağırmak için işleri zamanlar.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Uzaktan izleme çözümünde [Azure Stream Analytics ][lnk-asa] (ASA), IoT hub tarafından alınan cihaz iletilerini işleme veya depolama amacıyla diğer arka uç bileşenlerine gönderir. Farklı ASA işleri, iletilerin içeriğine göre belirli işlevler gerçekleştirir.

**İş 1: Cihaz bilgileri** gelen ileti akışından cihaz bilgileri iletilerine filtre uygular ve bunları olay hub'ı uç noktasına gönderir. Cihaz, başlangıçta ve **SendDeviceInfo** komutuna yanıt olarak cihaz bilgi iletileri gönderir. Bu iş **device-info** iletilerini tanımlamak için aşağıdaki sorgu tanımını kullanır:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Bu iş daha ayrıntılı işleme için Olay Hub’ına çıktısını gönderir.

**İş 2: Kurallar** cihaz başına eşiklere karşılık gelen sıcaklık ve nem telemetrisi değerlerini değerlendirir. Eşik değerleri çözüm portalında yer alan kurallar düzenleyicisinde ayarlanır. Her cihaz/değer çifti, Akış Analizi’nin **Başvuru Verileri** olarak okuduğu blob’daki zaman damgası tarafından depolanır. İş, boş olmayan değerleri cihaz için ayarlanan eşikle karşılaştırır. ' >' koşulunu aşarsa, iş eşiğin aşıldığını belirten bir **alarm** olayı verir; cihaz, değer ve zaman damgası değerlerini de sağlar. Bu iş bir alarm tetiklemesi gereken telemetri iletilerini belirlemek üzere aşağıdaki sorgu tanımını kullanır:

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

İş daha ayrıntılı işleme için çıktısını Olay Hub’ına gönderir ve her bir uyarının ayrıntılarını, çözüm portalının uyarı bilgilerini okuyabileceği blob depolamaya kaydeder.

**ş 3: Telemetri**, gelen cihaz telemetrisi akışını iki yolla çalıştırır. İlk olarak tüm telemetri iletilerini cihazlardan uzun süreli depolama için kalıcı blob depolamaya gönderir. İkincisi beş dakikalık bir kayan pencerede ortalama, en düşük ve en yüksek nem değerlerini ölçer ve bu verileri blob depolamaya gönderir. Çözüm portalı blob depolama alanından telemetri verilerini okuyarak grafikleri doldurur. Bu iş şu sorgu tanımını kullanır:

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
Çözüm, çözümdeki cihazlarda bulunan tüm ham ve özet telemetri verilerini kalıcı hale getirmek için Azure Blob Depolama kullanır. Portal, blob depolama alanından telemetri verilerini okuyarak grafikleri doldurur. Uyarıları görüntülemek için çözüm portalı, telemetri değerleri yapılandırılmış eşik değerlerini aştığında kayıt altına alan blob depolama alanından verileri okur. Çözüm, çözüm portalında sizin ayarladığınız eşik değerlerini kaydetmek için de blob depolama alanını kullanır.

## <a name="webjobs"></a>WebJobs
WebJobs cihaz benzeticilerini barındırmaya ek olarak çözüm içinde komut yanıtlarını işleyen bir Azure WebJob içinde çalışan **Olay İşleyicisi**’ni de barındırır. Cihaz komut geçmişini (Cosmos DB veritabanında depolanır) güncelleştirmek için komut yanıtı iletilerini kullanır.

## <a name="cosmos-db"></a>Cosmos DB
Çözüm, kendisine bağlı cihazlarla ilgili bilgileri depolamak için bir Cosmos DB veritabanı kullanır. Bu bilgiler çözüm portalından cihazlara gönderilen komutların ve çözüm portalından çağrılan yöntemlerin geçmişini içerir.

## <a name="solution-portal"></a>Çözüm portalı

Çözüm portalı, önceden yapılandırılmış çözümün bir parçası olarak dağıtılan bir web uygulamasıdır. Çözüm portalındaki temel sayfalar pano ve cihaz listesidir.

### <a name="dashboard"></a>Pano
Web uygulamasındaki bu sayfa, cihazlardaki telemetri verilerini görselleştirmek için PowerBI javascript denetimlerini kullanır (bkz. [PowerBI-visuals repo](https://www.github.com/Microsoft/PowerBI-visuals)). Çözüm, blob depolama alanına telemetri verilerini yazmak için ASA telemetri işini kullanır.

### <a name="device-list"></a>Cihaz listesi
Çözüm portalındaki bu sayfadan şunları yapabilirsiniz:

* Yeni bir cihaz hazırlayın. Bu eylem, benzersiz cihaz kimliğini ayarlar ve kimlik doğrulaması anahtarını oluşturur. Hem IoT Hub kimlik kayıt defterine hem de çözüme özel Cosmos DB veritabanına cihaz hakkındaki bilgileri yazar.
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
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twins]:  ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md

