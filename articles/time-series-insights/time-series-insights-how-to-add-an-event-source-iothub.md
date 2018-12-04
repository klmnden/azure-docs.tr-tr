---
title: Azure Time Series Insights için IOT Hub olay kaynağı ekleme | Microsoft Docs
description: Bu makalede bir IOT hub'ına zaman serisi görüşleri ortamınıza bağlı bir olay kaynağı ekleme
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: 3168fb6cf6e9b08fe4d2de921179129db241d153
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847176"
---
# <a name="how-to-add-an-iot-hub-event-source-to-time-series-insights-environment"></a>Zaman serisi görüşleri ortamına bir IOT Hub olay kaynağı ekleme

Bu makalede, Azure zaman serisi öngörüleri (TSI) ortamınıza bir IOT Hub'ından veri okuyan bir olay kaynağı eklemek için Azure portalını kullanmayı açıklar.

> [!NOTE]
> Bu yönergeler, TSI GA hem TSI Önizleme ortamları için geçerlidir.

## <a name="prerequisites"></a>Önkoşullar

* TSI bir ortam oluşturun. Daha fazla bilgi için [Azure TSI ortam oluşturma](time-series-insights-update-create-environment.md).
* Bir IoT Hub oluşturma. IOT hub'ları hakkında daha fazla bilgi için bkz. [Azure portalını kullanarak IOT Hub oluşturma](../iot-hub/iot-hub-create-through-portal.md).
* Etkin ileti olayları gönderilmekte olan IOT hub'ı olmalıdır.
* Gelen kullanmak için IOT hub'TSI ortam için ayrılmış bir tüketici grubu oluşturun. Her TSI olay kaynağı ile herhangi bir tüketiciye paylaşılmıyor kendi adanmış bir tüketici grubu olmalıdır. Aynı tüketici grubu olaylardan birden fazla okuyucuyu kapsayacak kullanmasına, tüm okuyucular hatalar görmeniz olasıdır. Ayrıntılar için bkz [IOT Hub Geliştirici kılavuzunun](../iot-hub/iot-hub-devguide.md).

### <a name="add-a-consumer-group-to-your-iot-hub"></a>IOT Hub'ınıza bir tüketici grubu Ekle

Tüketici grupları, Azure IOT hub'ları veri çekmek için uygulamalar tarafından kullanılır. Güvenilir bir şekilde verileri IOT Hub'ından okumak için bu TSI ortamı yalnızca tarafından kullanılmak üzere ayrılmış bir tüketici grubu, sağlar.

IOT Hub'ınıza yeni bir tüketici grubu eklemek için aşağıdaki adımları izleyin:

1. Azure portalında bulun ve IOT Hub'ınızı açın.
1. Altında **ayarları** başlığı seçin **yerleşik uç noktaları**.

   ![Bir IOT Hub][1]

1. Seçin **olayları** uç noktasını ve **özellikleri** sayfası açılır.

1. Altında **tüketici grupları** başlığı tüketici grubu için yeni ve benzersiz bir ad sağlayın. Bu aynı adı, yeni bir olay kaynağı oluştururken TSI ortamında kullanın.

1. Seçin **Kaydet** yeni tüketici grubunu kaydetmek için.

## <a name="add-a-new-event-source"></a>Yeni bir olay kaynağı ekleme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. TSI ortamınız bulun. Tıklayın **tüm kaynakları** Azure portalının sol tarafındaki menüde. TSI ortamınızı seçin.

1. Altında **ortam topolojisinin** başlığını tıklatın **olay kaynakları**.

   ![IOT hub'ı iki][2]

1. **+ Ekle**'ye tıklayın.

1. Sağlayan bir **olay kaynağı adını** bu TSI ortama benzersiz gibi **olay akışı**.

   ![IOT Hub üç][3]

1. Seçin **kaynak** olarak **IOT hub'ı**.

1. Uygun seçin **içeri aktarma seçeneği**.

   * Seçin **kullanım IOT Hub'ından kullanılabilir abonelikleri** zaten varsa var olan IOT hub'ı aboneliklerinizden biri. En kolay yaklaşım budur.
   * Seçin **sağlamak IOT hub'ı ayarlarını elle** zaman IOT hub'ı aboneliklerinize dış veya gelişmiş seçenekleri seçmenizi ister.

1. Seçtiyseniz **kullanım IOT Hub'ından kullanılabilir abonelikleri** seçeneği, aşağıdaki tabloda, her gerekli bir özellik açıklanmaktadır:

   ![IOT hub'ı dört][4]

   | Özellik | Açıklama |
   | --- | --- |
   | Abonelik Kimliği | Bu IOT hub'ı oluşturulduğu abonelik seçin.
   | IOT hub'ı adı | IOT hub'ı adını seçin.
   | IOT hub'ı ilke adı | IOT hub'ı Ayarlar sekmesinde bulunabilir paylaşılan erişim ilkesini seçin. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağınız için paylaşılan erişim ilkesi *gerekir* sahip **hizmetini bağlama** izinleri.
   | IOT hub'ı ilke anahtarı | Anahtar önceden doldurulur.
   | IOT Hub tüketici grubu | IOT Hub'ından olayları okumak için tüketici grubu. Olay kaynağınız için ayrılmış bir tüketici grubu kullanmak için önerilir.
   | Olay serileştirme biçimi | JSON şu an yalnızca seri hale getirme. Veri okuma veya olay iletileri şu biçimde olmalıdır. |
   | Zaman damgası özellik adı | Bu değeri belirlemek için IOT Hub'ına gönderilen ileti verileri ileti biçimi anlamanız gerekir. Bu değer **adı** ileti verileri, olay zaman damgası kullanmak istediğiniz belirli olay özelliğini. Değer büyük/küçük harf duyarlıdır. Boş bırakıldığına **olay sıraya alma süresi** içinde olay kaynağı olarak olay zaman damgası kullanılır. |

1. Seçtiyseniz **sağlamak IOT hub'ı ayarlarını elle** seçeneği, aşağıdaki tabloda, her gerekli bir özellik açıklanmaktadır:

   | Özellik | Açıklama |
   | --- | --- |
   | Abonelik Kimliği | Bu IOT hub'ının oluşturulduğu abonelik.
   | Kaynak grubu | Bu IOT hub'ının oluşturulduğu kaynak grubu adı.
   | IOT hub'ı adı | IOT Hub'ınızın adıdır. IOT Hub'ınızı oluşturduğunuzda, onu bir özel ad da vermiş oldunuz.
   | IOT hub'ı ilke adı | IOT hub'ı Ayarlar sekmesinde oluşturulabilmesi için paylaşılan erişim ilkesi. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağınız için paylaşılan erişim ilkesi *gerekir* sahip **hizmetini bağlama** izinleri.
   | IOT hub'ı ilke anahtarı | Service Bus ad alanı kimlik doğrulaması yapmak için kullanılan paylaşılan erişim anahtarı. Birincil veya ikincil anahtarı buraya girin.
   | IOT Hub tüketici grubu | IOT Hub'ından olayları okumak için tüketici grubu. Olay kaynağınız için ayrılmış bir tüketici grubu kullanmak için önerilir.
   | Olay serileştirme biçimi | JSON şu an yalnızca seri hale getirme. Veri okuma veya olay iletileri şu biçimde olmalıdır. |
   | Zaman damgası özellik adı | Bu değeri belirlemek için IOT Hub'ına gönderilen ileti verileri ileti biçimi anlamanız gerekir. Bu değer **adı** ileti verileri, olay zaman damgası kullanmak istediğiniz belirli olay özelliğini. Değer büyük/küçük harf duyarlıdır. Boş bırakıldığına **olay sıraya alma süresi** içinde olay kaynağı olarak olay zaman damgası kullanılır. |

1. IOT Hub'ınıza eklediğiniz adanmış TSI tüketici grubu adını ekleyin.

1. Seçin **Oluştur** yeni olay kaynağı ekleyin.

   ![IOT hub'ı beş][5]

1. Olay kaynağının oluşturulmasından sonra, Zaman Serisi Görüşleri ortamınıza veri akışını otomatik olarak başlatır.

## <a name="next-steps"></a>Sonraki adımlar

* [Veri erişim ilkelerini tanımlama](time-series-insights-data-access.md) verilerin güvenliğini sağlamak için.
* [Olayları gönderme](time-series-insights-send-events.md) olay kaynağına.
* Ortamınızda erişim [Time Series Insights gezgininin](https://insights.timeseries.azure.com).

<!-- Images -->
[1]: media/time-series-insights-how-to-add-an-event-source-iothub/iothub_one.png
[2]: media/time-series-insights-how-to-add-an-event-source-iothub/iothub_two.png
[3]: media/time-series-insights-how-to-add-an-event-source-iothub/iothub_three.png
[4]: media/time-series-insights-how-to-add-an-event-source-iothub/iothub_four.png
[5]: media/time-series-insights-how-to-add-an-event-source-iothub/iothub_five.png