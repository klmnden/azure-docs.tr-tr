---
title: Bir Azure zaman serisi görüşleri ortamına olayları gönderme | Microsoft Docs
description: Bir olay hub'ı yapılandırmak ve bir Azure zaman serisi Öngörülerinde görüntüleyebileceğiniz olayları göndermek için örnek uygulamayı çalıştırma hakkında bilgi edinin.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/06/2019
ms.custom: seodec18
ms.openlocfilehash: 48524020940149f6c67f4859f23c03eea140454b
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991489"
---
# <a name="send-events-to-a-time-series-insights-environment-by-using-an-event-hub"></a>Bir olay hub'ı kullanarak zaman serisi görüşleri ortamına olayları gönderme

Bu makalede, oluşturma ve Azure olay hub'ları, bir olay hub'ı yapılandırma açıklanmaktadır. Ayrıca, Event Hubs'dan olayları göndermek için örnek bir uygulama için Azure Time Series Insights çalışmasına nasıl açıklar. JSON biçiminde olaylar içeren mevcut bir olay hub'iniz varsa, bu öğreticiyi atlayabilir ve ortamınızı görüntüleyebilirsiniz [Azure Time Series Insights](./time-series-insights-update-create-environment.md).

## <a name="configure-an-event-hub"></a>Olay hub’ını yapılandırma

1. Bir olay hub'ı oluşturmayı öğrenmek için bkz: [Event Hubs belgeleri](https://docs.microsoft.com/azure/event-hubs/).
1. Arama kutusuna arama **Event Hubs**. Döndürülen listeden seçin **Event Hubs**.
1. Olay hub'ınızı seçin.
1. Bir olay hub'ı oluşturduğunuzda, bir olay hub'ı ad alanını oluşturuyorsunuz. Henüz bir olay hub'ı ad alanı içinde menüde altında oluşturmadıysanız **varlıkları**, bir olay hub'ı oluşturun.  

    [![Olay hub'ları listesi](media/send-events/updated.png)](media/send-events/updated.png#lightbox)

1. Bir olay hub'ı oluşturduğunuzda, olay hub'ları listesinde seçin.
1. Menüsünde altında **varlıkları**seçin **Event Hubs**.
1. Olay hub'ı yapılandırmak için adını seçin.
1. Altında **varlıkları**seçin **tüketici grupları**ve ardından **tüketici grubu**.

    [![Bir tüketici grubu oluşturun](media/send-events/consumer-group.png)](media/send-events/consumer-group.png#lightbox)

1. Özel zaman serisi görüşleri olay kaynağınız tarafından kullanılan bir tüketici grubu oluşturduğunuzdan emin olun.

    > [!IMPORTANT]
    > Bu tüketici grubunun, Azure Stream Analytics işi veya başka bir zaman serisi görüşleri ortamına gibi diğer tüm hizmet tarafından kullanılmadığından emin olun. Tüketici grubu tarafından kullanılıyorsa, bu ortam için ve diğer hizmetler için Hizmetleri, okuma işlemleri olumsuz etkilenir. Kullanırsanız **$Default** tüketici grubu diğer okuyucular potansiyel olarak, bir tüketici grubu yeniden kullanabilir.

1. Menüsünde altında **ayarları**seçin **paylaşılan erişim ilkeleri**ve ardından **Ekle**.

    [![Paylaşılan erişim ilkeleri'ni seçin ve ardından Ekle düğmesini seçin.](media/send-events/shared-access-policy.png)](media/send-events/shared-access-policy.png#lightbox)

1. İçinde **yeni paylaşılan erişim ilkesi ekleme** bölmesinde adlı bir paylaşılan erişim oluşturma **MySendPolicy**. Bu paylaşılan erişim ilkesi olayları göndermek için kullandığınız C# bu makaledeki örnekler.

    [![İlke adı kutusuna MySendPolicy girin](media/send-events/shared-access-policy-2.png)](media/send-events/shared-access-policy-2.png#lightbox)

1. Altında **talep**seçin **Gönder** onay kutusu.

## <a name="add-a-time-series-insights-instance"></a>Time Series Insights örneği ekleme

Time Series Insights güncelleştirme örnekleri bağlamsal veriler için gelen telemetri verilerini eklemek için kullanır. Verileri kullanarak sorgu zamanında birleştirilmiş bir **zaman serisi kimliği**. **Zaman serisi kimliği** örnek windmills için bu makalenin sonraki bölümlerinde kullandığımız projedir `id`. Zaman serisi görüşleri örnekleri hakkında daha fazla bilgi edinmek ve **zaman serisi kimliği**, bkz: [zaman serisi modelleri](./time-series-insights-update-tsm.md).

### <a name="create-a-time-series-insights-event-source"></a>Zaman serisi görüşleri olay kaynağı oluşturma

1. Olay kaynağı oluşturmadıysanız, adımlarını tamamlamanız [olay kaynağı oluşturma](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub).

1. İçin bir değer ayarlamanız `timeSeriesId`. Hakkında daha fazla bilgi edinmek için **zaman serisi kimliği**, bkz: [zaman serisi modelleri](./time-series-insights-update-tsm.md).

### <a name="push-events"></a>Olayları göndermek (windmills örneği)

1. Arama çubuğunda arama **Event Hubs**. Döndürülen listeden seçin **Event Hubs**.

1. Olay hub'ınızı seçin.

1. Git **paylaşılan erişim ilkeleri** > **RootManageSharedAccessKey**. Değeri kopyalamak **bağlantı dizesi-birincil anahtar**.

    [![Birincil anahtar bağlantı dizesi değerini kopyalayın](media/send-events/sample-code-connection-string.png)](media/send-events/sample-code-connection-string.png#lightbox)

1. https://tsiclientsample.azurewebsites.net/windFarmGen.html kısmına gidin. URL sanal Yeldeğirmeni cihazları çalıştırır.
1. İçinde **olay hub'ı bağlantı dizesi** kutusuna Web sayfası, içinde kopyaladığınız bağlantı dizesini yapıştırın [olayları gönderme](#push-events).
  
    [![Olay hub'ı bağlantı dizesi kutusunda birincil anahtar bağlantı dizesini yapıştırın](media/send-events/updated_two.png)](media/send-events/updated_two.png#lightbox)

1. Seçin **başlatmak için tıklatın**. Simülatör doğrudan kullanabileceğiniz JSON örneği oluşturur.

1. Azure portalında event hub'ınıza geri dönün. Üzerinde **genel bakış** sayfa, olay hub'ı tarafından alınan yeni olayları görürsünüz.

    [![Ölçümleri olay hub'ı gösteren bir olay hub'ı genel bakış sayfası](media/send-events/telemetry.png)](media/send-events/telemetry.png#lightbox)

## <a name="json"></a>Desteklenen JSON şekilleri

### <a name="example-one"></a>Bir örnek

* **Giriş**: Basit bir JSON nesnesi.

    ```JSON
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    }
    ```

* **Çıkış**: Bir olay.

    |kimlik|timestamp|
    |--------|---------------|
    |cihaz1|2016-01-08T01:08:00Z|

### <a name="example-two"></a>İki örnek

* **Giriş**: İki JSON nesnesi içeren JSON dizisi. Her bir JSON nesnesi bir olaya dönüştürülür.

    ```JSON
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

* **Çıkış**: İki olay.

    |kimlik|timestamp|
    |--------|---------------|
    |cihaz1|2016-01-08T01:08:00Z|
    |cihaz2|2016-01-08T01:17:00Z|

### <a name="example-three"></a>Örnek üç

* **Giriş**: İki JSON nesnesi içeren iç içe bir JSON dizisi ile JSON nesnesi.

    ```JSON
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

* **Çıkış**: İki olay. Özellik **konumu** üzerinden her olay için kopyalanır.

    |location|events.id|events.timestamp|
    |--------|---------------|----------------------|
    |WestUs|cihaz1|2016-01-08T01:08:00Z|
    |WestUs|cihaz2|2016-01-08T01:17:00Z|

### <a name="example-four"></a>Dört örnek

* **Giriş**: İki JSON nesnesi içeren iç içe bir JSON dizisi ile JSON nesnesi. Bu giriş, genel özellikleri karmaşık bir JSON nesnesi tarafından temsil edilebilir olduğunu gösterir.

    ```JSON
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

* **Çıkış**: İki olay.

    |konum|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
    |---|---|---|---|---|---|---|---|
    |WestUs|üretici1|EastUs|cihaz1|2016-01-08T01:08:00Z|basınç|psi|108.09|
    |WestUs|üretici1|EastUs|cihaz2|2016-01-08T01:17:00Z|titreşim|abs G|217.09|

## <a name="next-steps"></a>Sonraki adımlar

- [Ortamınızı görüntülemek](https://insights.timeseries.azure.com) Time Series Insights Gezgininde.
