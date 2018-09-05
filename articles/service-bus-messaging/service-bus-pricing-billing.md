---
title: Fiyatlandırma ve faturalama Service Bus | Microsoft Docs
description: Service Bus fiyatlandırma yapısı genel bakış.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 7c45b112-e911-45ab-9203-a2e5abccd6e0
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/21/2017
ms.author: spelluru
ms.openlocfilehash: 9f899afef175afa2509dc60e0920dc387f8a7c5e
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43702570"
---
# <a name="service-bus-pricing-and-billing"></a>Fiyatlandırma ve faturalama Service Bus

Azure Service Bus standart olarak sunulur ve [Premium](service-bus-premium-messaging.md) katmanları. Oluşturduğunuz her Service Bus hizmeti ad alanı için bir hizmet katmanı seçebilirsiniz ve o ad alanı içinde oluşturulan tüm varlıklar arasında bu katmanını uygular.

> [!NOTE]
> Geçerli Service Bus fiyatlandırması hakkında ayrıntılı bilgi için bkz. [Azure Service Bus fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/service-bus/)ve [Service Bus SSS Sayfasındaki](service-bus-faq.md#pricing).
>
>

Service Bus, kuyruklar ve konular/abonelikler için aşağıdaki 2 ölçümler kullanır:

1. **Mesajlaşma işlemleri**: kuyruk veya konu/Abonelik Hizmeti uç noktalarına karşı API çağrıları olarak tanımlanır. Bu ölçüm, kuyruklar ve konular/abonelikler için Faturalanabilir kullanım birincil birimi olarak alınan veya gönderilen iletiler yerine geçer.
2. **Aracılı bağlantı**: Kuyruklar, konular veya Aboneliklerde bir belirli bir saatlik örnekleme süresi açık kalıcı bağlantılar en yüksek sayısı olarak tanımlanmış. Bu ölçüm yalnızca standart katmanda, ek bağlantılar içinde açabileceğiniz uygular (daha önce bağlantı kuyruk/konu/abonelik başına 100 sınırlı) bağlantı başına nominal bir ücret karşılığında.

**Standart** katmanı, kuyruklar ve konular/abonelikler, indirim % 80 ' en yüksek kullanım düzeylerinde hacim tabanlı výsledek ile gerçekleştirilen işlemleri için kademeli fiyatlandırma kullanıma sunuluyor. Aylık 10 ABD Doları 12,5 milyon işlem başına ek ücret ödemeden aylık gerçekleştirmenize olanak sağlayan, bir standart katman taban ücreti yoktur.

**Premium** katmanı, her müşterinin iş yükü yalıtımlı şekilde çalışır. böylece CPU ve bellek katmanında kaynak yalıtımına sağlar. Bu kaynak kapsayıcısı *mesajlaşma birimi* olarak adlandırılır. Her premium ad alanı, en az bir mesajlaşma birimi için ayrılmıştır. Her Service Bus Premium ad alanı için 1, 2 veya 4 mesajlaşma birimi satın alabilirsiniz. Tek bir iş yükü veya varlık, birden çok mesajlaşma birimine yayılabilir ve faturalandırma 24 saatlik veya günlük oran fiyatlarında gerçekleştirilse de mesajlaşma birimlerinin sayısı isteğe bağlı olarak değiştirilebilir. Sonuç olarak, Service Bus tabanlı çözümünüz için tahmin edilebilir ve tekrarlanabilir bir performans elde edersiniz. Daha tahmin edilebilir ve kullanılabilir olmasının yanı sıra bu performans, daha hızlıdır.

> [!NOTE]
> Konuları ve abonelikleri yalnızca standart veya Premium ücretlendirme katmanları mevcuttur; Temel katman yalnızca kuyrukları destekler.

Standart katman taban ücreti, Azure aboneliği başına ayda yalnızca bir kez ücretlendirilir. Bu, tek ve standart katman hizmet veri yolu ad alanı oluşturduktan sonra ek taban ücret ödemeden, aynı Azure aboneliği altında istediğiniz kadar ek standart ad alanı oluşturabileceğiniz anlamına gelir.

[Service Bus fiyatlandırma](https://azure.microsoft.com/pricing/details/service-bus/) tabloda, standart ve Premium Katmanlar arasındaki işlevsel farklılıklar özetlenmiştir.

## <a name="messaging-operations"></a>Mesajlaşma işlemleri

Kuyruklar ve konular/abonelikler, "işlem," ileti başına faturalandırılır. Bir işlem, bir kuyruk veya konu/Abonelik Hizmeti uç noktasına karşı yapılan her API çağrısı anlamına gelir. Bu işlemlere yönetim, gönderme/alma ve oturum durumu işlemleri dahildir.

| İşlem Türü | Açıklama |
| --- | --- |
| Yönetim |Okuma, güncelleştirme, silme (CRUD) kuyruklar veya konular/aboneliklere karşı oluşturma. |
| Mesajlaşma |Kuyruklar veya konular/abonelikler ile ileti gönderip yeniden açın. |
| Oturum durumu |Alın veya bir kuyruk veya konu/abonelik üzerinde oturum durumunu ayarlayın. |

Maliyet ayrıntıları için listelenen fiyatlarını görmek [Service Bus fiyatlandırma](https://azure.microsoft.com/pricing/details/service-bus/) sayfası.

## <a name="brokered-connections"></a>Aracılı bağlantılar

*Aracılı bağlantı* "kalıcı olarak bağlı" Gönderenler/alıcılar kuyruklar, konular veya abonelikler karşı çok sayıda ilgili kullanım düzenlerini karşılamak. Kalıcı olarak bağlı Gönderenler/alıcılar, bu, bir sıfır ile AMQP veya HTTP kullanarak bağlantı zaman aşımı (örneğin, HTTP uzun yoklama) alma ' dir. HTTP göndericiler ve alıcılar hemen bir zaman aşımı ile aracılı bağlantı oluşturmaz.

Bağlantı kotaları ve diğer hizmet sınırlamaları için bkz: [Service Bus kotaları](service-bus-quotas.md) makalesi. Aracılı bağlantılar hakkında daha fazla bilgi için bkz. [SSS](#faq) bu makalenin devamındaki bölümü.

Standart katman ad alanı başına aracılı bağlantı sınırını kaldırır ve Azure aboneliği birleşik aracılı bağlantı kullanım sayar. Daha fazla bilgi için [aracılı bağlantı](https://azure.microsoft.com/pricing/details/service-bus/) tablo.

> [!NOTE]
> 1.000 aracılı bağlantı (temel ücretlendirme üzerinden) standart Mesajlaşma katmanına dahildir ve tüm kuyrukları, konular ve abonelikler ve ilişkili Azure aboneliği dahilindeki paylaşılabilir.
>
>

<br />

> [!NOTE]
> Faturalandırma, en yüksek eş zamanlı bağlantı sayısına göre yapılır ve ayda 744 saate göre eşit oranda dağıtılır.
>
>

### <a name="premium-tier"></a>Premium Katman

Aracı bağlantılar Premium katmanda ücretlendirilmez.

## <a name="faq"></a>SSS

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Aracılı bağlantılar nedir ve nasıl kendileri için ücretlendirilirim?

Bir aracılı bağlantı aşağıdakilerden biri olarak tanımlanır:

1. Bir istemci bir Service Bus kuyruk veya konu/abonelik için bir AMQP bağlantısı.
2. Sıfırdan büyük alma zaman aşımı değerine sahip bir Hizmet Veri Yolu konusundan veya kuyruktan mesaj almaya yönelik HTTP çağrısı.

Service Bus en yüksek dahil edilen miktarı (standart katmanında 1000) aşan eşzamanlı aracılı bağlantı sayısını ücretlendirir. En yüksek sayılar saatlik olarak ölçülür, ayda 744 saat eşit olarak dağıtılır ve aylık faturalandırma dönemi boyunca toplanır. Dahil edilen miktar (ayda 1.000 aracılı bağlantı), eşit olarak dağıtılan saatlik en yüksek sayıların toplamına göre faturalandırma döneminin sonunda uygulanır.

Örneğin:

1. Her 10.000 cihaz, tek bir AMQP bağlantısı aracılığıyla bağlanır ve bir hizmet veri yolu konusu'ndan komutlar alır. Cihazları, bir olay Hub'ına telemetri olayları gönderir. Tüm cihazlar her gün 12 saat boyunca bağlanıyorsa şu bağlantı ücretini (diğer Service Bus konu ücretlerine ek olarak) uygulama: 10.000 bağlantı * 12 saat * 31 gün / 744 = 5.000 aracılı bağlantı. 1.000 aracılı bağlantı içeren aylık kullanım sonra 120 $ toplam aracılı bağlantı başına $0.03 oranı üzerinden 4.000 aracılı bağlantı için ücretlendirilirsiniz.
2. 10.000 cihaz, sıfır olmayan bir zaman aşımı belirterek HTTP aracılığıyla hizmet veri yolu kuyruğu'ndan mesajlar alır. Tüm cihazlar her gün 12 saat boyunca bağlanırsa, (diğer Service Bus ücretlerine ek olarak) aşağıdaki bağlantı ücretlerini görürsünüz: 10.000 HTTP alma bağlantı * günde 12 saat * 31 gün / 744 saat = 5.000 aracılı bağlantı.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Aracılı bağlantı ücretleri kuyruklara ve konulara/aboneliklere uygulanır mı?

Evet. Gönderen sistem veya cihazların sayısı ne olursa olsun, HTTP kullanarak olay göndermeye ilişkin herhangi bir bağlantı ücreti yoktur. Bazen "uzun yoklama" olarak adlandırılan sıfırdan büyük bir zaman aşımı kullanan HTTP'ye sahip olayları almak, aracılı bağlantı ücretleri oluşturur. AMQP bağlantıları, bağlantıların gönderme veya alma için kullanılıp kullanılmadığından bağımsız olarak aracılı bağlantı ücretleri alınmasına neden olur. Bir Azure aboneliğindeki tüm standart ad alanları üzerindeki ilk 1.000 aracılı bağlantı ek ücret olmadan (taban ücretinin) dahil edilir. Bu izinler birçok hizmetten hizmete Mesajlaşma senaryosunu için yeterli olduğundan, aracılı bağlantı ücretleri genellikle sadece çok sayıda istemci ile AMQP veya HTTP uzun yoklaması kullanmayı planlıyorsanız, ilgili hale gelir; Örneğin, daha verimli olay akışı elde etmek veya çok sayıda cihaz veya uygulama örneği ile iki yönlü iletişim sağlamak için.

## <a name="next-steps"></a>Sonraki adımlar

* Service Bus fiyatlandırması hakkında tüm ayrıntılar için bkz. [Service Bus fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/service-bus/).
* Bkz: [Service Bus SSS Sayfasındaki](service-bus-faq.md#pricing) için fiyatlandırma ve faturalama Service bus hakkında bazı sık karşılaşılan SSS.

[Azure portal]: https://portal.azure.com
