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
ms.openlocfilehash: ccbd861c985e54a3808c0d4e8ea6169b6a61f134
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="concepts-in-azure-event-grid"></a>Azure Event kılavuzunda kavramları

Azure olay kılavuzunda ana kavramları şunlardır:

## <a name="events"></a>Olaylar

Bir olay sistem içinde gerçekleşen tam olarak bir şey açıklayan bilgileri küçük miktarıdır.  Her olay gibi genel bilgiler vardır: olay kaynağı olay yerinde ve benzersiz tanımlayıcı geçen süre.  Her bir olay da yalnızca belirli olay türü için uygun olan belirli bilgileri yok. Örneğin, Azure depolama alanında oluşturulan yeni bir dosya ile ilgili bir olay ayrıntıları dosyayla ilgili gibi içeriyor `lastTimeModified` değeri. Veya bir sanal makine yeniden başlatılıyor hakkında bir olay sanal makine ve yeniden başlatma nedenini adını içerir. Her olay verilerinin 64 KB ile sınırlıdır.

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

Olay kılavuz konularına abone olma ve konuları yayımlamak için güvenlik sağlar. Abone olduğunda, kaynak veya konu yeterli izinleriniz olmalıdır. Yayımlarken, konu için anahtar kimlik doğrulaması ya da SAS belirteci olması gerekir. Daha fazla bilgi için bkz: [olay kılavuz güvenlik ve kimlik doğrulama](security-authentication.md).

## <a name="failed-delivery"></a>Başarısız teslim

Olay kılavuz bulunamıyor doğrulayın olaya abonenin bitiş noktası tarafından alındı, olay redelivers. Daha fazla bilgi için bkz: [olay kılavuz ileti teslimi ve Yeniden Dene'yi](delivery-and-retry.md).

## <a name="next-steps"></a>Sonraki adımlar

* Olay kılavuz giriş için bkz: [hakkında olay kılavuz](overview.md).
* Hızlı bir şekilde olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).
