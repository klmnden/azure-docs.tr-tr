---
title: Azure olay kılavuz kavramları
description: Azure Event Grid ve kavramlarını açıklar. Olay kılavuzunun birkaç anahtar bileşenleri tanımlar.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/23/2018
ms.author: tomfitz
ms.openlocfilehash: abc1302f0317c8d5ecdc7ddaf8ca6d3a9e82b582
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626044"
---
# <a name="concepts-in-azure-event-grid"></a>Azure Event kılavuzunda kavramları

Bu makalede Azure olay kılavuzunda ana kavramlar açıklanmaktadır.

## <a name="events"></a>Olaylar

Bir olay sistem içinde gerçekleşen tam olarak bir şey açıklayan bilgileri küçük miktarıdır. Her olay gibi genel bilgiler vardır: olay kaynağı olay yerinde ve benzersiz tanımlayıcı geçen süre. Her bir olay da yalnızca belirli olay türü için uygun olan belirli bilgileri yok. Örneğin, bir Azure depolama alanında oluşturulan yeni bir dosya ile ilgili ayrıntıları dosya hakkında gibi olayında `lastTimeModified` değeri. Veya, bir Event Hubs olay yakalama dosyası URL'si. 

Her olay verilerinin 64 KB ile sınırlıdır.

Olayda gönderilen özelliklerini görmek [Azure olay kılavuz olay şema](event-schema.md).

## <a name="publishers"></a>Yayımcılar

Yayımcı, olayları olay kılavuza göndermek için karar kuruluş ya da kullanıcı ' dir. Microsoft, olaylar için çeşitli Azure hizmetlerine yayımlar. Kendi uygulamanızı olayları yayımlayabilirsiniz. Azure dışında hizmetlerini barındırmak kuruluşlar olayları olay kılavuz üzerinden yayımlayabilirsiniz.

## <a name="event-sources"></a>Olay kaynakları

Burada olay gerçekleştiğinde bir olay kaynağıdır. Her olay kaynağı, bir veya daha fazla olay türlerini ilişkilidir. Örneğin, Azure Storage blobu olayları oluşturulan olay kaynağıdır. IOT Hub cihaz olayları oluşturulan olay kaynağıdır. Tanımladığınız özel olaylar için olay kaynağı, uygulamasıdır. Olay kaynakları olay kılavuza olayları göndermek için sorumludur.

Herhangi bir desteklenen olay kılavuz kaynağının uygulama hakkında daha fazla bilgi için bkz: [olay kaynakları Azure olay kılavuzunda](event-sources.md).

## <a name="topics"></a>Konu başlıkları

Olay kılavuz konusu burada kaynak olaylar gönderir bir uç nokta sağlar. Yayımcı olay kılavuz konusu oluşturur ve bir olay kaynağı bir konu veya birden fazla konu gerekip gerekmediğini belirler. Bir konu ilgili olayların bir koleksiyon için kullanılır. Belirli olay türleri için yanıt vermek için abone olmak için hangi konuları aboneleri karar verin.

Sistem konular, Azure Hizmetleri tarafından sağlanan yerleşik konulardır. Yayımcı konular sahip, ancak bunlara abone olabilir çünkü Azure aboneliğinizde sistem konuları görmüyorum. Abone olmak için olayları almak istediğiniz kaynak hakkında bilgi sağlar. Kaynak erişim sahibi sürece olaylarına için abone olabilirsiniz.

Uygulama ve üçüncü taraf konuları bunun özel konulardır. Oluşturduğunuzda ya da erişim için özel bir konu atanan aboneliğinizde bu özel konusuna bakın.

Uygulamanızı tasarlarken oluşturmak için kaç tane konuları karar verirken esneklik vardır. Büyük çözümler için ilgili olayların her kategori için özel bir konu oluşturun. Örneğin, sipariş işleme ve kullanıcı hesaplarını değiştirme ile ilgili olayları gönderen bir uygulama göz önünde bulundurun. Her iki kategorileri olayların herhangi bir olay işleyicisini istediği düşüktür. İki özel konular oluşturmak ve bunları ilgilendiğiniz bir abone olay işleyicileri izin verin. Küçük çözümleri için tek bir konu tüm olayları göndermek tercih edebilirsiniz. Olay aboneleri istedikleri olay türleri için filtre uygulayabilirsiniz.

## <a name="event-subscriptions"></a>Olay abonelikleri

Bir abonelik olay kılavuz içinde alma ilgilenen bir konuda hangi olayların söyler. Abonelik oluştururken olayını işlemek için bir uç nokta sağlar. Uç noktasına gönderilen olaylar için filtre uygulayabilirsiniz. Olay türü ya da konu düzeni göre filtreleyebilirsiniz. Daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).

Abonelik oluşturma örnekler için bkz:

* [Olay kılavuz için Azure CLI örnekleri](cli-samples.md)
* [Olay kılavuz Azure PowerShell örnekleri](powershell-samples.md)
* [Olay kılavuz için Azure Resource Manager şablonları](template-samples.md)

Kılavuz abonelikler, geçerli olay alma hakkında daha fazla bilgi için bkz: [sorgu olay kılavuzuna abonelikleri](query-event-subscriptions.md).

## <a name="event-handlers"></a>Olay işleyicileri

Bir olay kılavuz açısından bakıldığında, olay işleyici burada olayı gönderilir yerdir. İşleyicisi olayını işlemek için bazı başka bir eylem alır. Olay kılavuz birden çok işleyici türlerini destekler. Desteklenen bir Azure hizmeti ya da kendi Web kancası işleyici olarak kullanabilirsiniz. İşleyici türüne bağlı olarak, olay kılavuz olay teslimini garanti etmek için farklı mekanizmaları izler. Bir durum kodu, işleyici dönene kadar HTTP Web kancası olay işleyicileri için olay denenir `200 – OK`. Sıra hizmeti başarıyla kuyruğa ileti gönderme işlem kadar için Azure depolama kuyruğu, olayları denenir.

Desteklenen olay kılavuz işleyicileri hiçbirini uygulama hakkında daha fazla bilgi için bkz: [olay işleyicileri Azure olay kılavuzunda](event-handlers.md).

## <a name="security"></a>Güvenlik

Olay kılavuz konularına abone olma ve konuları yayımlamak için güvenlik sağlar. Abone olduğunda, kaynak veya olay kılavuz konusu yeterli izinleri olmalıdır. Yayımlarken, konu için anahtar kimlik doğrulaması ya da SAS belirteci olması gerekir. Daha fazla bilgi için bkz: [olay kılavuz güvenlik ve kimlik doğrulama](security-authentication.md).

## <a name="event-delivery"></a>Olay teslimi

Olay kılavuz olaya abonenin bitiş noktası tarafından alındı doğrulayamıyorsanız, olay redelivers. Daha fazla bilgi için bkz: [olay kılavuz ileti teslimi ve Yeniden Dene'yi](delivery-and-retry.md).

## <a name="next-steps"></a>Sonraki adımlar

* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Hızlı bir şekilde olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).
