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
ms.date: 11/21/2017
ms.openlocfilehash: b6beecbf64cee925f62ac4c82919926fcb79940a
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627381"
---
# <a name="how-to-add-an-iot-hub-event-source-to-time-series-insights-environment"></a>Zaman serisi görüşleri ortamına bir IOT Hub olay kaynağı ekleme
Bu makalede, zaman serisi görüşleri ortamınıza bir IOT Hub'ından veri okuyan bir olay kaynağı eklemek için Azure portalını kullanmayı açıklar.

## <a name="prerequisites"></a>Önkoşullar
- Zaman serisi görüşleri ortamı oluşturun. Daha fazla bilgi için [Azure zaman serisi görüşleri ortamı oluşturma](time-series-insights-get-started.md) 
- Bir IoT Hub oluşturma. IOT hub'ları hakkında daha fazla bilgi için bkz. [Azure portalını kullanarak IOT Hub oluşturma](../iot-hub/iot-hub-create-through-portal.md)
- Etkin ileti olayları gönderilmekte olan IOT hub'ı olmalıdır.
- Gelen kullanmak için IOT hub'zaman serisi görüşleri ortamı için ayrılmış bir tüketici grubu oluşturun. Her zaman serisi görüşleri olay kaynağı ile herhangi bir tüketiciye paylaşılmıyor kendi adanmış bir tüketici grubu olmalıdır. Aynı tüketici grubu olaylardan birden fazla okuyucuyu kapsayacak kullanmasına, tüm okuyucular hatalar görmeniz olasıdır. Ayrıntılar için bkz [IOT Hub Geliştirici kılavuzunun](../iot-hub/iot-hub-devguide.md).

### <a name="add-a-consumer-group-to-your-iot-hub"></a>IOT Hub'ınıza bir tüketici grubu Ekle
Tüketici grupları, Azure IOT hub'ları veri çekmek için uygulamalar tarafından kullanılır. Güvenilir bir şekilde verileri IOT Hub'ından okumak için bu zaman serisi görüşleri ortamına yalnızca tarafından kullanılmak üzere ayrılmış bir tüketici grubu, sağlar.

IOT Hub'ınıza yeni bir tüketici grubu eklemek için aşağıdaki adımları izleyin:
1. Azure portalında bulun ve IOT Hub'ınızı açın.

2. Altında **Mesajlaşma** başlığı seçin **uç noktaları**. 

   ![Bir tüketici grubu Ekle](media/time-series-insights-how-to-add-an-event-source-iothub/5-add-consumer-group.png)

3. Seçin **olayları** uç noktasını ve **özellikleri** sayfası açılır.

4. Altında **tüketici grupları** başlığı tüketici grubu için yeni ve benzersiz bir ad sağlayın. Bu aynı adı, yeni bir olay kaynağı oluştururken zaman serisi görüşleri ortamı kullanın.

5. Seçin **Kaydet** yeni tüketici grubunu kaydetmek için.

## <a name="add-a-new-event-source"></a>Yeni bir olay kaynağı ekleme
1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Mevcut Time Series Insights ortamınızı bulun. Tıklayın **tüm kaynakları** Azure portalının sol tarafındaki menüde. Zaman Serisi Görüşleri ortamınızı seçin.

3. Altında **ortam topolojisinin** başlığını tıklatın **olay kaynakları**.
   ![Olay kaynakları + Ekle](media/time-series-insights-how-to-add-an-event-source-iothub/1-event-sources.png)

4. **+ Ekle**'ye tıklayın.

5. Sağlayan bir **olay kaynağı adını** bu zaman serisi görüşleri ortamına benzersiz gibi **olay akışı**.

   ![Formundaki ilk üç parametreleri doldurun.](media/time-series-insights-how-to-add-an-event-source-iothub/2-import-option.png)

6. Seçin **kaynak** olarak **IOT hub'ı**.

7. Uygun seçin **içeri aktarma seçeneği**. 
   - Seçin **kullanım IOT Hub'ından kullanılabilir abonelikleri** zaten varsa var olan IOT hub'ı aboneliklerinizden biri. En kolay yaklaşım budur.
   - Seçin **sağlamak IOT hub'ı ayarlarını elle** zaman IOT hub'ı aboneliklerinize dış veya gelişmiş seçenekleri seçmenizi ister. 

8. Seçtiyseniz **kullanım IOT Hub'ından kullanılabilir abonelikleri** seçeneği, aşağıdaki tabloda, her gerekli bir özellik açıklanmaktadır:

   ![Abonelik ve olay hub'ı ayrıntıları](media/time-series-insights-how-to-add-an-event-source-iothub/3-new-event-source.png)

   | Özellik | Açıklama |
   | --- | --- |
   | Abonelik Kimliği | Bu IOT hub'ı oluşturulduğu abonelik seçin.
   | IOT hub'ı adı | IOT hub'ı adını seçin.
   | IOT hub'ı ilke adı | IOT hub'ı Ayarlar sekmesinde bulunabilir paylaşılan erişim ilkesini seçin. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağınız için paylaşılan erişim ilkesi *gerekir* sahip **hizmetini bağlama** izinleri.
   | IOT hub'ı ilke anahtarı | Anahtar önceden doldurulur.
   | IOT Hub tüketici grubu | IOT Hub'ından olayları okumak için tüketici grubu. Olay kaynağınız için ayrılmış bir tüketici grubu kullanmak için önerilir.
   | Olay serileştirme biçimi | JSON şu an yalnızca seri hale getirme. Veri okuma veya olay iletileri şu biçimde olmalıdır. |
   | Zaman damgası özellik adı | Bu değeri belirlemek için IOT Hub'ına gönderilen ileti verileri ileti biçimi anlamanız gerekir. Bu değer **adı** ileti verileri, olay zaman damgası kullanmak istediğiniz belirli olay özelliğini. Değer büyük/küçük harf duyarlıdır. Boş bırakıldığına **olay sıraya alma süresi** içinde olay kaynağı olarak olay zaman damgası kullanılır. |

9. Seçtiyseniz **sağlamak IOT hub'ı ayarlarını elle** seçeneği, aşağıdaki tabloda, her gerekli bir özellik açıklanmaktadır:

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

10. IOT Hub'ınıza eklediğiniz adanmış TSI tüketici grubu adını ekleyin.

11. Seçin **Oluştur** yeni olay kaynağı ekleyin.

   ![Oluştur’a tıklayın](media/time-series-insights-how-to-add-an-event-source-iothub/4-create-button.png)

   Olay kaynağının oluşturulmasından sonra, Zaman Serisi Görüşleri ortamınıza veri akışını otomatik olarak başlatır.

## <a name="next-steps"></a>Sonraki adımlar
- [Veri erişim ilkelerini tanımlama](time-series-insights-data-access.md) verilerin güvenliğini sağlamak için.
- [Olayları gönderme](time-series-insights-send-events.md) olay kaynağına.
- Ortamınızda erişim [Time Series Insights gezgininin](https://insights.timeseries.azure.com).
