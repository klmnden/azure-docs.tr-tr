---
title: Azure zaman serisi Öngörüler'e bir olay hub'ı olay kaynağı ekleme | Microsoft Docs
description: Bu makalede, bir olay hub'ına zaman serisi Öngörüler ortamınıza bağlı bir olay kaynağı eklemeyi açıklar.
ms.service: time-series-insights
services: time-series-insights
author: sandshadow
ms.author: edett
manager: jhubbard
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/21/2017
ms.openlocfilehash: ed151160bd8bd0f0241e1a728fab53570e33a201
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34656026"
---
# <a name="how-to-add-an-event-hub-event-source-to-time-series-insights-environment"></a>Zaman serisi Öngörüler ortam için bir olay hub'ı olay kaynağı ekleme

Bu makalede, bir Event Hub'ından zaman serisi Öngörüler ortamınıza verilerini okuyan bir olay kaynağı eklemek için Azure Portalı'nı kullanmayı açıklar.

## <a name="prerequisites"></a>Önkoşullar
- Bir zaman serisi Öngörüler ortamı oluşturun. Daha fazla bilgi için bkz: [Azure zaman serisi Öngörüler ortam oluşturma](time-series-insights-get-started.md) 
- Olay Hub'ı oluşturma. Event Hubs hakkında daha fazla bilgi için bkz: [bir olay hub'ları ad alanı ve Azure portalını kullanarak bir event hub oluşturma](../event-hubs/event-hubs-create.md)
- Olay hub'ı etkin ileti olayları gönderilen olmalıdır. Daha fazla bilgi için bkz: [.NET Framework kullanılarak Azure Event Hubs için olayları göndermek](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).
- Gelen kullanmak için olay hub'zaman serisi Insight ortamı için ayrılmış bir tüketici grubu oluşturun. Herhangi bir tüketiciye paylaşılmayan kendi ayrılmış bir tüketici grubundaki her zaman serisi Öngörüler olay kaynağı olmalıdır. Birden çok okuyucular aynı tüketici grubu olaylarından kullanırsa, tüm okuyucular hatalar görmeniz olasıdır. Ayrıca olay hub'ı başına 20 tüketici grupları sınırı yoktur. Ayrıntılar için bkz [Event Hubs Programlama Kılavuzu](../event-hubs/event-hubs-programming-guide.md).

## <a name="add-a-new-event-source"></a>Yeni bir olay kaynağı Ekle
1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Mevcut zaman serisi Öngörüler ortamınızın bulun. Tıklatın **tüm kaynakları** Azure portalının sol tarafındaki menüde. Zaman Serisi Görüşleri ortamınızı seçin.

3. Altında **ortamı topolojisi** başlığını tıklatın **olay kaynakları**.

   ![Olay kaynakları + Ekle](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png)

4. **+ Ekle**'ye tıklayın.

5. Sağlayan bir **olay kaynağı adı** bu zaman serisi Öngörüler ortamına benzersiz gibi **olay akışı**.

   ![Form ilk üç parametre doldurun.](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png)

6. Seçin **kaynak** olarak **olay hub'ı**.

7. Uygun seçin **alma seçeneği**. 
   - Seçin **kullanım olay Hub'ından kullanılabilir abonelikler** zaten varsa var olan bir Event Hub aboneliklerinizden biri. Bu kolay bir yaklaşımdır.
   - Seçin **sağlamak olay hub'ı ayarlarını elle** zaman olay hub'ı aboneliklerinize dış veya gelişmiş seçenekleri seçmenizi ister. 

8. Seçtiyseniz **kullanım olay Hub'ından kullanılabilir abonelikler** seçeneği, aşağıdaki tabloda, her gerekli bir özellik açıklanmaktadır:

   ![Abonelik ve olay hub'ı ayrıntıları](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png)

   | Özellik | Açıklama |
   | --- | --- |
   | Abonelik Kimliği | Bu olay hub'ının oluşturulduğu abonelik seçin.
   | Service bus ad alanı | Olay hub'ı içeren Service Bus ad alanı seçin.
   | Olay hub'ı adı | Olay hub'ı adını seçin.
   | Olay hub'ı ilke adı | Olay hub'ı yapılandırma sekmesinde oluşturulabilmesi için paylaşılan erişim ilkesi seçin. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağı için paylaşılan erişim ilkesi *gerekir* sahip **okuma** izinleri.
   | Olay hub'ı İlkesi anahtarı | Anahtar değeri önceden girilmiş.
   | Olay hub tüketici grubu | Olay Hub'ından olayları okumak için tüketici grubu. Olay kaynağı için ayrılmış bir tüketici grubu kullanmak için önerilir. |
   | Olay serileştirme biçimi | JSON günümüzde yalnızca serileştirme şeklindedir. Olay iletileri bu biçiminde olması gerekir veya hiç veri okuyabilir. |
   | Zaman damgası özelliği adı | Bu değeri belirlemek için olay Hub'ına gönderilen ileti verilerin ileti biçimi anlamanız gerekir. Bu değer **adı** olay zaman damgası kullanmak istediğiniz ileti veri belirli olay özelliğinin. Değeri büyük/küçük harf duyarlıdır. Boş bırakılırsa **olay sıraya alma süresi** içinde olay kaynağı olay zaman damgası kullanılır. |


9. Seçtiyseniz **sağlamak olay hub'ı ayarlarını elle** seçeneği, aşağıdaki tabloda, her gerekli bir özellik açıklanmaktadır:

   | Özellik | Açıklama |
   | --- | --- |
   | Abonelik Kimliği | Bu olay hub'ının oluşturulduğu abonelik.
   | Kaynak grubu | Bu olay hub'ının oluşturulduğu kaynak grubu.
   | Service bus ad alanı | Bir hizmet veri yolu ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, hizmet veri yolu ad alanı da oluşturmuş olursunuz.
   | Olay hub'ı adı | Olay Hub'ınızı adı. Olay hub'ınızı oluşturduğunuzda, ona bir özel ad da vermiş.
   | Olay hub'ı ilke adı | Paylaşılan Erişim İlkesi olay hub'ı yapılandırma sekmesinde oluşturulabilir. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağı için paylaşılan erişim ilkesi *gerekir* sahip **okuma** izinleri.
   | Olay hub'ı İlkesi anahtarı | Hizmet veri yolu ad alanına erişimi kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı. Birincil veya ikincil anahtarı buraya girin.
   | Olay hub tüketici grubu | Olay Hub'ından olayları okumak için tüketici grubu. Olay kaynağı için ayrılmış bir tüketici grubu kullanmak için önerilir.
   | Olay serileştirme biçimi | JSON günümüzde yalnızca serileştirme şeklindedir. Olay iletileri bu biçiminde olması gerekir veya hiç veri okuyabilir. |
   | Zaman damgası özelliği adı | Bu değeri belirlemek için olay Hub'ına gönderilen ileti verilerin ileti biçimi anlamanız gerekir. Bu değer **adı** olay zaman damgası kullanmak istediğiniz ileti veri belirli olay özelliğinin. Değeri büyük/küçük harf duyarlıdır. Boş bırakılırsa **olay sıraya alma süresi** içinde olay kaynağı olay zaman damgası kullanılır. |


10. Seçin **oluşturma** yeni olay kaynağı ekleyin.
   
   ![Oluştur’a tıklayın](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png)

   Olay kaynağının oluşturulmasından sonra, Zaman Serisi Görüşleri ortamınıza veri akışını otomatik olarak başlatır.


### <a name="add-a-consumer-group-to-your-event-hub"></a>Bir tüketici grubu için olay Hub'ınızı ekleyin
Tüketici grupları, Azure olay hub'larından verilerini çekmesini uygulamalar tarafından kullanılır. Güvenilir bir şekilde olay Hub'ından veri okumak için bu zaman serisi Öngörüler ortam yalnızca tarafından kullanılmak üzere ayrılmış bir tüketici grubu, sağlar.

Olay Hub'ınıza yeni bir tüketici grubu eklemek için aşağıdaki adımları izleyin:
1. Azure portalında bulun ve olay Hub'ınızı açın.

2. Altında **varlıklar** başlığını seçin **tüketici grupları**.

   ![Olay hub'ı - bir tüketici grubu Ekle](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png)

3. Seçin **+ tüketici grubu** yeni bir tüketici grubu eklemek için. 

4. Üzerinde **tüketici grupları** sayfasında, yeni bir sağlayın benzersiz **adı**.  Zaman serisi Öngörüler ortamında yeni bir olay kaynağı oluştururken, bu aynı adı kullanın.

5. Seçin **oluşturma** yeni tüketici grubu oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
- [Veri erişim ilkeleri tanımlamanıza](time-series-insights-data-access.md) verilerin güvenliğini sağlamak için.
- [Olayları göndermek](time-series-insights-send-events.md) olay kaynağı.
- Ortamınızda erişim [zaman serisi Öngörüler explorer](https://insights.timeseries.azure.com).
