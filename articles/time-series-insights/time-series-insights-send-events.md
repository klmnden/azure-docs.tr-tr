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
ms.date: 12/03/2018
ms.openlocfilehash: c583c2211297acd83f88d23b2b8cbd9f8207927f
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52867621"
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a>Olay hub’ı kullanarak olayları Zaman Serisi Görüşleri ortamına gönderme

Bu makalede, oluşturma ve olay hub'ı yapılandırma açıklanır ve olayları göndermek için bir örnek uygulamayı çalıştırın. JSON biçiminde olaylar içeren mevcut bir olay hub'iniz varsa, bu öğreticiyi atlayabilir ve ortamınızı görüntüleyebilirsiniz [Azure Time Series Insights](./time-series-insights-update-create-environment.md).

## <a name="configure-an-event-hub"></a>Bir olay hub'ı yapılandırma

1. Olay Hub'ından olay hub'ı oluşturmak için yönergeleri [belgeleri](https://docs.microsoft.com/azure/event-hubs/).
1. Arama `Event Hub` arama çubuğunda. Tıklayın **Event Hubs** döndürülen listedeki.
1. Olay Hub'ınıza adına tıklayarak seçin.
1. Bir olay hub'ı oluşturduğunuzda, aslında bir olay hub'ı Namespace oluşturuyorsunuz.  Henüz bir olay hub'ı Namespace içinde oluşturmak varsa, varlıkları altında bir tane oluşturun.  

    ![güncelleştirildi][1]

1. Bir olay hub'ı oluşturulduktan sonra adına tıklayın.
1. Altında **varlıkları** Orta yapılandırma pencerede **Event Hubs** yeniden.
1. Bunu yapılandırmak için olay hub'ı adını seçin.

    ![Tüketici grubu][2]

1. Altında **varlıkları**seçin **tüketici grupları**.
1. Özellikle, TSI olay kaynağı tarafından kullanılan bir tüketici grubu oluşturduğunuzdan emin olun.

    > [!IMPORTANT]
    > Bu tüketici grubunun başka bir hizmet (örneğin, Stream Analytics işi veya başka bir TSI ortamı) tarafından kullanılmadığından emin olun. Tüketici grubu başka tarafından kullanılıyorsa, okuma işlemi olumsuz etkilenir bu ortam ve diğer hizmetler için. Kullanıyorsanız `$Default` tüketici grubu olarak, bu olası yeniden diğer okuyucular tarafından açabilir.

1. Altında **ayarları** başlığı seçin **paylaşım erişim ilkeleri**.
1. Olay hub'ında oluşturma **MySendPolicy** olayları göndermek için kullanılan C# örnek.

    ![Paylaşılan bir erişim =][3]

    ![paylaşılan-erişim-iki][4]

## <a name="add-time-series-insights-instances"></a>Time Series Insights örnek ekleyin

TSI güncelleştirme örnekleri bağlamsal veriler için gelen telemetri verilerini eklemek için kullanır. Sorgu kullanarak sırasında verilerin birleştirilmiş bir **zaman serisi kimliği**. **Zaman serisi kimliği** için örnek windmills projedir `Id`. Zaman serisi örnekleri hakkında daha fazla bilgi edinmek ve **zaman serisi kimlikleri**okuyun [zaman serisi modelleri](./time-series-insights-update-tsm.md).

### <a name="create-time-series-insights-event-source"></a>Zaman Serisi Görüşleri olay kaynağı oluşturma

1. Bir olay kaynağı oluşturmadıysanız, olay kaynağını oluşturmak için [bu yönergeleri](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub) izleyin.
1. Belirtin `timeSeriesId` – başvurmak [zaman serisi modelleri](./time-series-insights-update-tsm.md) hakkında daha fazla bilgi edinmek için **zaman serisi kimlikleri**.

### <a name="push-events-sample-windmills"></a>Olayları göndermek (örnek windmills)

1. Arama çubuğunda olay hub'ı arayın. Tıklayın **Event Hubs** döndürülen listedeki.
1. Olay hub'ınıza adına tıklayarak seçin.
1. Git **paylaşılan erişim ilkeleri** ardından **RootManageSharedAccessKey**. Kopyalama **bağlantı dizesi-birincil anahtar**

   ![bağlantı dizesi][5]

1. https://tsiclientsample.azurewebsites.net/windFarmGen.html kısmına gidin. Bu sanal Yeldeğirmeni cihazları çalıştırır.
1. İçinde üç adımda kopyaladığınız bağlantı dizesini yapıştırın **olay hub'ı bağlantı dizesi**.

    ![bağlantı dizesi][6]

1. Tıklayarak **başlatmak için tıklatın**. Simülatör doğrudan kullanabileceğiniz bir örnek JSON de oluşturur.
1. Olay Hub'ına dönün. Hub tarafından alınan yeni olayların görünmesi:

   ![telemetri][7]

<a id="json"></a>

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
> [Time Series Insights Gezgini içinde ortamınızı görüntülemek](https://insights.timeseries.azure.com).

<!-- Images -->
[1]: media/send-events/updated.png
[2]: media/send-events/consumer-group.png
[3]: media/send-events/shared-access-policy.png
[4]: media/send-events/shared-access-policy-2.png
[5]: media/send-events/sample-code-connection-string.png
[6]: media/send-events/updated_two.png
[7]: media/send-events/telemetry.png