---
title: "Azure olay kılavuz kavramları"
description: "Azure olay kılavuz ve onun kavramlarını açıklar. Olay kılavuzunun birkaç anahtar bileşenleri tanımlar."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 09/18/2017
ms.author: babanisa
ms.openlocfilehash: 5b69478bf00284594b984fde452f6bed4e73859b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="concepts-in-azure-event-grid"></a>Azure Event kılavuzunda kavramları

Azure olay kılavuzunda ana kavramları şunlardır:

## <a name="events"></a>Olaylar

Bir olay sistem içinde gerçekleşen tam olarak bir şey açıklayan bilgileri küçük miktarıdır.  Her olay gibi genel bilgiler vardır: olay kaynağı olay yerinde ve benzersiz tanımlayıcı geçen süre.  Her olay da yalnızca belirli olay ilgili belirli bilgiler sahiptir. Örneğin, Azure depolama alanında oluşturulan yeni bir dosya ile ilgili bir olay lastTimeModified değeri gibi dosya hakkındaki ayrıntıları içerir. Veya bir sanal makine yeniden başlatılıyor hakkında bir olay sanal makine ve yeniden başlatma nedenini adını içerir. Her olay verilerinin 64 KB ile sınırlıdır.

## <a name="event-sourcespublishers"></a>Olay kaynakları/yayımcıları

Burada olay gerçekleştiğinde bir olay kaynağıdır. Örneğin, Azure Storage blobu olayları oluşturulan olay kaynağıdır. Azure VM doku sanal makine olayları için olay kaynağı ' dir. Olay kaynakları, olayları olay kılavuza yayımlamak için sorumludur.

## <a name="topics"></a>Konu başlıkları

Yayımcıları olayları konulara kategorilere ayırma. Konu, burada yayımcı olaylar gönderir bir uç nokta içerir. Belirli olay türleri için yanıt vermek için abone olmak için hangi konuları aboneleri karar verin. Böylece aboneleri olayları uygun şekilde kullanma bulabilir konular da bir olay şeması sağlar.

Sistem konular, Azure Hizmetleri tarafından sağlanan yerleşik konulardır. Uygulama ve üçüncü taraf konuları bunun özel konulardır.

## <a name="event-subscriptions"></a>Olay abonelikleri

Bir abonelik olay hangi olayların bir konuda üzerinde bir abone olarak alma ilgilenmektedir kılavuz bildirir.  Bir abonelik de olayları aboneye nasıl teslim hakkında bilgi içerir.

## <a name="event-handlers"></a>Olay işleyicileri

Bir olay kılavuz açısından bakıldığında, olay işleyici burada olayı gönderilir yerdir. İşleyicisi olayını işlemek için bazı başka bir eylem alır.  Olay kılavuz birden çok abone türlerini destekler. Abone türüne bağlı olarak, olay kılavuz olay teslimini garanti etmek için farklı mekanizmaları izler.  Bir durum kodu, işleyici dönene kadar HTTP Web kancası olay işleyicileri için olay denenir `200 – OK`. Sıra hizmeti başarıyla kuyruğa ileti gönderme işlem kadar için Azure depolama kuyruğu, olayları denenir.

## <a name="filters"></a>Filtreler

Bir konuya abone olurken uç noktasına gönderilen olaylar için filtre uygulayabilirsiniz. Olay türü ya da konu düzeni göre filtreleyebilirsiniz. Daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).

## <a name="security"></a>Güvenlik

Olay konularına abone olma ve konuları yayımlamak için güvenlik sağlar. Abone olduğunda, kaynak veya konu yeterli izinleriniz olmalıdır. Yayımlarken, konu için anahtar kimlik doğrulaması ya da SAS belirteci olması gerekir. Daha fazla bilgi için bkz: [olay kılavuz güvenlik ve kimlik doğrulama](security-authentication.md).

## <a name="failed-delivery"></a>Başarısız teslim

Olay kılavuz bulunamıyor doğrulayın olaya abonenin bitiş noktası tarafından alındı, olay redelivers. Daha fazla bilgi için bkz: [olay kılavuz ileti teslimi ve Yeniden Dene'yi](delivery-and-retry.md).

## <a name="next-steps"></a>Sonraki adımlar

* Olay kılavuz giriş için bkz: [hakkında olay kılavuz](overview.md).
* Hızlı bir şekilde olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).