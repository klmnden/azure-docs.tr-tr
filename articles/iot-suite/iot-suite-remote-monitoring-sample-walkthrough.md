<properties
 pageTitle="Önceden yapılandırılmış Uzaktan İzleme çözümünde gezinme | Microsoft Azure"
 description="Azure IOT önceden yapılandırılmış çözümü uzaktan izlemenin ve mimarisinin açıklaması."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="03/02/2016"
 ms.author="stevehob"/>

# Önceden yapılandırılmış uzaktan izleme çözümünde gezinme

## Giriş

IoT Paketi uzaktan izleme çözümü, uzak konumlarda yer alan birden çok makineyi çalıştıran iş senaryosu için temel uçtan uca izleme çözümüdür. Bu çözüm, önemli Azure IoT Paketi hizmetlerini iş senaryosunun genel uygulamasını sağlamak amacıyla bir araya getirir; kendi özel iş gereksinimlerini karşılaması amacıyla bu türden IoT çözümünü uygulamayı planlayan müşteriler için bir başlangıç noktasıdır.

## Mantıksal mimari

Aşağıdaki diyagram önceden yapılandırılmış çözümün mantıksal bileşenlerinin ana hatların vermektedir:

![](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


### Sanal cihazlar

Önceden yapılandırılmış çözümde, sanal cihaz bir soğutma cihazını temsil eder (örneğin, bir yapının klimaları veya bir tesisin havalandırma birimi). Her sanal cihaz IoT Hub'ına şu telemetri iletilerini gönderir:


| İleti  | Açıklama |
|----------|-------------|
| Başlangıç  | Cihaz başlatıldığında, cihaz kimliği, cihaz meta veriler, cihazın desteklediği komutların listesi, cihazın geçerli yapılandırması gibi cihazın kendi hakkında bilgileri kapsayan bir **device-info** iletisi gönderir. |


Sanal cihazların meta veri olarak gönderdiği cihaz özellikleri:

| Özellik               |  Amaç |
|------------------------|--------- |
| Cihaz Kimliği              | Çözümde cihaz oluşturulduğunda sağlanan veya atanan kimlik. |
| Üretici           | Cihaz üreticisi |
| Model Numarası           | Cihazın model numarası |
| Seri Numarası          | Cihazın seri numarası |
| Üretici yazılımı               | Geçerli cihazdaki üretici yazılımı sürümü |
| Platform               | Cihazın platform mimarisi |
| İşlemci              | Cihazı çalıştıran işlemci |
| Yüklü RAM          | Cihazda yüklü RAM miktarı |
| Hub Etkin Durumu      | Cihazın IoT Hub durumu özelliği |
| Oluşturma Zamanı           | Çözümde cihazın oluşturulduğu zaman |
| Güncelleştirme Zamanı           | Cihaz özelliklerinin en son güncelleştirildiği zaman |
| Enlem               | Cihazın enlem konumu |
| Boylam              | Cihazın boylam konumu |

Benzetici, örnek değerlerle sanal cihazlarda bu özelliklerin çekirdeğini oluşturur.  Benzeticinin sanal cihazı her başlatışında, cihaz IoT Hub'ına önceden tanımlanmış meta verileri gönderir. Cihaz portalında yapılmış meta veri güncelleştirmelerin üzerine yazılacağını unutmayın.


Sanal cihazlar IoT hub'ından gönderilen şu komutları işleyebilir:

| Komut                | Açıklama                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | Canlı olup olmadığını denetlemek için cihaza bir _ping_ gönderir   |
| StartTelemetry         | Cihazın telemetri göndermesini başlatır                 |
| StopTelemetry          | Cihazın telemetri göndermesini durdurur             |
| ChangeSetPointTemp     | Çevresinde rastgele verilerin oluşturulduğu ayar noktası değerini değiştirir |
| DiagnosticTelemetry    | Ek bir telemetri değeri (externalTemp) göndermek için cihaz benzeticisini tetikler |
| ChangeDeviceState      | Cihazla ilgili genişletilmiş durum özelliğini değiştirir ve cihazdan cihaz bilgi iletisi gönderir |


Cihaz komut bildirimi IoT hub’ı aracılığıyla sağlanır.


### Azure Stream Analytics işleri


**İş 1: Cihaz bilgileri** gelen ileti akışından cihaz bilgileri iletilerine filtre uygular ve bunları olay hub'ı uç noktasına gönderir. Cihaz, cihaz bilgileri iletilerini başlangıçta ve **SendDeviceInfo** komutuna yanıt olarak gönderir. Bu iş şu sorgu tanımını kullanır:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

**İş 2: Kurallar** cihaz başına eşiklere karşılık gelen sıcaklık ve nem telemetrisi değerlerini değerlendirir. Eşik değerleri çözümde yer alan kurallar düzenleyicisinde ayarlanır. Her cihaz/değer çifti, Stream Analytics içinde **Başvuru Verileri** olarak okunan blob’daki zaman damgası tarafından depolanır. İş, boş olmayan değerleri cihaz için ayarlanan eşikle karşılaştırır. ' >' koşulunu aşarsa, iş eşiğin aşıldığını belirten bir **alarm** olayı verir; cihaz, değer ve zaman damgası değerlerini de sağlar. Bu iş şu sorgu tanımını kullanır:

```
WITH AlarmsData AS 
(
SELECT
     Stream.DeviceID,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.DeviceID = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.DeviceID,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.DeviceID = Ref.DeviceID
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

**ş 3: Telemetri**, gelen cihaz telemetrisi akışını iki yolla çalıştırır. Önce tüm telemetri iletilerini cihazdan kalıcı blob depolamaya gönderir. Sonra, beş dakikalık kayan pencere üzerinde ortalama, en alt ve en üst nem değerlerini ölçer. Bu veriler blob depolamaya da gönderilir. Bu iş şu sorgu tanımını kullanır:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM 
      [IoTHubStream] 
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    *
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    DeviceId,
    AVG (Humidity) AS [AverageHumidity], 
    MIN(Humidity) AS [MinimumHumidity], 
    MAX(Humidity) AS [MaxHumidity], 
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM
    [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    DeviceId, 
    SlidingWindow (mi, 5)
```

### Olay İşlemcisi

**Olay İşlemcisi** cihaz bilgileri iletilerini ve komut yanıtlarını işler. Şunları kullanır:

- Cihaz kayıt defterini (DocumentDB veritabanında depolanır) güncelleştirmek için geçerli cihaz bilgilerinin bulunduğu cihaz bilgileri iletileri.
- Cihaz komut geçmişini (DocumentDB veritabanında depolanır) güncelleştirmek için komut yanıtı iletileri.

## Haydi dolaşmaya başlayalım

Bu bölümde, çözüm bileşenlerinde gezineceksiniz; burada hedeflenen kullanım örneğini açıklanacak ve örnekler verilecektir.

### Uzaktan İzleme Panosu
Web uygulamasındaki bu sayfa, blob depolamadaki Stream Analytics işlerine ait çıktı verilerini görselleştirmek için PowerBI javascript denetimlerini kullanır (bkz. [PowerBI-visuals repo](https://www.github.com/Microsoft/PowerBI-visuals)).


### Cihaz Yönetim Portalı

Bu web uygulamasıyla şunları yapabilirsiniz:

- Benzersiz cihaz kimliği ayarlayan ve kimlik doğrulaması anahtarı oluşturan yeni cihazı hazırlama.
- Mevcut özellikleri görüntülemeyi ve yeni özelliklerle güncellemeyi de kapsayan cihaz özelliklerini yönetme.
- Cihaza komut gönderme.
- Cihaz için komut geçmişini görüntüleme.

### Bulut çözümünün davranışını gözlemleme
Hazırlanan kaynaklarınızı [Azure portalı](https://portal.azure.com)’na giderek ve belirttiğiniz çözüm adına sahip kaynak grubunda gezinerek görüntüleyebilirsiniz.

![](media/iot-suite-remote-monitoring-sample-walkthrough/azureportal_01.png)

Örneği ilk kez çalıştırdığınızda, önceden yapılandırılmış dört sanal cihaz bulunur.

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_01.png)

Cihaz Yönetim portalını yeni bir sanal cihaz eklemek için kullanabilirsiniz:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_02.png)

Başlangıçta, Cihaz Yönetim portalında yeni cihazın **Beklemede** durumundadır:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_03.png)

Uygulama sanal makineyi dağıtmayı bitirdiğinde, cihaz durumunun aşağıdaki ekran görüntüsünde gösterildiği gibi Cihaz Yönetim portalında **Çalışıyor** olarak değiştiğini göreceksiniz. **DeviceInfo** Stream Analytics işi cihaz durumu bilgilerini cihazdan Cihaz Yönetim portalına gönderir.

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_04.png)

Çözüm portalını kullanarak **ChangeSetPointTemp** gibi komutları cihaza gönderebilirsiniz:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_05.png)

Cihaz komutu sorunsuz yürüttüğünü raporladığında, durum **Başarılı** olarak değişir:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_06.png)

Çözüm portalını kullanarak model numarası gibi belirli özelliklere sahip cihazları arayabilirsiniz:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_07.png)

Bir Cihazı devre dışı bırakabilir, devre dışı kaldıktan sonra da kaldırabilirsiniz:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_08.png)


## Sonraki adımlar

Aşağıdaki TechNet blog gönderileri önceden yapılandırılmış uzaktan izleme çözümü hakkında ek ayrıntılı bilgi sağlar:

- [IoT Paketi - Başlık Altında - Uzaktan İzleme](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [IoT Paketi - Uzaktan İzleme - Canlı ve Sanal Cihaz Ekleme](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)


<!--HONumber=Jun16_HO2-->


