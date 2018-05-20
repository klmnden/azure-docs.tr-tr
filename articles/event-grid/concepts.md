---
title: Azure olay kılavuz kavramları
description: Azure olay kılavuz ve onun kavramlarını açıklar. Olay kılavuzunun birkaç anahtar bileşenleri tanımlar.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: babanisa
ms.openlocfilehash: fd82d163ba8407a3dfa088cd322f3e236be5d7ea
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="concepts-in-azure-event-grid"></a>Azure Event kılavuzunda kavramları

Bu makalede Azure olay kılavuzunda ana kavramlar açıklanmaktadır.

## <a name="events"></a>Olaylar

Bir olay sistem içinde gerçekleşen tam olarak bir şey açıklayan bilgileri küçük miktarıdır. Her olay gibi genel bilgiler vardır: olay kaynağı olay yerinde ve benzersiz tanımlayıcı geçen süre. Her bir olay da yalnızca belirli olay türü için uygun olan belirli bilgileri yok. Örneğin, bir Azure depolama alanında oluşturulan yeni bir dosya ile ilgili ayrıntıları dosya hakkında gibi olayında `lastTimeModified` değeri. Veya, bir Event Hubs olay yakalama dosyası URL'si. Her olay verilerinin 64 KB ile sınırlıdır.

## <a name="event-sourcespublishers"></a>Olay kaynakları/yayımcıları

Burada olay gerçekleştiğinde bir olay kaynağıdır. Örneğin, Azure Storage blobu olayları oluşturulan olay kaynağıdır. Azure VM doku sanal makine olayları için olay kaynağı ' dir. Olay kaynakları, olayları olay kılavuza yayımlamak için sorumludur.

Herhangi bir desteklenen olay kılavuz kaynağının uygulama hakkında daha fazla bilgi için bkz: [olay kaynakları Azure olay kılavuzunda](event-sources.md).

## <a name="topics"></a>Konu başlıkları

Yayımcıları olayları konulara kategorilere ayırma. Olay Kılavuzu konu, burada yayımcı olaylar gönderir bir uç nokta içerir. Belirli olay türleri için yanıt vermek için abone olmak için hangi konuları aboneleri karar verin. Böylece aboneleri olayları uygun şekilde kullanmak nasıl öğrenebilirsiniz konular da bir olay şeması sağlar.

Sistem konular, Azure Hizmetleri tarafından sağlanan yerleşik konulardır. Uygulama ve üçüncü taraf konuları bunun özel konulardır.

Uygulamanızı tasarlarken oluşturmak için kaç tane konuları karar verirken esneklik vardır. Büyük çözümler için ilgili olayların her kategori için özel bir konu oluşturun. Örneğin, sipariş işleme ve kullanıcı hesaplarını değiştirme ile ilgili olayları gönderen bir uygulama göz önünde bulundurun. Her iki kategorileri olayların herhangi bir olay işleyicisini istediği düşüktür. İki özel konular oluşturmak ve bunları ilgilendiğiniz bir abone olay işleyicileri izin verin. Küçük çözümleri için tek bir konu tüm olayları göndermek tercih edebilirsiniz. Olay aboneleri istedikleri olay türleri için filtre uygulayabilirsiniz.

## <a name="event-subscriptions"></a>Olay abonelikleri

Bir abonelik olay hangi olayların bir konuda üzerinde bir abone olarak alma ilgilenmektedir kılavuz bildirir. Bir abonelik de olayları aboneye nasıl teslim hakkında bilgi içerir.

## <a name="event-handlers"></a>Olay işleyicileri

Bir olay kılavuz açısından bakıldığında, olay işleyici burada olayı gönderilir yerdir. İşleyicisi olayını işlemek için bazı başka bir eylem alır. Olay kılavuz birden çok abone türlerini destekler. Abone türüne bağlı olarak, olay kılavuz olay teslimini garanti etmek için farklı mekanizmaları izler. Bir durum kodu, işleyici dönene kadar HTTP Web kancası olay işleyicileri için olay denenir `200 – OK`. Sıra hizmeti başarıyla kuyruğa ileti gönderme işlem kadar için Azure depolama kuyruğu, olayları denenir.

Desteklenen olay kılavuz işleyicileri hiçbirini uygulama hakkında daha fazla bilgi için bkz: [olay işleyicileri Azure olay kılavuzunda](event-handlers.md).

## <a name="filters"></a>Filtreler

Bir olay kılavuz konuya abone olurken uç noktasına gönderilen olaylar için filtre uygulayabilirsiniz. Olay türü ya da konu düzeni göre filtreleyebilirsiniz. Daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).

## <a name="security"></a>Güvenlik

Olay kılavuz konularına abone olma ve konuları yayımlamak için güvenlik sağlar. Abone olduğunda, kaynak veya olay kılavuz konusu yeterli izinleri olmalıdır. Yayımlarken, konu için anahtar kimlik doğrulaması ya da SAS belirteci olması gerekir. Daha fazla bilgi için bkz: [olay kılavuz güvenlik ve kimlik doğrulama](security-authentication.md).

## <a name="failed-delivery"></a>Başarısız teslim

Olay kılavuz olaya abonenin bitiş noktası tarafından alındı doğrulayamıyorsanız, olay redelivers. Daha fazla bilgi için bkz: [olay kılavuz ileti teslimi ve Yeniden Dene'yi](delivery-and-retry.md).

## <a name="next-steps"></a>Sonraki adımlar

* Olay kılavuz giriş için bkz: [hakkında olay kılavuz](overview.md).
* Hızlı bir şekilde olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).
