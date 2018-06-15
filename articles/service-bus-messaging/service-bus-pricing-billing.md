---
title: Fiyatlandırma ve faturalama Service Bus | Microsoft Docs
description: Hizmet veri yolu fiyatlandırma yapısına genel bakış.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 7c45b112-e911-45ab-9203-a2e5abccd6e0
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/21/2017
ms.author: sethm
ms.openlocfilehash: 8ccb44b5009588c28bc79bb45e1a7640ead6c817
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
ms.locfileid: "27159795"
---
# <a name="service-bus-pricing-and-billing"></a>Service Bus fiyatlandırma ve faturalama

Azure Service Bus standart olarak sunulan ve [Premium](service-bus-premium-messaging.md) katmanları. Oluşturduğunuz her Service Bus hizmeti ad alanı için bir hizmet katmanına seçebilir ve bu ad alanı içinde oluşturulan tüm varlıklar arasında bu katmanı seçimi uygular.

> [!NOTE]
> Hizmet veri yolu geçerli fiyatlandırma hakkında ayrıntılı bilgi için bkz: [Azure Service Bus fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/service-bus/)ve [Service Bus SSS](service-bus-faq.md#pricing).
>
>

Service Bus, kuyruklar ve konular/abonelikler için aşağıdaki 2 metre kullanır:

1. **Operations Mesajlaşma**: API çağrıları kuyruk veya konu başlığının/aboneliğinin hizmet uç noktalarına karşı olarak tanımlanır. Bu ölçüm, gönderilen veya Faturalanabilir kullanım kuyruklar ve konular/abonelikler için birincil birimi olarak alınan iletiler yerini alır.
2. **Aracılı bağlantı**: sıraları, konuları ve abonelikleri karşı belirli bir saatlik örnekleme süresi boyunca açık kalıcı bağlantılar en yüksek sayısı olarak tanımlanmış. Bu ölçüm yalnızca içinde açabilirsiniz ek bağlantılar standart katmanında uygulanır (daha önce bağlantıları konu/kuyruk/abonelik başına 100 sınırlı) bir nominal bağlantı başına ücret karşılığında.

**Standart** kuyruklar ve konular/abonelikler, birim tabanlı indirimleri % 80 en yüksek kullanım düzeyleri, bunun sonucunda ile gerçekleştirilen işlemleri için derecelendirilmiş fiyatlandırma katmanı tanıtır. 10 aylık hiçbir ek ücret ödemeden aylık 12,5 milyon işlemler gerçekleştirmenizi sağlayan, bir standart katmanı temel ücret yoktur.

**Premium** katmanı sağlar CPU ve bellek katmanında kaynak yalıtımına böylece her müşterinin iş yükü yalıtımlı şekilde çalışır. Bu kaynak kapsayıcısı *mesajlaşma birimi* olarak adlandırılır. Her premium ad alanı, en az bir mesajlaşma birimi için ayrılmıştır. Her Service Bus Premium ad alanı için 1, 2 veya 4 mesajlaşma birimi satın alabilirsiniz. Tek bir iş yükü veya varlık, birden çok mesajlaşma birimine yayılabilir ve faturalandırma 24 saatlik veya günlük oran fiyatlarında gerçekleştirilse de mesajlaşma birimlerinin sayısı isteğe bağlı olarak değiştirilebilir. Sonuç olarak, Service Bus tabanlı çözümünüz için tahmin edilebilir ve tekrarlanabilir bir performans elde edersiniz. Daha tahmin edilebilir ve kullanılabilir olmasının yanı sıra bu performans, daha hızlıdır.

Standart katmanı temel ücret aylık Azure abonelik başına yalnızca bir kez doludur unutmayın. Başka bir deyişle, tek bir standart katmanı Service Bus ad alanı oluşturduktan sonra ek temel ücretlerinin yansıtılmasını olmadan, aynı Azure abonelik altında istediğiniz sayıda ek standart ad alanları oluşturabilirsiniz.

[Service Bus fiyatlandırma](https://azure.microsoft.com/pricing/details/service-bus/) tablo standart ve Premium katmanlar arasında işlevsel farklılıklar özetler.

## <a name="messaging-operations"></a>Mesajlaşma işlemleri

Kuyruklar ve konular/abonelikler "işlemi" değil ileti başına faturalandırılır. Bir kuyruk veya konu başlığının/aboneliğinin hizmet uç noktası karşı yapılan herhangi bir API çağrısı bir işlem başvuruyor. Bu işlemlere yönetim, gönderme/alma ve oturum durumu işlemleri dahildir.

| İşlem Türü | Açıklama |
| --- | --- |
| Yönetim |Oluşturma, okuma, güncelleştirme, Sil (CRUD) sıralar veya konuları/abonelikleri karşı. |
| Mesajlaşma |İleti gönderme ve sıralar veya konuları/abonelikleri ile alma. |
| Oturum durumu |Alın veya oturum durumu bir kuyruk veya konu başlığının/aboneliğinin ayarlayın. |

Listelenen fiyatlar maliyet Ayrıntılar için bkz [Service Bus fiyatlandırma](https://azure.microsoft.com/pricing/details/service-bus/) sayfası.

## <a name="brokered-connections"></a>Aracılı bağlantılar

*Aracılı bağlantı* çok sayıda "kalıcı olarak bağlı" Gönderenler/alıcılar sıralar, konuları ve abonelikleri karşı ilgili kullanım desenlerini uyum sağlamak. Kalıcı olarak bağlı Gönderenler/alıcılar bir sıfır ile AMQP veya HTTP kullanarak bağlan o zaman aşımı (örneğin, HTTP uzun yoklama) alacak olan. HTTP göndericiler ile alıcılar hemen bir zaman aşımı ile aracılı bağlantılar oluşturmaz.

Bağlantı kotaları ve diğer hizmet sınırları için bkz: [Service Bus kotaları](service-bus-quotas.md) makalesi. Aracılı bağlantılar hakkında daha fazla bilgi için bkz: [SSS](#faq) bu makalenin sonraki bölümlerinde bölümü.

Standart katmanı başına ad alanı aracılı bağlantı sınırı kaldırır ve Azure abonelik üzerinden toplama aracılı bağlantı kullanım sayar. Daha fazla bilgi için bkz: [aracılı bağlantılar](https://azure.microsoft.com/pricing/details/service-bus/) tablo.

> [!NOTE]
> 1.000 aracılı bağlantılar (üzerinden temel ücret) standart Mesajlaşma katmanı dahil edilmiştir ve tüm kuyruklar, konular ve abonelikler ilişkili Azure abonelik içindeki arasında paylaşılabilir.
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

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Aracılı bağlantılar nelerdir ve nasıl ı kendileri için sizden ücret?

Bir aracılı bağlantı aşağıdakilerden biri olarak tanımlanır:

1. Bir istemci bir hizmet veri yolu kuyruğu ya da konu başlığının/aboneliğinin bir AMQP bağlantısı.
2. Sıfırdan büyük alma zaman aşımı değerine sahip bir Hizmet Veri Yolu konusundan veya kuyruktan mesaj almaya yönelik HTTP çağrısı.

Hizmet veri yolu giderleri (1.000 standart katmanındaki) dahil edilen miktar aşan eşzamanlı aracılı bağlantı en yüksek sayısı. En yüksek sayılar saatlik olarak ölçülür, ayda 744 saat eşit olarak dağıtılır ve aylık faturalandırma dönemi boyunca toplanır. Dahil edilen miktar (ayda 1.000 aracılı bağlantı), eşit olarak dağıtılan saatlik en yüksek sayıların toplamına göre faturalandırma döneminin sonunda uygulanır.

Örneğin:

1. Her 10.000 cihaz tek bir AMQP bağlantısı üzerinden bağlanır ve hizmet veri yolu konusundan komutlarını alır. Aygıtları telemetri olayları bir Event Hub'ına gönderin. Tüm aygıtlar için 12 saat her gün bağlanırsanız, aşağıdaki bağlantı ücretleri (tüm diğer Service Bus konu ücretleri ek olarak) uygulanır: 10.000 bağlantıları * 12 saat * 31 gün / 744 = 5.000 aracılı bağlantılar. Sonra aylık indirimi 1.000 aracılı bağlantı için 120 toplam aracılı bağlantı başına 0.03 hızında 4.000 aracılı bağlantılar sizden ücret.
2. 10.000 cihaz sıfır olmayan bir zaman aşımı belirten HTTP üzerinden Service Bus kuyruğuna iletileri alacak. Tüm aygıtlar için 12 saat her gün bağlanırsanız, aşağıdaki bağlantı giderleri (ek olarak tüm diğer Service Bus ücretleri) görürsünüz: 10.000 HTTP alan bağlantıları * 12 saat günde * 744 saatleri/31 gün = 5.000 aracılı bağlantılar.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Aracılı bağlantı ücretleri kuyruklara ve konulara/aboneliklere uygulanır mı?

Evet. Gönderen sistem veya cihazların sayısı ne olursa olsun, HTTP kullanarak olay göndermeye ilişkin herhangi bir bağlantı ücreti yoktur. Olaylar bazen "uzun yoklama," olarak da adlandırılır sıfırdan büyük bir zaman aşımı kullanarak HTTP ile alma aracılı bağlantı ücretleri oluşturur. AMQP bağlantıları, bağlantıların gönderme veya alma için kullanılıp kullanılmadığından bağımsız olarak aracılı bağlantı ücretleri alınmasına neden olur. Bir Azure aboneliği standart tüm ad alanlarını ilk 1.000 aracılı bağlantılarında (ötesinde temel ücret) Ekstra ücret ödemeden dahil edilir. Bu kesintileri birçok hizmet ileti senaryolarını kapsamak için yeterli olduğundan, aracılı bağlantı ücretleri genellikle yalnızca AMQP veya HTTP uzun yoklama çok sayıda istemci ile kullanmayı planlıyorsanız, ilgili hale gelir; Örneğin, olay daha verimli akış elde etmek veya sayıda cihaz veya uygulama örnekleri ile çift yönlü iletişimi etkinleştir.

## <a name="next-steps"></a>Sonraki adımlar

* Hizmet veri yolu fiyatlandırma hakkında tüm ayrıntılar için bkz: [Service Bus fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/service-bus/).
* Bkz: [Service Bus SSS](service-bus-faq.md#pricing) fiyatlandırma ve faturalama Service bus hakkında bazı sık sık sorulan sorular için.

[Azure portal]: https://portal.azure.com
