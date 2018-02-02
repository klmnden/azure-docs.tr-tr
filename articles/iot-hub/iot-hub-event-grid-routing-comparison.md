---
title: "Olay kılavuz, IOT Hub için yönlendirme karşılaştırma | Microsoft Docs"
description: "IOT hub'ı kendi ileti yönlendirme hizmeti sunar, ancak ayrıca olay yayımlama için olay kılavuz ile tümleştirilir. İki özellik karşılaştırın."
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.service: iot-hub
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: 5a0a97ccf033b2981ba13be455482146ba212228
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="compare-message-routing-and-event-grid-for-iot-hub"></a>İleti Yönlendirme ve olay kılavuz için IOT Hub ile karşılaştırın

Azure IOT Hub, bağlı aygıtlardan veri akışı ve bu verileri iş uygulamalarınıza tümleştirmek için yeteneği sağlar. IOT hub'ı diğer Azure hizmetlerini ve iş uygulamaları IOT olayları tümleştirmek için iki yöntem sunar. Hangi senaryonuz için en iyi seçenektir seçebilmeleri bu makalede bu olanağı sağlayan iki özellikleri açıklanmaktadır.

* **IOT hub'ı ileti yönlendirme**: Bu IOT hub'ı özelliği sağlar kullanıcılara [cihaz bulut iletilerini yönlendirmek](iot-hub-devguide-messages-read-custom.md) hizmet uç noktalarına Azure Storage kapsayıcıları, olay hub'ları, Service Bus kuyrukları ve Service Bus konu başlıkları gibi. Yönlendirme kuralları sorgu tabanlı yollar gerçekleştirmek için esneklik sağlar. Bunlar ayrıca etkinleştirmek [kritik uyarılar](iot-hub-devguide-messages-d2c.md) tetiklemek sorguları aracılığıyla Eylemler ve ileti üstbilgilerini ve gövde göre. 
* **Olay kılavuz ile IOT hub'ı tümleştirme**: Azure olay kılavuz hizmettir yayımlamayı kullanan bir tam olarak yönetilen olay yönlendirme-model abone olun. IOT Hub ve olay kılavuz çalışmasına birlikte [IOT hub'ı olayları Azure ve Azure dışı hizmetleriyle tümleştirme](iot-hub-event-grid.md), neredeyse gerçek zamanlı. 

## <a name="similarities-and-differences"></a>Benzerlikler ve farklar

İleti Yönlendirme ve olay kılavuz uyarı yapılandırma olanak sağlarken, ikisi arasındaki bazı farklar vardır. Ayrıntılar için aşağıdaki tabloya bakın:

| Özellik | IOT hub'ı ileti yönlendirme | Olay kılavuz ile IOT hub'ı tümleştirme |
| ------- | --------------- | ---------- |
| **Cihaz iletileri** | Evet, ileti yönlendirme telemetri verileri için kullanılabilir. | Hayır, olay kılavuz yalnızca telemetri olmayan IOT hub'ı olaylar için kullanılabilir. |
| **Olay türü** | Evet, ileti yönlendirme twin değişiklikleri ve cihaz yaşam döngüsü olayları bildirebilir. | Evet, olay kılavuz bir IOT Hub'ına kaydedilen aygıtlar ve aygıtlar silindiğinde bildirebilirsiniz. |
| **Sıralama** | Evet, olayları sıralama korunur.  | Hayır, olayların sırası garanti edilmez. | 
| **İleti boyutu üst sınırı** | 256 KB, cihaz bulut | 64 KB |
| **Filtreleme** | Zengin SQL benzeri bir dil filtreleme, ileti üstbilgilerini ve gövdeleri filtreleme destekler. Örnekler için bkz: [IOT hub'ı sorgu dili](iot-hub-devguide-query-language.md). | Filtreleme soneki/depolama gibi hiyerarşik hizmetler için iyi çalışan öneki cihaz kimlikleri, temel. |
| **Uç noktaları** | <ul><li>Olay Hub'ı</li> <li>Depolama blobu</li> <li>Service Bus kuyruğu</li> <li>Hizmet Veri Yolu konuları</li></ul><br>Ücretli IOT Hub SKU'ları (S1, S2 ve S3) 10 özel uç noktalar ile sınırlıdır. IOT Hub 100 yollar oluşturulabilir. | <ul><li>Azure İşlevleri</li> <li>Azure Otomasyonu</li> <li>Olay Hub'ı</li> <li>Logic Apps</li> <li>Microsoft Flow</li> <li>Üçüncü taraf hizmetleri aracılığıyla Web kancaları</li></ul><br>Uç noktalar en güncel listesi için bkz [olay kılavuz olay işleyicileri](../event-grid/overview.md#event-handlers). |
| **Maliyet** | İleti yönlendirme için ayrı bir ücret ödemeden yoktur. Yalnızca giriş telemetri IOT hub'ı içine ücretlendirilir. Örneğin, üç farklı uç noktalar yönlendirilmiş bir ileti varsa, tek bir ileti için faturalandırılır. | Ücretsiz IOT hub'dan yoktur. Olay kılavuz sunar ilk 100.000 işlemleri her ay ücretsiz olarak ve ardından $0.60 başına milyon işlemlerini bundan sonra. |

IOT hub'ı ileti yönlendirme ve olay kılavuz benzerlikler çok vardır, bazıları aşağıdaki tabloda açıklanmıştır:

| Özellik | IOT hub'ı ileti yönlendirme | Olay kılavuz ile IOT hub'ı tümleştirme |
| ------- | --------------- | ---------- |
| **Güvenilirlik** | Yüksek: her ileti en az bir kez her yol için uç noktaya sağlar. Bir saat içinde teslim edilmedi tüm iletilerin süresi dolar. | Yüksek: her ileti en az bir kez her abonelik için Web kancası sağlar. 24 saat içinde teslim edilmedi tüm olayları süresi dolar. | 
| **Ölçeklenebilirlik** | Yüksek: milyonlarca eş zamanlı cihazı iletileri milyarlarca gönderme desteklemek için en iyi duruma getirilmiş. | Yüksek: Bölge başına saniyede yönlendirme 10,000,000 olayların yeteneği. |
| **Gecikme süresi** | Düşük: Neredeyse gerçek zamanlı. | Düşük: Neredeyse gerçek zamanlı. |
| **Birden çok Uç noktalara Gönder** | Evet, birden çok uç nokta için tek bir ileti gönderin. | Evet, birden çok uç nokta için tek bir ileti gönderin.  | 
| **Güvenlik** | IOT Hub cihaz başına kimlik ve iptal edilebilir erişim denetimi sağlar. Daha fazla bilgi için bkz: [IOT hub'ı erişim denetimi](iot-hub-devguide-security.md). | Olay kılavuz üç noktalarda doğrulama sağlar: olay abonelikleri, olay yayımlama ve Web kancası olay teslimi. Daha fazla bilgi için bkz: [olay kılavuz güvenlik ve kimlik doğrulama](../event-grid/security-authentication.md). |

## <a name="how-to-choose"></a>Seçim yapma

IOT hub'ı ileti yönlendirme ve olay kılavuz ile IOT hub'ı tümleştirme benzer sonuçlar elde etmek için farklı eylemleri gerçekleştirin. Bunlar hem IOT hub'ı çözümünüzden bilgi alın ve diğer hizmetleri tepki verme onu geçirmek. Bu nedenle nasıl hangisinin kullanılacağını karar? Önceki bölümdeki verilere ek olarak, kararınızı yol göstermeye yardımcı olması için aşağıdaki soruları kullanın: 

* **Ne tür veriler Uç noktalara gönderiyor?**

   Diğer hizmetlere telemetri verileri göndermek gerektiğinde IOT hub'ı ileti yönlendirme kullanın. Ayrıca ileti üstbilgilerini ve İleti gövdeleri sorgulama etkinleştirir yönlendirme iletisi. 

   IOT hub'ı hizmetinde meydana gelen olayları olay kılavuz ile IOT hub'ı tümleştirme çalışır. Bu IOT hub'ı olayları cihaz oluşturma ve silme içerir. 

* **Bu bilgileri almak hangi uç noktaları gerekiyor?**

   IOT hub'ı ileti yönlendirme sınırlı uç noktalarını destekler, ancak verileri ve olayları ek uç noktaları için yeniden yönlendirme için bağlayıcıları oluşturabilirsiniz. Desteklenen uç noktaları tam listesi için önceki bölümde tabloya bakın. 

   IOT hub'ı tümleştirmeyi olay kılavuz ile daha fazla uç noktalarını destekler. Azure hizmet ekosistemi dışına ve üçüncü taraf iş uygulamalarına yönlendirme genişletmek için Web kancası ile de çalışır. 

* **Verilerinizi sırayla gelirse olmaması önemli?**

   Böylece aynı biçimde taramaması IOT hub'ı ileti yönlendirme iletilerinin gönderilme, sipariş tutar.

   Olay kılavuz uç noktaları bunlar oluştuğunu sırayla olayları alacak garanti etmez. Ancak, olay şema olayları uç noktada geldikten sonra sırayı tanımlamak için kullanılan bir zaman damgası içermez. 

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [IOT hub'ı ileti yönlendirme](iot-hub-devguide-messages-d2c.md) ve [IOT Hub uç noktaları](iot-hub-devguide-endpoints.md).

* Daha fazla bilgi edinmek [Azure olay kılavuz](../event-grid/overview.md)

* Olay kılavuz tümleştirmesi denemek [Logic Apps kullanarak Azure IOT Hub olayları hakkında e-posta bildirimleri gönderme](../event-grid/publish-iot-hub-events-to-logic-apps.md)