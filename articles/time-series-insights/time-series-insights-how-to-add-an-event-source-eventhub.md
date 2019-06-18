---
title: Azure Time Series Insights için Event Hubs olay kaynağı ekleme | Microsoft Docs
description: Bu makalede, Azure Event Hubs için zaman serisi görüşleri ortamınıza bağlı bir olay kaynağı eklemeyi açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/01/2019
ms.custom: seodec18
ms.openlocfilehash: 8b39001481764eb955ab4535e8c6ea1752e0c012
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66475734"
---
# <a name="add-an-event-hub-event-source-to-your-time-series-insights-environment"></a>Zaman serisi görüşleri ortamınıza bir event hub olay kaynağı ekleme

Bu makalede, Azure zaman serisi görüşleri ortamınız için Azure Event Hubs'dan olay verilerini okuyan bir olay kaynağı eklemek için Azure portalını kullanmayı açıklar.

> [!NOTE]
> Bu makalede açıklanan adımlar, hem zaman serisi öngörüleri GA ve zaman serisi öngörüleri Önizleme ortamları için geçerlidir.

## <a name="prerequisites"></a>Önkoşullar

- Bölümünde anlatıldığı gibi bir zaman serisi görüşleri ortamı oluşturma [Azure zaman serisi görüşleri ortamı oluşturma](./time-series-insights-update-create-environment.md).
- Bir olay hub'ı oluşturun. Bkz: [Azure portalını kullanarak bir Event Hubs ad alanı ve olay hub'ı oluşturma](../event-hubs/event-hubs-create.md).
- Gönderilen etkin ileti olayları olay hub'ı olması gerekir. Bilgi edinmek için nasıl [Gönder olayları Azure Event Hubs için .NET Framework kullanarak](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).
- Zaman serisi görüşleri ortamına gelen tüketebileceği olay hub'ında ayrılmış bir tüketici grubu oluşturun. Her zaman serisi görüşleri olay kaynağı, diğer bir tüketici ile paylaşılmaz kendi adanmış bir tüketici grubu olması gerekir. Aynı tüketici grubu olaylardan birden fazla okuyucuyu kapsayacak kullanmasına, tüm okuyucular hatalar görmeniz olasıdır. Olay hub'ı başına 20 tüketici grubu sınırı yoktur. Ayrıntılar için bkz [Event Hubs Programlama Kılavuzu](../event-hubs/event-hubs-programming-guide.md).

### <a name="add-a-consumer-group-to-your-event-hub"></a>Olay hub'ınıza bir tüketici grubu Ekle

Uygulamaları, Azure Event Hubs'dan veri çekmek için tüketici grubu kullanır. Güvenilir bir şekilde verileri olay hub'ından okumak için yalnızca bu zaman serisi görüşleri ortamı tarafından kullanılan ayrılmış bir tüketici grubu sağlayın.

Olay hub'ında yeni bir tüketici grubu eklemek için:

1. Azure portalında bulun ve olay hub'ınızı açın.

1. Altında **varlıkları**seçin **tüketici grupları**ve ardından **tüketici grubu**.

   [![Olay hub'ı - bir tüketici grubu Ekle](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png#lightbox)

1. Üzerinde **tüketici grupları** sayfasında, yeni bir benzersiz değer için girin **adı**.  Zaman serisi görüşleri ortamına yeni bir olay kaynağı oluşturduğunuzda bu aynı adı kullanın.

1. **Oluştur**’u seçin.

## <a name="add-a-new-event-source"></a>Yeni bir olay kaynağı ekleme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Mevcut Time Series Insights ortamınızı bulun. Sol menüde **tüm kaynakları**ve ardından zaman serisi görüşleri ortamınızı seçin.

1. Altında **ortam topolojisinin**seçin **olay kaynakları**ve ardından **Ekle**.

   [![Olay kaynakları altında Ekle düğmesini seçin.](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png#lightbox)

1. İçin bir değer girin **olay kaynağı adını** olan bu zaman serisi görüşleri ortamına benzersiz gibi **olay akışı**.

1. İçin **kaynak**seçin **olay hub'ı**.

1. İçin uygun değerleri seçin **içeri aktarma seçeneği**:
   - Mevcut bir olay hub'ı aboneliklerinizden biri varsa, seçin **kullanımı olay Hub'ından kullanılabilir abonelikleri**. Bu seçenek için kolay bir yaklaşımdır.

       [![Yeni olay kaynağı bölmesinde ilk üç parametreleri için değerleri girin.](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png#lightbox)


       [![Abonelik ve olay hub'ı ayrıntıları](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png#lightbox)

     Aşağıdaki tablo için gerekli özellikleri açıklar **kullanımı olay Hub'ından kullanılabilir abonelikleri** seçeneği:

     | Özellik | Açıklama |
     | --- | --- |
     | Abonelik Kimliği | Bu olay hub'ı oluşturulduğu abonelik seçin.
     | Service Bus ad alanı | Olay hub'ı içeren Azure Service Bus ad alanı seçin.
     | Olay hub'ı adı | Olay hub'ı adını seçin.
     | Olay hub'ı ilke adı | Paylaşılan erişim ilkesi seçin. Olay hub'ında paylaşılan erişim ilkesi oluşturabilirsiniz **yapılandırma** sekmesi. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağınız için paylaşılan erişim ilkesi *gerekir* sahip **okuma** izinleri.
     | Olay hub'ı ilke anahtarı | Anahtar değeri önceden doldurulmuş.
     | Olay hub'ı tüketici grubu | Tüketici grubu olayları olay hub'ından okur. Olay kaynağınız için ayrılmış bir tüketici grubu kullanmanızı öneririz. |
     | Olay serileştirme biçimi | Şu anda, JSON yalnızca serileştirme biçimidir. Veri okunamıyor veya olay iletileri şu biçimde olmalıdır. |
     | Zaman damgası özellik adı | Bu değeri belirlemek için olay hub'ına gönderilen ileti verileri ileti biçimi anlamanız gerekir. Bu değer **adı** ileti verileri, olay zaman damgası kullanmak istediğiniz belirli olay özelliğini. Değer büyük/küçük harf duyarlıdır. Boş bırakılırsa **olay sıraya alma süresi** olay kaynağı olarak olay zaman damgası kullanılır. |

    - Olay hub'ı aboneliklerinize dış olup olmadığını veya Gelişmiş seçeneklerini seçmek istiyorsanız seçin **sağlayan olay hub'ı ayarlarını elle**.

      İçin gerekli özellikler aşağıdaki tabloda açıklanmıştır **sağlayan olay hub'ı ayarlarını elle** seçeneği:
 
      | Özellik | Açıklama |
      | --- | --- |
      | Abonelik Kimliği | Bu olay hub'ı oluşturulduğu abonelik.
      | Kaynak grubu | Bu olay hub'ı oluşturulduğu kaynak grubu.
      | Service Bus ad alanı | Service Bus ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, bir Service Bus ad alanı da oluşturmuş olursunuz.
      | Olay hub'ı adı | Olay hub'ınızın adıdır. Olay hub'ınızı oluşturduğunuzda, onu bir özel ad da vermiş oldunuz.
      | Olay hub'ı ilke adı | Paylaşılan erişim ilkesi. Olay hub'ında bir paylaşılan erişim ilkesi oluşturabilirsiniz **yapılandırma** sekmesi. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağınız için paylaşılan erişim ilkesi *gerekir* sahip **okuma** izinleri.
      | Olay hub'ı ilke anahtarı | Service Bus ad alanı kimlik doğrulaması yapmak için kullanılan paylaşılan erişim anahtarı. Birincil veya ikincil anahtarı buraya girin.
      | Olay hub'ı tüketici grubu | Tüketici grubu olayları olay hub'ından okur. Olay kaynağınız için ayrılmış bir tüketici grubu kullanmanızı öneririz.
      | Olay serileştirme biçimi | Şu anda, JSON yalnızca serileştirme biçimidir. Veri okunamıyor veya olay iletileri şu biçimde olmalıdır. |
      | Zaman damgası özellik adı | Bu değeri belirlemek için olay hub'ına gönderilen ileti verileri ileti biçimi anlamanız gerekir. Bu değer **adı** ileti verileri, olay zaman damgası kullanmak istediğiniz belirli olay özelliğini. Değer büyük/küçük harf duyarlıdır. Boş bırakılırsa **olay sıraya alma süresi** olay kaynağı olarak olay zaman damgası kullanılır. |

1. Olay hub'ınıza eklediğiniz adanmış Time Series Insights tüketici grubu adını ekleyin.

1. **Oluştur**’u seçin.

   [![Oluştur'u seçin](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png#lightbox)

   Olay kaynağı oluşturulduktan sonra zaman serisi görüşleri ortamınıza veri akışını otomatik olarak başlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Veri erişim ilkelerini tanımlama](time-series-insights-data-access.md) verilerin güvenliğini sağlamak için.

* [Olayları gönderme](time-series-insights-send-events.md) olay kaynağına.

* Ortamınızda erişim [Time Series Insights gezgininin](https://insights.timeseries.azure.com).
