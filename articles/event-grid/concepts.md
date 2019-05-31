---
title: Azure Event Grid kavramları
description: Azure Event Grid ve kavramlarını açıklar. Event Grid anahtar çeşitli bileşenlerinin tanımlar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: spelluru
ms.openlocfilehash: 0821c749a6cb718e1b8abb74a2925bc041850eaf
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66305264"
---
# <a name="concepts-in-azure-event-grid"></a>Azure Event Grid kavramları

Bu makalede, Azure Event Grid içindeki ana kavramlar açıklanır.

## <a name="events"></a>Olaylar

Sistemde gerçekleşen bir şey tam olarak açıklayan bilgileri en az miktarda bir olaydır. Her bir olay gibi genel bilgiler yok: olayının kaynağı, olay yerinde ve benzersiz tanımlayıcı geçen süre. Her olay, yalnızca belirli olay türü için uygun olan belirli bilgileri de vardır. Örneğin, bir Azure depolamada oluşturulan yeni bir dosya ile ilgili ayrıntıları dosya hakkında gibi olayda `lastTimeModified` değeri. Veya, Event Hubs olay yakalama dosyanın URL'si. 

Bir olay boyutu en fazla 64 KB, genel kullanılabilirlik (GA) hizmet düzeyi sözleşmesi (SLA) tarafından alınmıştır. Bir olay boyutu en fazla desteği 1 MB şu anda Önizleme aşamasındadır. Olaylar üzerinde 64 KB, 64 KB'lık artışlarla ücretlendirilir. 


Bir olayı gönderilir özelliklerini görmek [Azure Event Grid olay şeması](event-schema.md).

## <a name="publishers"></a>Yayımcılar

Bir kullanıcı veya olayları Event Grid'e göndermek için karar kuruluş yayımcısıdır. Microsoft, çeşitli Azure Hizmetleri için olayları yayımlar. Kendi uygulamanızı olayları yayımlayabilir. Azure dışındaki hizmetleri barındıran kuruluşlar için Event Grid aracılığıyla olayları yayımlayabilir.

## <a name="event-sources"></a>Olay kaynakları

Burada olay gerçekleştiğinde bir olay kaynağıdır. Her bir olay kaynağı, bir veya daha fazla olay türlerini ilgilidir. Örneğin, Azure depolama blob oluşturulan olayları için olay kaynağı ' dir. IOT Hub cihaz tarafından oluşturulan olayları için olay kaynağı ' dir. Uygulamanızı tanımlayan özel olayları için olay kaynağı ' dir. Olay kaynakları olayları Event Grid'e göndermek için sorumlu olursunuz.

Desteklenen Event Grid kaynaklarından herhangi biri uygulama hakkında daha fazla bilgi için bkz: [Azure Event Grid olay kaynaklarında](event-sources.md).

## <a name="topics"></a>Konu başlıkları

Olay Kılavuzu konusunu burada kaynak olayları gönderen bir uç nokta sağlar. Yayımcı olay Kılavuzu konusu oluşturur ve bir olay kaynağı bir konu veya konu birden fazla gerekip gerekmediğini belirler. Bir konu koleksiyonunu ilgili olaylar için kullanılır. Belirli olay türleri için yanıt vermek için abone olma konuları aboneleri karar verin.

Sistem konular, Azure Hizmetleri tarafından sağlanan yerleşik konulardır. Azure aboneliğinizde yayımcı konulara sahip, ancak bunlara abone olabilir çünkü sistem konuları görmüyorum. Abone olmak için kaynak, olaylarından almak istediğiniz bilgileri sağlayın. Kaynağa erişimi olduğu sürece, kendi olaylarına abone olabilirsiniz.

Özel konular şunlardır: uygulama ve üçüncü taraf konuları. Oluşturduğunuzda veya bir özel konu için okuma erişimi atanan aboneliğinizde özel konuya bakın.

Uygulamanızı tasarlarken oluşturmak için ne kadar konunun verirken esnekliğine sahipsiniz. Büyük çözümler için ilgili olay her kategori için özel bir konu oluşturun. Örneğin, sipariş işleme ve kullanıcı hesaplarını değiştirme ile ilgili olayları gönderen bir uygulama düşünün. Her iki olayların kategorilerini herhangi bir olay işleyici istediği düşüktür. İki özel konu oluşturma ve onları ilgilendiren bir abone olay işleyicileri sağlar. Küçük çözümler için tüm olayları tek bir konu başlığına göndermeyi tercih edebilirsiniz. Etkinlik abonelerinden istedikleri olay türleri için filtre uygulayabilirsiniz.

## <a name="event-subscriptions"></a>Olay abonelikleri

Abonelik alma içinde ilginizi çeken bir konuya hangi olayların Event Grid söyler. Abonelik oluştururken, olayı işlemek için bir uç nokta sağlar. Uç noktasına gönderilen olaylar için filtre uygulayabilirsiniz. Olay türü veya konu deseni filtreleyebilirsiniz. Daha fazla bilgi için [Event Grid aboneliği şema](subscription-creation-schema.md).

Abonelik oluşturma örnekler için bkz:

* [Event Grid için Azure CLI örnekleri](cli-samples.md)
* [Event Grid için Azure PowerShell örnekleri](powershell-samples.md)
* [Event Grid için Azure Resource Manager şablonları](template-samples.md)

Geçerli olay Kılavuzu abonelikleri alma hakkında daha fazla bilgi için bkz: [Event Grid aboneliklerini sorgulama](query-event-subscriptions.md).

## <a name="event-subscription-expiration"></a>Olay aboneliği süre sonu
Olay aboneliğinin süresi bu tarihte otomatik olarak sona erer. Sınırlı bir süre için yalnızca gerekli olay aboneliklerine yönelik bir sona erme süresini ayarlayabilir ve bu Aboneliklerdeki temizleme hakkında endişelenmeye gerek istemiyorum. Örneğin, bir senaryoyu test etmek için bir olay aboneliği oluşturulurken bir sona erme süresini ayarlayabilir isteyebilirsiniz. 

Bir süre sonu ayarlama örneği için bkz: [Gelişmiş Filtreler ile abone ol](how-to-filter-events.md#subscribe-with-advanced-filters).

## <a name="event-handlers"></a>Olay işleyicileri

Event Grid açısından bakıldığında, bir olay işleyicisi, burada olay gönderilir yerdir. Olayı işlemek için bazı başka bir eylem işleyici alır. Event Grid, çeşitli işleyici türlerini destekler. Desteklenen bir Azure hizmeti veya kendi Web kancası işleyicisi olarak kullanabilirsiniz. İşleyici türüne bağlı olarak, Event Grid olay teslimini garanti etmek için farklı mekanizmalar izler. HTTP Web kancası olay işleyicileri için olay işleyicisi durum kodu dönene kadar yeniden denenir `200 – OK`. Kuyruk hizmeti başarıyla kuyruğa ileti gönderme işlemleri kadar için Azure depolama kuyruğu, olayları yeniden denenir.

Tüm desteklenen Event Grid işleyicilerin uygulanması hakkında daha fazla bilgi için bkz: [Azure Event Grid olay işleyicileri](event-handlers.md).

## <a name="security"></a>Güvenlik

Olay Kılavuzu konularına abone olma ve konuları yayımlama için güvenlik sağlar. Abone olurken, kaynak veya olay Kılavuzu konu üzerinde yeterli izinleri olmalıdır. Yayımlama sırasında bir SAS belirteci veya anahtar kimlik doğrulaması için konu olmalıdır. Daha fazla bilgi için [Event Grid güvenliğini ve kimlik doğrulaması](security-authentication.md).

## <a name="event-delivery"></a>Olay teslimi

Event Grid olay abonenin uç noktası tarafından alındı doğrulayamazsa olay redelivers. Daha fazla bilgi için [Event Grid iletiyi teslim ve yeniden deneme](delivery-and-retry.md).

## <a name="batching"></a>Toplu İşleme

Özel bir konu kullanırken, olayları bir dizide her zaman yayımlanması gerekir. Düşük aktarım hızı senaryoları, Bununla birlikte, yüksek hacimli için kullanım örnekleri için bu bir toplu olabilir, birçok batch önerilir başına birlikte olayları yayımlamak daha yüksek verimlilik elde edin. Toplu iş 1 MB'a kadar olabilir. Her olay hala 64 KB'lık (Genel kullanım) veya 1 MB (Önizleme) büyük olmamalıdır.

> [!NOTE]
> Bir olay boyutu en fazla 64 KB, genel kullanılabilirlik (GA) hizmet düzeyi sözleşmesi (SLA) tarafından alınmıştır. Bir olay boyutu en fazla desteği 1 MB şu anda Önizleme aşamasındadır. Olaylar üzerinde 64 KB, 64 KB'lık artışlarla ücretlendirilir. 

## <a name="next-steps"></a>Sonraki adımlar

* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid ile hızla çalışmaya başlamak için bkz: [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md).
