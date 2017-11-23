---
title: "Azure zaman serisi Öngörüler'e bir IOT hub'ı olay kaynağı ekleme | Microsoft Docs"
description: "Bu makalede bir IOT hub'ına zaman serisi Öngörüler ortamınıza bağlı bir olay kaynağı ekleme"
services: time-series-insights
ms.service: time-series-insights
author: sandshadow
ms.author: edett
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/21/2017
ms.openlocfilehash: 0469c35056d1d02457c162b8540af472b84f1e92
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2017
---
# <a name="how-to-add-an-iot-hub-event-source-to-time-series-insights-environment"></a>Zaman serisi Öngörüler ortamına bir IOT hub'ı olay kaynağı ekleme
Bu makalede, IOT Hub'ından zaman serisi Öngörüler ortamınıza verilerini okuyan bir olay kaynağı eklemek için Azure Portalı'nı kullanmayı açıklar.

## <a name="prerequisites"></a>Ön koşullar
- Bir zaman serisi Öngörüler ortamı oluşturun. Daha fazla bilgi için bkz: [Azure zaman serisi Öngörüler ortam oluşturma](time-series-insights-get-started.md) 
- IOT hub'ı oluşturun. IOT hub'ları hakkında daha fazla bilgi için bkz: [Azure portalını kullanarak IOT Hub oluşturma](../iot-hub/iot-hub-create-through-portal.md)
- IOT hub'ı etkin ileti olayları gönderilen olmalıdır.
- Gelen kullanmak için IOT hub'zaman serisi Insight ortamı için ayrılmış bir tüketici grubu oluşturun. Herhangi bir tüketiciye paylaşılmayan kendi ayrılmış bir tüketici grubundaki her zaman serisi Öngörüler olay kaynağı olmalıdır. Birden çok okuyucular aynı tüketici grubu olaylarından kullanırsa, tüm okuyucular hatalar görmeniz olasıdır. Ayrıntılar için bkz [IOT Hub Geliştirici Kılavuzu](../iot-hub/iot-hub-devguide.md).

## <a name="add-a-new-event-source"></a>Yeni bir olay kaynağı Ekle
1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Mevcut zaman serisi Öngörüler ortamınızın bulun. Tıklatın **tüm kaynakları** Azure portalının sol tarafındaki menüde. Zaman Serisi Görüşleri ortamınızı seçin.

3. Altında **ortamı topolojisi** başlığını tıklatın **olay kaynakları**.
   ![Olay kaynakları + Ekle](media/time-series-insights-how-to-add-an-event-source-iothub/1-event-sources.png)

4. Tıklatın **+ Ekle**.

5. Sağlayan bir **olay kaynağı adı** bu zaman serisi Öngörüler ortamına benzersiz gibi **olay akışı**.

   ![Form ilk üç parametre doldurun.](media/time-series-insights-how-to-add-an-event-source-iothub/2-import-option.png)

6. Seçin **kaynak** olarak **IOT hub'ı**.

7. Uygun seçin **alma seçeneği**. 
   - Seçin **kullanım IOT Hub'ından kullanılabilir abonelikler** zaten varsa var olan IOT hub'ı aboneliklerinizden biri. Bu kolay bir yaklaşımdır.
   - Seçin **sağlamak ve IOT hub'ı ayarlarını elle** zaman IOT hub'ı aboneliklerinize dış veya gelişmiş seçenekleri seçmenizi ister. 

8. Seçtiyseniz **kullanım IOT Hub'ından kullanılabilir abonelikler** seçeneği, aşağıdaki tabloda, her gerekli bir özellik açıklanmaktadır:

   ![Abonelik ve olay hub'ı ayrıntıları](media/time-series-insights-how-to-add-an-event-source-iothub/3-new-event-source.png)

   | Özellik | Açıklama |
   | --- | --- |
   | Abonelik Kimliği | Bu IOT hub'ının oluşturulduğu abonelik seçin.
   | IOT hub'ı adı | IOT Hub adını seçin.
   | IOT hub'ı ilke adı | IOT hub'ı Ayarlar sekmesinde bulunan paylaşılan erişim ilkesi seçin. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağı için paylaşılan erişim ilkesi *gerekir* sahip **service bağlanma** izinleri.
   | IOT hub'ı ilke anahtarı | Anahtar önceden doldurulmaz.
   | IOT Hub tüketici grubu | IOT Hub'ından olayları okumak için tüketici grubu. Olay kaynağı için ayrılmış bir tüketici grubu kullanmak için önerilir.
   | Olayı seri hale getirme biçimi | JSON günümüzde yalnızca serileştirme şeklindedir. Olay iletileri bu biçiminde olması gerekir veya hiç veri okuyabilir. |
   | Zaman damgası özelliği adı | Bu değeri belirlemek için ileti biçimi, IOT Hub'ına gönderilen ileti verilerin anlamanız gerekir. Bu değer **adı** olay zaman damgası kullanmak istediğiniz ileti veri belirli olay özelliğinin. Değeri büyük/küçük harf duyarlıdır. Boş bırakılırsa **olay sıraya alma süresi** içinde olay kaynağı olay zaman damgası kullanılır. |

9. Seçtiyseniz **sağlamak ve IOT hub'ı ayarlarını elle** seçeneği, aşağıdaki tabloda, her gerekli bir özellik açıklanmaktadır:

   | Özellik | Açıklama |
   | --- | --- |
   | Abonelik Kimliği | Bu IOT hub'ının oluşturulduğu abonelik.
   | Kaynak grubu | Bu IOT hub'ının oluşturulduğu kaynak grubunun adı.
   | IOT hub'ı adı | IOT Hub'ınızın adıdır. IOT Hub'ınızı oluşturduğunuzda, ona bir özel ad da vermiş.
   | IOT hub'ı ilke adı | Paylaşılan Erişim İlkesi IOT hub'ı ayarları sekmesinde oluşturulabilir. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağı için paylaşılan erişim ilkesi *gerekir* sahip **service bağlanma** izinleri.
   | IOT hub'ı ilke anahtarı | Hizmet veri yolu ad alanına erişimi kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı. Birincil veya ikincil anahtarı buraya girin.
   | IOT Hub tüketici grubu | IOT Hub'ından olayları okumak için tüketici grubu. Olay kaynağı için ayrılmış bir tüketici grubu kullanmak için önerilir.
   | Olayı seri hale getirme biçimi | JSON günümüzde yalnızca serileştirme şeklindedir. Olay iletileri bu biçiminde olması gerekir veya hiç veri okuyabilir. |
   | Zaman damgası özelliği adı | Bu değeri belirlemek için ileti biçimi, IOT Hub'ına gönderilen ileti verilerin anlamanız gerekir. Bu değer **adı** olay zaman damgası kullanmak istediğiniz ileti veri belirli olay özelliğinin. Değeri büyük/küçük harf duyarlıdır. Boş bırakılırsa **olay sıraya alma süresi** içinde olay kaynağı olay zaman damgası kullanılır. |

10. Seçin **oluşturma** yeni olay kaynağı ekleyin.

   ![Oluştur’a tıklayın](media/time-series-insights-how-to-add-an-event-source-iothub/4-create-button.png)

   Olay kaynağının oluşturulmasından sonra, Zaman Serisi Görüşleri ortamınıza veri akışını otomatik olarak başlatır.

### <a name="add-a-consumer-group-to-your-iot-hub"></a>Bir tüketici grubu IOT Hub'ınızı ekleyin
Tüketici grupları, Azure IOT hub'larından verilerini çekmesini uygulamalar tarafından kullanılır. Güvenilir bir şekilde IOT Hub'ından veri okumak için bu zaman serisi Öngörüler ortam yalnızca tarafından kullanılmak üzere ayrılmış bir tüketici grubu, sağlar.

IOT Hub'ınıza yeni bir tüketici grubu eklemek için aşağıdaki adımları izleyin:
1. Azure portalında bulun ve IOT Hub'ınızı açın.

2. Altında **ileti** başlığını seçin **uç noktaları**. 

   ![Bir tüketici grubu Ekle](media/time-series-insights-how-to-add-an-event-source-iothub/5-add-consumer-group.png)

3. Seçin **olayları** uç noktasını ve **özellikleri** sayfası açılır.

4. Altında **tüketici grupları** başlığı tüketici grubu için benzersiz bir ad sağlayın. Bu aynı adı zaman serisi Öngörüler ortamında yeni bir olay kaynağı oluştururken kullanın.

5. Seçin **kaydetmek** yeni bir tüketici grubundaki kaydetmek için.

## <a name="next-steps"></a>Sonraki adımlar
- [Veri erişim ilkeleri tanımlamanıza](time-series-insights-data-access.md) verilerin güvenliğini sağlamak için.
- [Olayları göndermek](time-series-insights-send-events.md) olay kaynağı.
- Ortamınızda erişim [zaman serisi Öngörüler explorer](https://insights.timeseries.azure.com).
