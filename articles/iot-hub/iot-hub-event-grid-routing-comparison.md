---
title: Yönlendirme için IOT Hub ile Event Grid, karşılaştırma | Microsoft Docs
description: IOT hub'ı kendi ileti yönlendirme hizmeti sunar, ancak aynı zamanda olay yayımlama için Event Grid ile tümleşir. İki özelliklerini karşılaştırın.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: kgremban
ms.openlocfilehash: e7d5ab3ebdf29fbf699220a3b214176ec4862739
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60398743"
---
# <a name="compare-message-routing-and-event-grid-for-iot-hub"></a>İleti Yönlendirme ve Event Grid için IOT Hub'ı karşılaştırma

Azure IOT Hub, verileri iş uygulamalarınızla tümleştirin ve bağlı cihazlarınızdan veri akışını yeteneği sağlar. IOT Hub, IOT olayları diğer Azure Hizmetleri veya iş uygulamalarınızla tümleştirmek için iki yöntem sunar. Bu makalede, hangi senaryonuz için en iyi bir seçenektir seçebilmeniz bu özelliği sağlayan iki özellikleri açıklanmaktadır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

**[IOT Hub ileti yönlendirme](iot-hub-devguide-messages-d2c.md)**: Bu IOT hub'ı özellik için hizmet uç noktaları Azure depolama kapsayıcıları, Event Hubs, Service Bus kuyrukları ve Service Bus konuları gibi CİHAZDAN buluta iletileri yönlendirmek kullanıcıların sağlar. Yönlendirme, bir sorgulama uç noktalar için yönlendirme önce verileri filtreleme yeteneği sağlar. Cihazın telemetri verilerine ek olarak da gönderebilirsiniz [olmayan telemetri olayları](iot-hub-devguide-messages-d2c.md#non-telemetry-events) eylemleri tetiklemek için kullanılabilir. 

**Event Grid ile IOT hub'ı tümleştirme**: Azure Event Grid; Yayımla kullanan bir tam olarak yönetilen olay yönlendirme hizmetini-abonelik modeli. IOT Hub ve Event Grid için birlikte çalışır [IOT Hub olaylarını Azure ve Azure olmayan hizmetlerle tümleştirme](iot-hub-event-grid.md), neredeyse gerçek zamanlı olarak. 

## <a name="differences"></a>Farkları

İleti Yönlendirme ve Event Grid uyarı yapılandırması olanak sağlarken, ikisi arasındaki bazı temel farklar vardır. Ayrıntılar için aşağıdaki tabloya bakın:

| Özellik | IOT Hub ileti yönlendirme | Event Grid ile IOT hub'ı tümleştirme |
| ------- | --------------- | ---------- |
| **Cihaz iletileri** | Evet, ileti yönlendirme telemetri verileri için kullanılabilir. | Hayır, Event Grid telemetri IOT Hub olaylarını yalnızca kullanılabilir. |
| **Cihaz olayları** | Evet, ileti yönlendirme ikizi değişiklikleri ve cihaz yaşam döngüsü olaylarını bildirebilirsiniz. | Evet, Event Grid cihazları oluşturulan, silindi, bağlı ve IOT Hub'ından bağlantısı kesildi rapor edebilirsiniz |
| **Sıralama** | Evet, olayların sırası korunur.  | Hayır, olayların sırası garanti edilmez. | 
| **En büyük ileti boyutu** | 256 KB, CİHAZDAN buluta | 64 KB |
| **Filtreleme** | Zengin ileti uygulama özellikleri üzerinde filtreleme, ileti sistemi özelliklerini, ileti gövdesi, cihaz ikizi etiketleri ve cihaz özellikleri çifti. Örnekler için bkz [ileti yönlendirme sorgusu söz dizimi](iot-hub-devguide-routing-query-syntax.md). | Filtreleme soneki/depolama alanı gibi hiyerarşik Hizmetleri için iyi düşünülerek öneki cihaz kimlikleri temel. |
| **Uç noktaları** | <ul><li>Event Hubs</li> <li>Azure Blob Depolama</li> <li>Service Bus kuyruğu</li> <li>Hizmet Veri Yolu konuları</li></ul><br>Ücretli IOT hub'ı SKU'lar (S1, S2 ve S3), 10 özel uç noktalar ile sınırlıdır. IOT hub'ı 100 yollar oluşturulabilir. | <ul><li>Azure İşlevleri</li> <li>Azure Otomasyonu</li> <li>Event Hubs</li> <li>Logic Apps</li> <li>Depolama Blobu</li> <li>Özel Konu Başlıkları</li> <li>Web kancaları aracılığıyla üçüncü taraf hizmetleri</li></ul><br>Uç noktaları en güncel listesi için bkz. [Event Grid olay işleyicileri](../event-grid/overview.md#event-handlers). |
| **Maliyet** | İleti yönlendirme için ayrı bir ücret yoktur. Yalnızca IOT hub'ıyla telemetri girişi ücretlendirilir. Örneğin, üç farklı Uç noktalara yönlendirilmesi ileti varsa, tek bir ileti için faturalandırılırsınız. | IOT Hub'ından ücret ödemenize gerek yoktur. Event Grid aylık ilk 100.000 işlem ücretsiz olarak sağlar ve ardından $0.60 sonradan milyon işlem başına. |

## <a name="similarities"></a>Benzerlikler

IOT Hub ileti yönlendirme ve Event Grid çok benzerlikler, bazıları aşağıdaki tabloda açıklanmıştır:

| Özellik | IOT Hub ileti yönlendirme | Event Grid ile IOT hub'ı tümleştirme |
| ------- | --------------- | ---------- |
| **Güvenilirlik** | Yüksek: Uç nokta her yol için en az bir kez her ileti gönderir. Bir saat içinde teslim edilmeyen tüm iletilerin süresi dolar. | Yüksek: Her ileti Web kancası her abonelik için en az bir kez gönderir. 24 saat içinde teslim edilmeyen tüm olayların süresi dolar. | 
| **Ölçeklenebilirlik** | Yüksek: Eşzamanlı olarak bağlanan cihazların milyarlarca ileti gönderen, milyonlarca desteği için en iyi duruma getirilmiş. | Yüksek: Bölge başına saniyede 10.000.000 yönlendirme olayların özelliğine sahip. |
| **Gecikme süresi** | Düşük: Neredeyse gerçek zamanlı. | Düşük: Neredeyse gerçek zamanlı. |
| **Birden fazla uç nokta için Gönder** | Evet, birden fazla uç nokta için tek bir ileti gönderin. | Evet, birden fazla uç nokta için tek bir ileti gönderin.  | 
| **Güvenlik** | IOT Hub, cihaz başına kimlik ve iptal edilebilir erişim denetimi sağlar. Daha fazla bilgi için [IOT hub'ı erişim denetimi](iot-hub-devguide-security.md). | Event Grid üç noktalarda doğrulama sağlar: olay abonelikleri, olay yayımlama ve Web kancası olay teslimi. Daha fazla bilgi için [Event Grid güvenliğini ve kimlik doğrulaması](../event-grid/security-authentication.md). |

## <a name="how-to-choose"></a>Seçim yapma

IOT Hub ileti yönlendirme ve Event Grid ile IOT hub'ı tümleştirme benzer sonuçlar elde etmek için farklı eylemleri gerçekleştirin. Bunlar hem IOT Hub çözümünüzü bilgilerini almak ve diğer hizmetleri tepki verebilir böylece geçirin. Bu nedenle nasıl hangisinin kullanılacağını karar veririm? Kararınız yardımcı kılavuz olarak aşağıdaki soruları göz önünde bulundurun: 

* **Uç noktalar için ne tür veriler gönderdiğiniz?**

   Diğer hizmetler için telemetri verileri göndermek olduğunda, IOT Hub ileti yönlendirme kullanın. Ayrıca ileti uygulama ve Sistem özellikleri, ileti gövdesi, cihaz ikizi etiketleri ve cihaz ikizi özelliklerini sorgulama etkinleştirir yönlendirme iletisi.

   Event Grid ile IOT hub'ı tümleştirmeyi, IOT Hub hizmeti meydana gelen olayları ile birlikte çalışır. Oluşturulan, silindi, bağlı ve bağlantısı kesilmiş bir cihaz bu IOT Hub olaylarını içerir. 

* **Bu bilgileri almak hangi uç noktaları gerekiyor?**

   IOT Hub ileti yönlendirme sınırlı uç noktaları destekliyor, ancak verileri ve olayları ek uç noktalar için yönlendirecek şekilde bağlayıcılar oluşturabilirsiniz. Desteklenen uç noktalar tam listesi için önceki bölümdeki tabloya bakın. 

   Event Grid ile IOT hub'ı tümleştirme daha fazla uç noktaları destekliyor. Bir Azure hizmeti ekosistem dışında ve üçüncü taraf iş uygulamalarına yönlendirme genişletmek için Web kancaları ile de çalışır. 

* **Verilerinizi sırayla geldiğinde, önemli mi?**

   IOT Hub ileti yönlendirme, böylece aynı şekilde ulaştıkları iletilerin gönderildiği sırada tutar.

   Event Grid, uç noktalar aynı sırada, gerçekleşen olayları alacaksınız garanti etmez. Ancak, olay şeması uç noktasında olayları geldikten sonra sırasını belirlemek için kullanılan bir zaman damgası içerir. 

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [IOT Hub ileti yönlendirme](iot-hub-devguide-messages-d2c.md) ve [IOT Hub uç noktaları](iot-hub-devguide-endpoints.md).
* [Azure Event Grid](../event-grid/overview.md) hakkında daha fazla bilgi edinin.
* İleti yollarını nasıl oluşturulacağını öğrenmek için bkz: [yolları kullanarak işlem IOT Hub CİHAZDAN buluta iletileri](../iot-hub/tutorial-routing.md) öğretici.
* Event Grid tümleştirmesi denemek [Logic Apps kullanarak Azure IOT Hub olayları hakkında e-posta bildirimleri gönderme](../event-grid/publish-iot-hub-events-to-logic-apps.md).