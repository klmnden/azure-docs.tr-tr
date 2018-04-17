---
title: Bir Azure zaman serisi Öngörüler ortama olayları göndermek nasıl | Microsoft Docs
description: Bu öğretici, oluşturmak, olay hub'ı yapılandırmak ve Azure zaman serisi Insights'ta gösterilmesini itme olaylara bir örnek uygulamayı çalıştırın açıklanmaktadır.
services: time-series-insights
ms.service: time-series-insights
author: venkatgct
ms.author: venkatja
manager: jhubbard
editor: MarkMcGeeAtAquent
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 04/09/2018
ms.openlocfilehash: c29b90e703a66cbbc25227f9a4307c74d82b03b5
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a>Olay hub’ı kullanarak olayları Zaman Serisi Görüşleri ortamına gönderme
Bu makalede oluşturmak ve olay hub'ı yapılandırmak nasıl açıklar ve anında iletme olaylara bir örnek uygulamayı çalıştırın. Varolan bir event hub'olaylarla JSON biçiminde varsa, Bu öğretici atlayın ve ortamınızdaki görüntülemek [zaman serisi Öngörüler](https://insights.timeseries.azure.com).

## <a name="configure-an-event-hub"></a>Olay hub’ını yapılandırma
1. Olay hub’ı oluşturmak için, Olay Hub’ı [belgelerindeki](../event-hubs/event-hubs-create.md) yönergeleri izleyin.

2. Arama **olay hub'ı** arama çubuğunda. Tıklatın **olay hub'ları** döndürülen listedeki.

3. Olay hub'ınızı adına tıklayarak seçin.

4. Altında **varlıklar** Orta yapılandırma penceresinde **olay hub'ları** yeniden.

5. Olay hub'ı yapılandırmak için adını seçin.

  ![Olay hub’ı tüketici grubunu seçme](media/send-events/consumer-group.png)

6. Altında **varlıklar**seçin **tüketici grupları**.
 
7. Özel olarak yalnızca Zaman Serisi Görüşleri olay kaynağınız tarafından kullanılan bir tüketici grubu oluşturduğunuzdan emin olun.

   > [!IMPORTANT]
   > Bu tüketici grubunun başka hiçbir hizmet (örneğin, Stream Analytics işi veya başka bir Zaman Serisi Görüşleri ortamı) tarafından kullanılmadığından emin olun. Tüketici grubu tarafından kullanılıyorsa, hizmetleri, okuma işlemi olumsuz şekilde etkilenir bu ortam ve diğer hizmetler için. Tüketici grubu olarak “$Default” kullanıyorsanız, bu diğer okuyucular tarafından olası yeniden kullanıma yol açabilir.

8. Altında **ayarları** başlığını seçin **paylaşım erişim ilkeleri**.

9. Olay hub'ına, oluşturma **MySendPolicy** csharp örnek olayları göndermek için kullanılır.

  ![Paylaşılan erişim ilkeleri’ni seçin ve Ekle düğmesine tıklayın](media/send-events/shared-access-policy.png)  

  ![Yeni paylaşılan erişim ilkesi ekleme](media/send-events/shared-access-policy-2.png)  

## <a name="add-time-series-insights-reference-data-set"></a>Zaman serisi Öngörüler başvuru veri kümesi Ekle 
Başvuru verileri içinde TSI kullanarak telemetri verilerinizi contextualizes.  Bu bağlam filtre ve toplama kolaylaştırır ve verilerinize anlamı ekler.  TSI birleştirmeler Giriş zaman veri başvuru ve bu verileri firmalarda geriye dönük katılamaz.  Bu nedenle, bir olay kaynağı veri ile eklemeden önce başvuru veri eklemek için önemlidir.  Veri konumu veya algılayıcı türü gibi olan aygıt/etiketi/algılayıcı için katılmak istiyor yararlı boyutlar dilim ve filtre kolaylaştırmak için kimliği.  

> [!IMPORTANT]
> Geçmiş verileri karşıya yüklediğinizde, yapılandırılmış bir başvuru veri kümesi olması önemlidir.

Karşıya yükleme TSI geçmiş verileri toplu, başvuru verileri yerinde olduğundan emin olun.  Unutmayın, bu olay kaynağı veri varsa TSI okuma birleştirilmiş olay kaynağından başlayacaktır.  Yerinde, başvuru verileri elde edene kadar özellikle bu olay kaynağı veri varsa bir olay kaynağı için TSI katılma beklemek kullanışlıdır. Alternatif olarak, başvuru veri kümesi yerinde olana kadar bu olay kaynağı veri göndermek için bekleyebilirsiniz.

Başvuru verileri yönetmek için web tabanlı kullanıcı arabirimi yoktur TSI Explorer'ın ve programlı bir C# API'si yok. TSI Gezgini bir görsel kullanıcı karşıya yükleme dosyalarını veya Yapıştır bileşenini varolan başvuru veri kümelerini JSON veya CSV biçiminde deneyimine sahiptir. API ile gerektiğinde özel bir uygulama oluşturabilirsiniz.

Zaman serisi Öngörüler başvuru verilerinde yönetme ile ilgili daha fazla bilgi için bkz: [başvuru veri makale](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-add-reference-data-set).

## <a name="create-time-series-insights-event-source"></a>Zaman Serisi Görüşleri olay kaynağı oluşturma
1. Bir olay kaynağı oluşturmadıysanız, olay kaynağını oluşturmak için [bu yönergeleri](time-series-insights-how-to-add-an-event-source-eventhub.md) izleyin.

2. Belirtin **deviceTimestamp** zaman damgası özellik adı olarak – bu özellik C# örnek gerçek zaman damgası olarak kullanılır. Zaman damgası özellik adı büyük/küçük harfe duyarlıdır ve olay hub’ına JSON olarak gönderildiğinde değerleri __yyyy-AA-ggTSS:dd:ss.FFFFFFFK__ biçiminde olmalıdır. Özellik olayda mevcut değilse olay hub'ı sıraya alınan zamanı kullanılır.

  ![Olay kaynağı oluşturma](media/send-events/event-source-1.png)

## <a name="sample-code-to-push-events"></a>Olayları göndermek için kullanılacak örnek kod
1. Adlı bir olay hub'ı ilke gidin **MySendPolicy**. Kopya **bağlantı dizesi** İlkesi anahtara sahip.

  ![MySendPolicy bağlantı dizesini kopyalama](media/send-events/sample-code-connection-string.png)

2. Üç cihazdan her biri için 600 olay gönderecek olan aşağıdaki kodu çalıştırın. `eventHubConnectionString` öğesini bağlantı dizenizle güncelleştirin.

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.Rdx.DataGenerator
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            var from = new DateTime(2017, 4, 20, 15, 0, 0, DateTimeKind.Utc);
            Random r = new Random();
            const int numberOfEvents = 600;

            var deviceIds = new[] { "device1", "device2", "device3" };

            var events = new List<string>();
            for (int i = 0; i < numberOfEvents; ++i)
            {
                for (int device = 0; device < deviceIds.Length; ++device)
                {
                    // Generate event and serialize as JSON object:
                    // { "deviceTimestamp": "utc timestamp", "deviceId": "guid", "value": 123.456 }
                    events.Add(
                        String.Format(
                            CultureInfo.InvariantCulture,
                            @"{{ ""deviceTimestamp"": ""{0}"", ""deviceId"": ""{1}"", ""value"": {2} }}",
                            (from + TimeSpan.FromSeconds(i * 30)).ToString("o"),
                            deviceIds[device],
                            r.NextDouble()));
                }
            }

            // Create event hub connection.
            var eventHubConnectionString = @"Endpoint=sb://...";
            var eventHubClient = EventHubClient.CreateFromConnectionString(eventHubConnectionString);

            using (var ms = new MemoryStream())
            using (var sw = new StreamWriter(ms))
            {
                // Wrap events into JSON array:
                sw.Write("[");
                for (int i = 0; i < events.Count; ++i)
                {
                    if (i > 0)
                    {
                        sw.Write(',');
                    }
                    sw.Write(events[i]);
                }
                sw.Write("]");

                sw.Flush();
                ms.Position = 0;

                // Send JSON to event hub.
                EventData eventData = new EventData(ms);
                eventHubClient.Send(eventData);
            }
        }
    }
}

```
## <a name="supported-json-shapes"></a>Desteklenen JSON şekilleri
### <a name="sample-1"></a>Örnek 1

#### <a name="input"></a>Girdi

Basit bir JSON nesnesi.

```json
{
    "id":"device1",
    "timestamp":"2016-01-08T01:08:00Z"
}
```
#### <a name="output---one-event"></a>Çıktı - tek olay

|id|timestamp|
|--------|---------------|
|cihaz1|2016-01-08T01:08:00Z|

### <a name="sample-2"></a>Örnek 2

#### <a name="input"></a>Girdi
İki JSON nesnesi içeren JSON dizisi. Her JSON nesnesi bir olaya dönüştürülür.
```json
[
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    },
    {
        "id":"device2",
        "timestamp":"2016-01-17T01:17:00Z"
    }
]
```
#### <a name="output---two-events"></a>Çıktı - iki olayları

|id|timestamp|
|--------|---------------|
|cihaz1|2016-01-08T01:08:00Z|
|cihaz2|2016-01-08T01:17:00Z|

### <a name="sample-3"></a>Örnek 3

#### <a name="input"></a>Girdi

İki JSON nesnelerini içeren iç içe geçmiş bir JSON dizisi olan bir JSON nesnesi:
```json
{
    "location":"WestUs",
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z"
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z"
        }
    ]
}

```
#### <a name="output---two-events"></a>Çıktı - iki olayları
"Konum" özelliği üzerinde her olayın kopyalanır dikkat edin.

|location|events.id|events.timestamp|
|--------|---------------|----------------------|
|WestUs|cihaz1|2016-01-08T01:08:00Z|
|WestUs|cihaz2|2016-01-08T01:17:00Z|

### <a name="sample-4"></a>Örnek 4

#### <a name="input"></a>Girdi

İki JSON nesnesi içeren iç içe bir JSON dizisi ile JSON nesnesi. Girilen bu değer, genel özelliklerin karmaşık JSON nesnesiyle ifade edilebileceğini gösterir.

```json
{
    "location":"WestUs",
    "manufacturer":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z",
            "data":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z",
            "data":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```
#### <a name="output---two-events"></a>Çıktı - iki olayları

|location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
|---|---|---|---|---|---|---|---|
|WestUs|üretici1|EastUs|cihaz1|2016-01-08T01:08:00Z|basınç|psi|108.09|
|WestUs|üretici1|EastUs|cihaz2|2016-01-08T01:17:00Z|titreşim|abs G|217.09|

### <a name="json-shaping-strategies"></a>JSON şekillendirme stratejileri
Bir başlangıç noktası ve onu ve bu sorunların azaltılması stratejileri ile ilgili sorunları ele gibi bir olay aşağıdaki örnekte kullanalım.

#### <a name="payload-1"></a>Yükü 1:
```json
[{
            "messageId": "LINE_DATA",
            "deviceId": "FXXX",
            "timestamp": 1522355650620,
            "series": [{
                        "chId": 3,
                        "value": -3750.0
                  }, {
                        "chId": 13,
                        "value": 0.58015072345733643
                  }, {
                        "chId": 11,
                        "value": 800.0
                  }, {
                        "chId": 21,
                        "value": 0.0
                  }, {
                        "chId": 14,
                        "value": -999.0
                  }, {
                        "chId": 37,
                        "value": 2.445906400680542
                  }, {
                        "chId": 39,
                        "value": 0.0
                  }, {
                        "chId": 40,
                        "value": 1.0
                  }, {
                        "chId": 1,
                        "value": 1.0172575712203979
                  }
            ],
            "EventProcessedUtcTime": "2018-03-29T20:36:21.3245900Z",
            "PartitionId": 2,
            "EventEnqueuedUtcTime": "2018-03-29T20:34:11.0830000Z",
            "IoTHub": {
                  "MessageId": "<17xxx2xx-36x0-4875-9x1x-x428x41x1x68>",
                  "CorrelationId": "<x253x5xx-7xxx-4xx3-91x4-xxx3bx2xx0x3>",
                  "ConnectionDeviceId": "AAAA-ZZ-001",
                  "ConnectionDeviceGenerationId": "<123456789012345678>",
                  "EnqueuedTime": "2018-03-29T20:34:10.7990000Z",
                  "StreamId": null
            }
      }
]
 ```

Bu olaylar dizisi için TSI yükü olarak anında varsa, her bir ölçü birimi değeri başına bir olay olarak depolanır. Bunun yapılması bir aşırı uygun olmayabilir olay oluşturabilirsiniz. Başvuru verileri içinde TSI özellikleri olarak anlamlı adları eklemek için kullanabileceğiniz dikkat edin.  Örneğin, anahtar özelliğiyle başvuru veri kümesi oluşturabilirsiniz = chId:  

chId ölçü birimi 24 altyapısı Petrol baskısı PSI 25 CALC pompa oranı bbl/dak

Zaman serisi Öngörüler başvuru verilerinde yönetme ile ilgili daha fazla bilgi için bkz: [başvuru veri makale](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-add-reference-data-set).

Başka bir sorun ilk yükü zaman damgası milisaniye cinsinden olmasıdır. TSI yalnızca ISO biçiminde zaman damgaları kabul eder. Sıraya alınan zaman damgası kullanılacak olan varsayılan zaman damgası davranışı TSI içinde bırakın bir çözümdür.

Yukarıdaki yükü alternatif olarak, başka bir örneğe bakalım.  

#### <a name="payload-2"></a>Yükü 2:
```json
{
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "STATE Engine State": 1,
      "unit": "NONE"
}, {
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "MPC_AAAA-ZZ-001",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "Well Head Px 1": -494162.8515625,
      "unit": "psi"
}, {
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "CALC Pump Rate": 0,
      "unit": "bbl/min"
}, {
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "Engine Fuel Pressure": 0,
      "unit": "psi"
}, {
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "Engine Oil Pressure": 0.58015072345733643,
      "unit": "psi"
}
```

Yükü 1 gibi TSI her ölçülen değeri her bir benzersiz olay olarak depolar.  Önemli farktır TSI okuyacaksa *zaman damgası* olarak doğru bir şekilde Burada, ISO olarak.  

Gönderilen olay sayısını azaltmak gerekiyorsa, sonra bilgileri aşağıdaki gibi gönderebilir.  

#### <a name="payload-3"></a>Yükü 3:
```json
{
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "CALC Pump Rate": 0,
      "CALC Pump Rate.unit": "bbl/min"
      "Engine Oil Pressure": 0.58015072345733643,
      "Engine Oil Pressure.unit": "psi"
      "Engine Fuel Pressure": 0,
      "Engine Fuel Pressure.unit": "psi"
}
```
Bir son öneri aşağıda verilmiştir.

#### <a name="payload-4"></a>Yükü 4:
```json
{
              "line": "Line01",
              "station": "Station 11",
              "gatewayid": "AAAA-ZZ-001",
              "deviceid": "F12XX",
              "timestamp": "2018-03-29T20:34:15.0000000Z",
              "CALC Pump Rate": {
                           "value": 0,
                           "unit": "bbl/min"
              },
              "Engine Oil Pressure": {
                           "value": 0.58015072345733643,
                           "unit": "psi"
              },
              "Engine Fuel Pressure": {
                           "value": 0,
                           "unit": "psi"
              }
}
```

Bu örnekte, JSON düzleştirme sonra çıktısı gösterilmektedir:

```json
{
      "line": "Line01",
      "station": "Station 11",,
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "CALC Pump Rate.value": 0,
      "CALC Pump Rate.unit": "bbl/min"
      "Engine Oil Pressure.value": 0.58015072345733643,
      "Engine Oil Pressure.unit": "psi"
      "Engine Fuel Pressure.value": 0,
      "Engine Fuel Pressure.unit": "psi"
}
```

Hala olay sayısı düşük tutarken her biri kendi json nesnesi içinde kanallar için farklı özellikleri tanımlamak için serbestçe. Bu düzleştirilmiş yaklaşım dikkate almak önemlidir daha fazla alan kaplar. Hangisi önce gelirse, TSI kapasite olayları hem boyutunu temel alır.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Zaman serisi Öngörüler Explorer'da ortamınızı görüntülemek](https://insights.timeseries.azure.com).
