---
title: Azure Time Series Insights için bir Event Hub olay kaynağı ekleme | Microsoft Docs
description: Bu makalede, zaman serisi görüşleri ortamınıza olay Hub'ına bağlı bir olay kaynağı eklemeyi açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/21/2017
ms.openlocfilehash: ce4bf1ab74e4203f0deb7b2984ffa6a66d5efd4a
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627119"
---
# <a name="how-to-add-an-event-hub-event-source-to-time-series-insights-environment"></a>Zaman serisi görüşleri ortamına bir Event Hub olay kaynağı ekleme

Bu makalede, zaman serisi görüşleri ortamınıza bir olay Hub'ından veri okuyan bir olay kaynağı eklemek için Azure portalını kullanmayı açıklar.

## <a name="prerequisites"></a>Önkoşullar
- Zaman serisi görüşleri ortamı oluşturun. Daha fazla bilgi için [Azure zaman serisi görüşleri ortamı oluşturma](time-series-insights-get-started.md) 
- Olay Hub'ı oluşturma. Event Hubs hakkında daha fazla bilgi için bkz. [bir Event Hubs ad alanı ve Azure portalını kullanarak bir olay hub'ı oluşturma](../event-hubs/event-hubs-create.md)
- Etkin ileti olayları gönderilmekte olan olay hub'ı olmalıdır. Daha fazla bilgi için [olayları .NET Framework kullanarak Azure Event Hubs'a gönderme](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).
- Gelen kullanmak için olay Hub'zaman serisi görüşleri ortamı için ayrılmış bir tüketici grubu oluşturun. Her zaman serisi görüşleri olay kaynağı ile herhangi bir tüketiciye paylaşılmıyor kendi adanmış bir tüketici grubu olmalıdır. Aynı tüketici grubu olaylardan birden fazla okuyucuyu kapsayacak kullanmasına, tüm okuyucular hatalar görmeniz olasıdır. Ayrıca bir olay hub'ı başına 20 tüketici grubu sınırı unutmayın. Ayrıntılar için bkz [Event Hubs Programlama Kılavuzu](../event-hubs/event-hubs-programming-guide.md).

### <a name="add-a-consumer-group-to-your-event-hub"></a>Olay Hub'ınıza bir tüketici grubu Ekle
Tüketici grupları, Azure Event Hubs'dan veri çekmek için uygulamalar tarafından kullanılır. Güvenilir bir şekilde verileri olay Hub'ından okumak için bu zaman serisi görüşleri ortamına yalnızca tarafından kullanılmak üzere ayrılmış bir tüketici grubu, sağlar.

Olay Hub'ında yeni bir tüketici grubu eklemek için aşağıdaki adımları izleyin:
1. Azure portalında bulun ve olay Hub'ınızı açın.

2. Altında **varlıkları** başlığı seçin **tüketici grupları**.

   ![Olay hub'ı - bir tüketici grubu Ekle](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png)

3. Seçin **+ tüketici grubu** yeni bir tüketici grubu eklemek için. 

4. Üzerinde **tüketici grupları** sayfasında, yeni bir sağlayın benzersiz **adı**.  Zaman serisi görüşleri ortamına yeni bir olay kaynağı oluştururken bu aynı adı kullanın.

5. Seçin **Oluştur** yeni tüketici grubu oluşturun.

## <a name="add-a-new-event-source"></a>Yeni bir olay kaynağı ekleme
1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Mevcut Time Series Insights ortamınızı bulun. Tıklayın **tüm kaynakları** Azure portalının sol tarafındaki menüde. Zaman Serisi Görüşleri ortamınızı seçin.

3. Altında **ortam topolojisinin** başlığını tıklatın **olay kaynakları**.

   ![Olay kaynakları + Ekle](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png)

4. **+ Ekle**'ye tıklayın.

5. Sağlayan bir **olay kaynağı adını** bu zaman serisi görüşleri ortamına benzersiz gibi **olay akışı**.

   ![Formundaki ilk üç parametreleri doldurun.](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png)

6. Seçin **kaynak** olarak **olay hub'ı**.

7. Uygun seçin **içeri aktarma seçeneği**. 
   - Seçin **kullanımı olay Hub'ından kullanılabilir abonelikleri** zaten varsa var olan bir olay hub'ı aboneliklerinizden biri. En kolay yaklaşım budur.
   - Seçin **sağlayan olay hub'ı ayarlarını elle** olduğunda olay hub'ı aboneliklerinize dış veya gelişmiş seçenekleri seçmenizi ister. 

8. Seçtiyseniz **kullanımı olay Hub'ından kullanılabilir abonelikleri** seçeneği, aşağıdaki tabloda, her gerekli bir özellik açıklanmaktadır:

   ![Abonelik ve olay hub'ı ayrıntıları](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png)

   | Özellik | Açıklama |
   | --- | --- |
   | Abonelik Kimliği | Bu olay hub'ı oluşturulduğu abonelik seçin.
   | Service bus ad alanı | Olay hub'ı içeren Service Bus ad alanı seçin.
   | Olay hub'ı adı | Olay hub'ı adını seçin.
   | Olay hub'ı ilke adı | Olay hub'ı yapılandırma sekmesinde oluşturulabilmesi için paylaşılan erişim ilkesi seçin. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağınız için paylaşılan erişim ilkesi *gerekir* sahip **okuma** izinleri.
   | Olay hub'ı ilke anahtarı | Anahtar değeri önceden doldurulmuş.
   | Olay hub'ı tüketici grubu | Olayları olay Hub'ından okumak için tüketici grubu. Olay kaynağınız için ayrılmış bir tüketici grubu kullanmak için önerilir. |
   | Olay serileştirme biçimi | JSON şu an yalnızca seri hale getirme. Veri okuma veya olay iletileri şu biçimde olmalıdır. |
   | Zaman damgası özellik adı | Bu değeri belirlemek için olay Hub'ına gönderilen ileti verileri ileti biçimi anlamanız gerekir. Bu değer **adı** ileti verileri, olay zaman damgası kullanmak istediğiniz belirli olay özelliğini. Değer büyük/küçük harf duyarlıdır. Boş bırakıldığına **olay sıraya alma süresi** içinde olay kaynağı olarak olay zaman damgası kullanılır. |


9. Seçtiyseniz **sağlayan olay hub'ı ayarlarını elle** seçeneği, aşağıdaki tabloda, her gerekli bir özellik açıklanmaktadır:

   | Özellik | Açıklama |
   | --- | --- |
   | Abonelik Kimliği | Bu olay hub'ı oluşturulduğu abonelik.
   | Kaynak grubu | Bu olay hub'ı oluşturulduğu kaynak grubu.
   | Service bus ad alanı | Service Bus ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, bir Service Bus ad alanı da oluşturmuş olursunuz.
   | Olay hub'ı adı | Olay Hub'ınızın adıdır. Olay hub'ınızı oluşturduğunuzda, onu bir özel ad da vermiş oldunuz.
   | Olay hub'ı ilke adı | Paylaşılan Erişim İlkesi olay hub'ı yapılandırma sekmesinde oluşturulabilir. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağınız için paylaşılan erişim ilkesi *gerekir* sahip **okuma** izinleri.
   | Olay hub'ı ilke anahtarı | Service Bus ad alanı kimlik doğrulaması yapmak için kullanılan paylaşılan erişim anahtarı. Birincil veya ikincil anahtarı buraya girin.
   | Olay hub'ı tüketici grubu | Olayları olay Hub'ından okumak için tüketici grubu. Olay kaynağınız için ayrılmış bir tüketici grubu kullanmak için önerilir.
   | Olay serileştirme biçimi | JSON şu an yalnızca seri hale getirme. Veri okuma veya olay iletileri şu biçimde olmalıdır. |
   | Zaman damgası özellik adı | Bu değeri belirlemek için olay Hub'ına gönderilen ileti verileri ileti biçimi anlamanız gerekir. Bu değer **adı** ileti verileri, olay zaman damgası kullanmak istediğiniz belirli olay özelliğini. Değer büyük/küçük harf duyarlıdır. Boş bırakıldığına **olay sıraya alma süresi** içinde olay kaynağı olarak olay zaman damgası kullanılır. |

10. Olay hub'ınıza eklediğiniz adanmış TSI tüketici grubu adını ekleyin.

11. Seçin **Oluştur** yeni olay kaynağı ekleyin.
   
   ![Oluştur’a tıklayın](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png)

   Olay kaynağının oluşturulmasından sonra, Zaman Serisi Görüşleri ortamınıza veri akışını otomatik olarak başlatır.


## <a name="next-steps"></a>Sonraki adımlar
- [Veri erişim ilkelerini tanımlama](time-series-insights-data-access.md) verilerin güvenliğini sağlamak için.
- [Olayları gönderme](time-series-insights-send-events.md) olay kaynağına.
- Ortamınızda erişim [Time Series Insights gezgininin](https://insights.timeseries.azure.com).
