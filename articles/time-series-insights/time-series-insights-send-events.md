---
title: Bir Azure zaman serisi görüşleri ortamına olayları gönderme | Microsoft Docs
description: Bu öğreticide, oluşturma ve olay hub'ı yapılandırma ve Azure zaman serisi Öngörülerinde gösterilecek bir olayları göndermek için örnek uygulamayı çalıştırma açıklanmaktadır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/09/2018
ms.openlocfilehash: 30b83c54d314934f1de170955eec22e7b2a264b8
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39629761"
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a>Olay hub’ı kullanarak olayları Zaman Serisi Görüşleri ortamına gönderme
Bu makalede, oluşturma ve olay hub'ı yapılandırma açıklanır ve olayları göndermek için bir örnek uygulamayı çalıştırın. JSON biçiminde olaylar içeren mevcut bir olay hub'iniz varsa, bu öğreticiyi atlayabilir ve ortamınızı görüntüleyebilirsiniz [Time Series Insights](https://insights.timeseries.azure.com).

## <a name="configure-an-event-hub"></a>Olay hub’ını yapılandırma
1. Olay hub’ı oluşturmak için, Olay Hub’ı [belgelerindeki](../event-hubs/event-hubs-create.md) yönergeleri izleyin.

2. Arama **olay hub'ı** arama çubuğunda. Tıklayın **Event Hubs** döndürülen listedeki.

3. Olay hub'ınıza adına tıklayarak seçin.

4. Altında **varlıkları** Orta yapılandırma pencerede **Event Hubs** yeniden.

5. Olay hub'ı yapılandırmak için adını seçin.

  ![Olay hub’ı tüketici grubunu seçme](media/send-events/consumer-group.png)

6. Altında **varlıkları**seçin **tüketici grupları**.
 
7. Özel olarak yalnızca Zaman Serisi Görüşleri olay kaynağınız tarafından kullanılan bir tüketici grubu oluşturduğunuzdan emin olun.

   > [!IMPORTANT]
   > Bu tüketici grubunun başka hiçbir hizmet (örneğin, Stream Analytics işi veya başka bir Zaman Serisi Görüşleri ortamı) tarafından kullanılmadığından emin olun. Tüketici grubu başka tarafından kullanılıyorsa, okuma işlemi olumsuz etkilenir bu ortam ve diğer hizmetler için. Tüketici grubu olarak “$Default” kullanıyorsanız, bu diğer okuyucular tarafından olası yeniden kullanıma yol açabilir.

8. Altında **ayarları** başlığı seçin **paylaşım erişim ilkeleri**.

9. Olay hub'ında oluşturma **MySendPolicy** csharp örneğinde olay göndermek için kullanılır.

  ![Paylaşılan erişim ilkeleri’ni seçin ve Ekle düğmesine tıklayın](media/send-events/shared-access-policy.png)  

  ![Yeni paylaşılan erişim ilkesi ekleme](media/send-events/shared-access-policy-2.png)  

## <a name="add-time-series-insights-reference-data-set"></a>Zaman serisi görüşleri başvuru veri kümesi Ekle 
TSI başvuru verilerini kullanarak, telemetri verilerinizi standartlı.  Bu bağlamı anlamı verilerinize ekler ve filtre ve toplama için kolaylaştırır.  TSI birleşimler giriş zamanında başvuru verileri ve geriye dönük olarak bu veriler birleştirilemiyor.  Bu nedenle, bir olay kaynağı veri ile eklemeden önce başvuru veri eklemek için önemlidir.  Veri konumu veya algılayıcı türü gibi olan bir cihaz/etiket/sensör için katılın isteyebileceğiniz yararlı boyutlar dilim ve filtre kolaylaştırmak için kimliği.  

> [!IMPORTANT]
> Geçmiş verileri karşıya yüklediğinizde, yapılandırılmış bir başvuru veri kümesi olması önemlidir.

Karşıya yükleme geçmiş verileri TSI toplu, başvuru verileri yerinde olduğundan emin olun.  Unutmayın, bu olay kaynağı veri varsa TSI okuma birleştirilmiş olay kaynağından hemen uygulanır.  Başvuru verilerinizi yerinde bulunana kadar özellikle bu olay kaynağı veri varsa olay kaynağı için TSI katılmak için beklenecek yararlıdır. Alternatif olarak, başvuru veri kümesi yerinde olana kadar bu olay kaynağına veri göndermek için bekleyebilirsiniz.

Başvuru verileri yönetmek için web tabanlı kullanıcı arabirimi yoktur TSI Gezgininde ve programlı bir C# API yoktur. TSI Gezgini bir görsel kullanıcı karşıya yükleme dosyalarını veya yapıştırma de varolan başvuru veri kümesi JSON veya CSV biçiminde deneyimi vardır. API ile gerektiğinde özel bir uygulama oluşturabilirsiniz.

Time Series Insights içinde başvuru verilerini yönetme ile ilgili daha fazla bilgi için bkz: [başvuru veri makale](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

## <a name="create-time-series-insights-event-source"></a>Zaman Serisi Görüşleri olay kaynağı oluşturma
1. Bir olay kaynağı oluşturmadıysanız, olay kaynağını oluşturmak için [bu yönergeleri](time-series-insights-how-to-add-an-event-source-eventhub.md) izleyin.

2. Belirtin **devicetimestamp değerini** zaman damgası özellik adı – bu özellik C# örneği gerçek zaman damgası olarak kullanılır. Zaman damgası özellik adı büyük/küçük harfe duyarlıdır ve olay hub’ına JSON olarak gönderildiğinde değerleri __yyyy-AA-ggTSS:dd:ss.FFFFFFFK__ biçiminde olmalıdır. Özellik olayda mevcut değilse olay hub'ı sıraya alınan zamanı kullanılır.

  ![Olay kaynağı oluşturma](media/send-events/event-source-1.png)

## <a name="sample-code-to-push-events"></a>Olayları göndermek için kullanılacak örnek kod
1. Olay hub'ı İlkesi adlı Git **MySendPolicy**. Kopyalama **bağlantı dizesi** ilke anahtarı.

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
#### <a name="output---one-event"></a>Çıkış - tek olay

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
#### <a name="output---two-events"></a>Çıkış - iki olay

|id|timestamp|
|--------|---------------|
|cihaz1|2016-01-08T01:08:00Z|
|cihaz2|2016-01-08T01:17:00Z|

### <a name="sample-3"></a>Örnek 3

#### <a name="input"></a>Girdi

İki JSON nesnesi içeren iç içe bir JSON dizisi ile JSON nesnesi:
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
#### <a name="output---two-events"></a>Çıkış - iki olay
"Konum" özelliği üzerinden her olayın kopyalanır dikkat edin.

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
#### <a name="output---two-events"></a>Çıkış - iki olay

|location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
|---|---|---|---|---|---|---|---|
|WestUs|üretici1|EastUs|cihaz1|2016-01-08T01:08:00Z|basınç|psi|108.09|
|WestUs|üretici1|EastUs|cihaz2|2016-01-08T01:17:00Z|titreşim|abs G|217.09|



## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Ortamınızı Time Series Insights Gezgini içinde görüntüleyebiliyordunuz](https://insights.timeseries.azure.com).
