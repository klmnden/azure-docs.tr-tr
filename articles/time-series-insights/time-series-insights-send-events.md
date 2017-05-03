---
title: "Azure Zaman Serisi Görüşleri ortamına olayları gönderme | Microsoft Docs"
description: "Bu öğretici, olayların Zaman Serisi Görüşleri ortamına nasıl gönderildiği konusunu kapsar"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: venkatja
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 92e3e64f235e165a6a1772b6e1724789f3ec3049
ms.lasthandoff: 04/25/2017

---
# <a name="send-events-to-a-time-series-insights-environment-via-event-hub"></a>Olay hub’ı üzerinden olayları Zaman Serisi Görüşleri ortamına gönderme

Bu öğreticide, olay hub’ının nasıl oluşturulduğu ve yapılandırıldığı, ayrıca olayları göndermek için örnek bir uygulamanın nasıl çalıştırıldığı açıklanır. Zaten JSON biçiminde olaylar içeren mevcut bir olay hub’ınız varsa, bu öğreticiyi atlayabilir ve [zaman serisi gezgininde](https://insights.timeseries.azure.com) ortamınızı görüntüleyebilirsiniz.

## <a name="configure-an-event-hub"></a>Olay hub’ını yapılandırma
1. Olay hub’ı oluşturmak için, Olay Hub’ı [belgelerindeki](https://docs.microsoft.com/azure/event-hubs/event-hubs-create) yönergeleri izleyin.

2. Özel olarak yalnızca Zaman Serisi Görüşleri olay kaynağınız tarafından kullanılan bir tüketici grubu oluşturduğunuzdan emin olun.

  > [!IMPORTANT]
  > Bu tüketici grubunun başka hiçbir hizmet (örneğin, Stream Analytics işi veya başka bir Zaman Serisi Görüşleri ortamı) tarafından kullanılmadığından emin olun. Tüketici grubu başka hizmetler tarafından kullanılıyorsa, bu ortam ve diğer hizmetler için okuma işlemi olumsuz etkilenir. Tüketici grubu olarak “$Default” kullanıyorsanız, bu diğer okuyucular tarafından olası yeniden kullanıma yol açabilir.

  ![Olay hub’ı tüketici grubunu seçme](media/send-events/consumer-group.png)

3. Olay hub’ında, aşağıdaki örnekte olayları göndermek için kullanılan “MySendPolicy” ilkesini oluşturun.

  ![Paylaşılan erişim ilkeleri’ni seçin ve Ekle düğmesine tıklayın](media/send-events/shared-access-policy.png)  

  ![Yeni paylaşılan erişim ilkesi ekleme](media/send-events/shared-access-policy-2.png)  

## <a name="create-time-series-insights-event-source"></a>Zaman Serisi Görüşleri olay kaynağı oluşturma
1. Olay kaynağı oluşturmadıysanız, olay kaynağını oluşturmak için [burada](time-series-insights-add-event-source.md) belirtilen yönergeleri izleyin.

2. Zaman damgası özellik adı olarak “deviceTimestamp” değerini belirtin; bu özellik aşağıdaki örnekte gerçek zaman damgası olarak kullanılmıştır. Zaman damgası özellik adı büyük/küçük harfe duyarlıdır ve olay hub’ına JSON olarak gönderildiğinde değerleri __yyyy-AA-ggTSS:dd:ss.FFFFFFFK__ biçiminde olmalıdır. Olayda özellik yoksa, olayın olay hub’ında sıraya alındığı saat kullanılır.

  ![Olay kaynağı oluşturma](media/send-events/event-source-1.png)

## <a name="run-sample-code-to-push-events"></a>Olayları göndermek için örnek kodu çalıştırma
1. “MySendPolicy” olay hub’ı ilkesine gidin ve ilke anahtarıyla bağlantı dizesini kopyalayın.

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
    "deviceId":"device1",
    "deviceTimestamp":"2016-01-08T01:08:00Z"
}
```
#### <a name="output---1-event"></a>Çıkış - 1 olay

|deviceId|deviceTimestamp|
|--------|---------------|
|cihaz1|2016-01-08T01:08:00Z|

### <a name="sample-2"></a>Örnek 2

#### <a name="input"></a>Girdi
İki JSON nesnesi içeren JSON dizisi. Her JSON nesnesi bir olaya dönüştürülür.
```json
[
    {
        "deviceId":"device1",
        "deviceTimestamp":"2016-01-08T01:08:00Z"
    },
    {
        "deviceId":"device2",
        "deviceTimestamp":"2016-01-17T01:17:00Z"
    }
]
```
#### <a name="output---2-events"></a>Çıkış - 2 Olay

|deviceId|deviceTimestamp|
|--------|---------------|
|cihaz1|2016-01-08T01:08:00Z|
|cihaz2|2016-01-08T01:17:00Z|

### <a name="sample-3"></a>Örnek 3

#### <a name="input"></a>Girdi

İki JSON nesnesi içeren iç içe bir JSON dizisi ile JSON nesnesi.
```json
{
    "location":"WestUs",
    "events":[
        {
            "deviceId":"device1",
            "deviceTimestamp":"2016-01-08T01:08:00Z"
        },
        {
            "deviceId":"device2",
            "deviceTimestamp":"2016-01-17T01:17:00Z"
        }
    ]
}

```
#### <a name="output---2-events"></a>Çıkış - 2 Olay
"location" özelliğinin her olaya kopyalandığına dikkat edin.

|location|events.deviceId|events.deviceTimestamp|
|--------|---------------|----------------------|
|WestUs|cihaz1|2016-01-08T01:08:00Z|
|WestUs|cihaz2|2016-01-08T01:17:00Z|

### <a name="sample-4"></a>Örnek 4

#### <a name="input"></a>Girdi

```json
{
    "location":"WestUs",
    "manufacturerInfo":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "deviceId":"device1",
            "deviceTimestamp":"2016-01-08T01:08:00Z",
            "deviceData":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "deviceId":"device2",
            "deviceTimestamp":"2016-01-17T01:17:00Z",
            "deviceData":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```
#### <a name="output---2-events"></a>Çıkış - 2 Olay

|location|manufacturerInfo.name|manufacturerInfo.location|events.deviceId|events.deviceTimestamp|events.deviceData.type|events.deviceData.units|events.deviceData.value|
|---|---|---|---|---|---|---|---|
|WestUs|üretici1|EastUs|cihaz1|2016-01-08T01:08:00Z|basınç|psi|108.09|
|WestUs|üretici1|EastUs|cihaz1|2016-01-08T01:17:00Z|titreşim|abs G|217.09|

## <a name="next-steps"></a>Sonraki adımlar

* [Zaman Serisi Görüşleri Portalı](https://insights.timeseries.azure.com)’nda ortamınızı görüntüleme

