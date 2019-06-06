---
title: Azure Time Series Insights için IOT hub olay kaynağı ekleme | Microsoft Docs
description: Bu makalede, bir IOT hub'ına zaman serisi görüşleri ortamınıza bağlı bir olay kaynağı eklemeyi açıklar.
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
ms.openlocfilehash: 955b0e36c63b181e2fe6d2f87e7b015196fceff9
ms.sourcegitcommit: ec7b0bf593645c0d1ef401a3350f162e02c7e9b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455607"
---
# <a name="add-an-iot-hub-event-source-to-your-time-series-insights-environment"></a>Zaman serisi görüşleri ortamınıza bir IOT hub olay kaynağı ekleme

Bu makalede, Azure zaman serisi görüşleri ortamınız için Azure IOT Hub'ından veri okuyan bir olay kaynağı eklemek için Azure portalını kullanmayı açıklar.

> [!NOTE]
> Azure zaman serisi öngörüleri genel kullanım için hem zaman serisi öngörüleri Önizleme ortamları için bu makaledeki yönergeleri uygulayın.

## <a name="prerequisites"></a>Önkoşullar

* Oluşturma bir [Azure zaman serisi görüşleri ortamına](time-series-insights-update-create-environment.md).
* Oluşturma bir [Azure portalı kullanarak IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* IOT hub'ı, etkin iletide olayları gönderilmekte olan olması gerekir.
* IOT hub'den kullanmak zaman serisi görüşleri ortamı için ayrılmış bir tüketici grubu oluşturun. Her zaman serisi görüşleri olay kaynağı, diğer bir tüketici ile paylaşılmaz kendi adanmış bir tüketici grubu olması gerekir. Aynı tüketici grubu olaylardan birden fazla okuyucuyu kapsayacak kullanmasına, tüm okuyucular hatalar görmeniz olasıdır. Ayrıntılar için bkz [Azure IOT Hub Geliştirici kılavuzunun](../iot-hub/iot-hub-devguide.md).

### <a name="add-a-consumer-group-to-your-iot-hub"></a>IOT hub'ınıza bir tüketici grubu Ekle

Uygulamaları, Azure IOT Hub'ından veri çekmek için tüketici gruplarını kullanın. Güvenilir bir şekilde verileri IOT hub'ından okumak için yalnızca bu zaman serisi görüşleri ortamı tarafından kullanılan ayrılmış bir tüketici grubu sağlayın.

IOT hub'ınıza yeni bir tüketici grubu eklemek için:

1. Azure portalında bulun ve IOT hub'ınızı açın.

1. Altında **ayarları**seçin **yerleşik uç noktaları**ve ardından **olayları** uç noktası.

   [![Yerleşik uç noktalar sayfasında, olayları düğmeyi seçin](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_one.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_one.png#lightbox)

1. Altında **tüketici grupları**, tüketici grubu için benzersiz bir ad girin. Yeni bir olay kaynağı oluşturduğunuzda, zaman serisi görüşleri ortamınıza bu aynı adı kullanın.

1. **Kaydet**’i seçin.

## <a name="add-a-new-event-source"></a>Yeni bir olay kaynağı ekleme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Soldaki menüden **Tüm kaynaklar**'ı seçin. Zaman Serisi Görüşleri ortamınızı seçin.

1. Altında **ortam topolojisinin**seçin **olay kaynakları**ve ardından **Ekle**.

   [![Olay kaynakları seçin ve ardından Ekle düğmesini seçin.](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_two.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_two.png#lightbox)

1. İçinde **yeni olay kaynağı** bölmesinde için **olay kaynağı adını**, bu zaman serisi görüşleri ortamı için benzersiz bir ad girin. Örneğin, **olay akışı**.

1. İçin **kaynak**seçin **IOT hub'ı**.

1. İçin bir değer seçin **içeri aktarma seçeneği**:

   * Aboneliklerinizden biri zaten bir IOT hub'ı varsa, seçin **kullanım IOT Hub'ından kullanılabilir abonelikleri**. Bu seçenek için kolay bir yaklaşımdır.
   
     [![Yeni olay kaynağı bölmesindeki seçenekleri belirleyin](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_three.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_three.png#lightbox)

    * Aşağıdaki tablo için gerekli olan özellikleri açıklar **kullanım IOT Hub'ından kullanılabilir abonelikleri** seçeneği:

       [![Yeni olay kaynağı bölmesinde - kullanım IOT Hub'ında kullanılabilir abonelikleri seçeneği ayarlamak için Özellikler](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_four.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_four.png#lightbox)

       | Özellik | Açıklama |
       | --- | --- |
       | Abonelik Kimliği | IOT hub'ının oluşturulduğu abonelik seçin.
       | IOT hub'ı adı | IOT hub'ı adını seçin.
       | IOT hub'ı ilke adı | Paylaşılan erişim ilkesi seçin. Paylaşılan erişim ilkesi, IOT hub'ı Ayarlar sekmesinde bulabilirsiniz. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağınız için paylaşılan erişim ilkesi *gerekir* sahip **hizmetini bağlama** izinleri.
       | IOT hub'ı ilke anahtarı | Anahtar önceden doldurulur.
       | IOT hub tüketici grubu | Tüketici grubu olayları IOT hub'ından okur. Olay kaynağınız için ayrılmış bir tüketici grubu kullanmanızı öneririz.
       | Olay serileştirme biçimi | Şu anda, JSON yalnızca serileştirme biçimidir. Veri okuma veya olay iletileri şu biçimde olmalıdır. |
       | Zaman damgası özellik adı | Bu değeri belirlemek için IOT hub'ına gönderilen ileti verileri ileti biçimi anlamanız gerekir. Bu değer **adı** ileti verileri, olay zaman damgası kullanmak istediğiniz belirli olay özelliğini. Değer büyük/küçük harf duyarlıdır. Boş bırakılırsa **olay sıraya alma süresi** olay kaynağı olarak olay zaman damgası kullanılır. |

    * IOT hub'ı aboneliklerinize dış olup olmadığını veya istiyorsanız Gelişmiş Seçenekler'i seçin, **sağlamak IOT hub'ı ayarlarını elle**.

      İçin gerekli özellikler aşağıdaki tabloda açıklanmıştır **sağlamak IOT hub'ı ayarlarını elle**:

       | Özellik | Açıklama |
       | --- | --- |
       | Abonelik Kimliği | IOT hub'ının oluşturulduğu abonelik.
       | Kaynak grubu | IOT hub'ının oluşturulduğu kaynak grubu adı.
       | IOT hub'ı adı | IOT hub'ınızın adıdır. IOT hub'ınızı oluşturduğunuzda, IOT hub için bir ad girdiniz.
       | IOT hub'ı ilke adı | Paylaşılan erişim ilkesi. IOT hub'ı Ayarlar sekmesinde, paylaşılan erişim ilkesi oluşturabilirsiniz. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağınız için paylaşılan erişim ilkesi *gerekir* sahip **hizmetini bağlama** izinleri.
       | IOT hub'ı ilke anahtarı | Azure Service Bus ad alanı kimlik doğrulaması yapmak için kullanılan paylaşılan erişim anahtarı. Birincil veya ikincil anahtarı buraya girin.
       | IOT hub tüketici grubu | Tüketici grubu olayları IOT hub'ından okur. Olay kaynağınız için ayrılmış bir tüketici grubu kullanmanızı öneririz.
       | Olay serileştirme biçimi | Şu anda, JSON yalnızca serileştirme biçimidir. Veri okuma veya olay iletileri şu biçimde olmalıdır. |
       | Zaman damgası özellik adı | Bu değeri belirlemek için IOT hub'ına gönderilen ileti verileri ileti biçimi anlamanız gerekir. Bu değer **adı** ileti verileri, olay zaman damgası kullanmak istediğiniz belirli olay özelliğini. Değer büyük/küçük harf duyarlıdır. Boş bırakılırsa **olay sıraya alma süresi** olay kaynağı olarak olay zaman damgası kullanılır. |

1. IOT hub'ınıza eklediğiniz adanmış Time Series Insights tüketici grubu adını ekleyin.

1. **Oluştur**’u seçin.

   [![Oluştur düğmesi](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_five.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_five.png#lightbox)

1. Olay kaynağı oluşturduktan sonra zaman serisi görüşleri ortamınıza veri akışını otomatik olarak başlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Veri erişim ilkelerini tanımlama](time-series-insights-data-access.md) verilerin güvenliğini sağlamak için.

* [Olayları gönderme](time-series-insights-send-events.md) olay kaynağına.

* Ortamınızda erişim [Time Series Insights gezgininin](https://insights.timeseries.azure.com).
