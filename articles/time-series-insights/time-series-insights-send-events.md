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
ms.date: 11/26/2018
ms.openlocfilehash: 64fb9f72cfd7edef18d56f15cbcce726dd33b50d
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847278"
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a>Olay hub’ı kullanarak olayları Zaman Serisi Görüşleri ortamına gönderme

Bu makalede, oluşturma ve olay hub'ı yapılandırma açıklanır ve olayları göndermek için bir örnek uygulamayı çalıştırın. JSON biçiminde olaylar içeren mevcut bir olay hub'iniz varsa, bu öğreticiyi atlayabilir ve ortamınızı görüntüleyebilirsiniz [Azure Time Series Insights](./time-series-insights-update-create-environment.md).

## <a name="configure-an-event-hub"></a>Bir olay hub'ı yapılandırma

1. Bir olay hub'ı oluşturmak için olay Hub'ından yönergeleri [belgeleri](https://docs.microsoft.com/azure/event-hubs/).
1. Olay hub'ı için arama çubuğuna arayın. Tıklayın **Event Hubs** döndürülen listedeki.
1. Olay Hub'ınıza adına tıklayarak seçin.
1. Altında **varlıkları** Orta yapılandırma pencerede **Event Hubs** yeniden.
1. Bunu yapılandırmak için olay hub'ı adını seçin.

    ![Tüketici grubu][1]

1. Altında **varlıkları**seçin **tüketici grupları**.
1. Özellikle, TSI olay kaynağı tarafından kullanılan bir tüketici grubu oluşturduğunuzdan emin olun.

> [!IMPORTANT]
> Bu tüketici grubunun başka bir hizmet (örneğin, Stream Analytics işi veya başka bir TSI ortamı) tarafından kullanılmadığından emin olun. Tüketici grubu başka tarafından kullanılıyorsa, okuma işlemi olumsuz etkilenir bu ortam ve diğer hizmetler için. Kullanıyorsanız `$Default` tüketici grubu olarak, bu olası yeniden diğer okuyucular tarafından açabilir.

1. Altında **ayarları** başlığı seçin **paylaşım erişim ilkeleri**.
1. Olay hub'ında oluşturma **MySendPolicy** olayları göndermek için kullanılan C# örnek.

    ![Paylaşılan bir erişim =][2]

    ![paylaşılan-erişim-iki][3]

## <a name="add-time-series-insights-instances"></a>Time Series Insights örnek ekleyin

TSI güncelleştirme örnekleri bağlamsal veriler için gelen telemetri verilerini eklemek için kullanır. Sorgu kullanarak sırasında verilerin birleştirilmiş bir **zaman serisi kimliği**. **Zaman serisi kimliği** için örnek windmills projedir `Id`. Zaman serisi örnekleri hakkında daha fazla bilgi edinmek ve **zaman serisi kimlikleri**okuyun [zaman serisi modelleri](./time-series-insights-update-tsm.md).

### <a name="create-time-series-insights-event-source"></a>Zaman Serisi Görüşleri olay kaynağı oluşturma

1. Olay kaynağı oluşturmadıysanız, olay kaynağını oluşturmak için bu yönergeleri izleyin.
1. Belirtin `timeSeriesId` – başvurmak [zaman serisi modelleri](./time-series-insights-update-tsm.md) hakkında daha fazla bilgi edinmek için **zaman serisi kimlikleri**.

### <a name="push-events-sample-windmills"></a>Olayları göndermek (örnek windmills)

1. Arama çubuğunda olay hub'ı arayın. Tıklayın **Event Hubs** döndürülen listedeki.
1. Olay hub'ınıza adına tıklayarak seçin.
1. Git **paylaşılan erişim ilkeleri** ardından **RootManageSharedAccessKey**. Kopyalama **bağlantı dizesi-birincil anahtar**

   ![bağlantı dizesi][4]

1. https://tsiclientsample.azurewebsites.net/windFarmGen.html kısmına gidin. Bu sanal Yeldeğirmeni cihazları çalıştırır.
1. İçinde üç adımda kopyaladığınız bağlantı dizesini yapıştırın **olay hub'ı bağlantı dizesi**
1. Tıklayarak **başlatmak için tıklatın**
1. Olay Hub'ına dönün. Hub tarafından alınan yeni olayların görünmesi:

   ![telemetri][5]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Time Series Insights Gezgini içinde ortamınızı görüntülemek](https://insights.timeseries.azure.com).

<!-- Images -->
[1]: media/send-events/consumer-group.png
[2]: media/send-events/shared-access-policy.png
[3]: media/send-events/shared-access-policy-2.png
[4]: media/send-events/sample-code-connection-string.png
[5]: media/send-events/telemetry.png