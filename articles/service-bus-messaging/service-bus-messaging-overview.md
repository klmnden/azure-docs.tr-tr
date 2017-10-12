---
title: "Azure Service Bus mesajlaşmasına genel bakış | Microsoft Belgeleri"
description: "Service Bus mesajlaşması ve Azure Geçiş’in açıklaması"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 08/30/2017
ms.author: sethm
ms.openlocfilehash: 462fa0a6e86237b8c9d9ba97aa377f8d112a8b8e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Service Bus mesajlaşma hizmeti: bulutta esnek veri teslimatı

Microsoft Azure Service Bus, güvenilir bir bilgi teslimatı hizmetidir. Bu hizmetin amacı, iletişimi daha kolay hale getirmektir. İki veya daha fazla taraf bilgi değişimi gerçekleştirmek istediğinde bir iletişim kolaylaştırıcısına ihtiyaç duyarlar. Service Bus ise aracılı ya da üçüncü taraf iletişim mekanizmasıdır. Fiziksel dünyadaki posta hizmetine benzer şekilde işler. Posta hizmetleri, farklı türdeki mektupların ve paketlerin dünyanın her yanına çeşitli teslimat garantileriyle gönderilmesini oldukça kolaylaştırır.

Posta hizmetlerinin mektup teslimatlarına benzer şekilde, Service Bus hem alıcı hem de göndericilerden gelen esnek bilgi teslimatıdır. Mesajlaşma hizmeti, iki taraf aynı anda çevrimiçi veya tam olarak aynı anda kullanılabilir olmasalar da bilgilerin teslim edilmesini sağlar. Bu şekilde, mesajlaşma bir mektup göndermeye benzer; aracısız iletişim ise bir telefon araması gerçekleştirmeye (veya daha çok aracılı mesajlaşmaya benzeyen, çağrı bekletme ve arayan kimliğinden önceki telefon aramalarına) benzetilebilir.

Ayrıca, ileti alıcısı işlemler, yineleme algılaması, zamana bağlı süre sonu ve toplu işlem gerçekleştirme de dahil olmak üzere çeşitli teslimat özelliklerini ihtiyaç duyar. Bu düzenler de posta hizmetindekilerle benzerlik gösterir: tekrar teslimat, gerekli imza, adres değişikliği veya geri çağırma.

Service Bus, iki farklı mesajlaşma desenini destekler: *Azure Geçişi* ve *Service Bus Mesajlaşması*.

## <a name="azure-relay"></a>Azure Geçişi

Azure Geçişi'nin [WCF Geçişi](../service-bus-relay/relay-what-is-it.md) bileşeni, birçok farklı aktarım protokolünü ve web hizmeti standardını destekleyen merkezileştirilmiş (ancak yüksek düzeyde yük dengeleme olanağı sunan) bir hizmettir. Bunlar; SOAP, WS-* ve hatta REST'i bile içerir. [Geçiş hizmeti](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md), birçok farklı geçiş bağlanabilirliği seçeneği sağlar ve mümkün olduğunda doğrudan eşdüzey bağlantılar anlaşmasına yardım edebilir. Service Bus, hem performans hem de kullanılabilirlik bağlamında Windows Communication Foundation (WCF) kullanan .NET geliştiricileri için iyileştirilmiştir. Ayrıca, SOAP ve REST arabirimleri aracılığıyla geçiş hizmetine tam erişim sunar. Bu özellik sayesinde, tüm SOAP ve REST programlama ortamlarının Service Bus ile tümleşmesine olanak sağlanır.

Geçiş hizmeti, geleneksel tek yönlü mesajlaşmayı, istek/yanıt mesajlaşmasını ve eşdüzey mesajlaşmayı destekler. Ayrıca, artırılmış uçtan uca verimliliğe yönelik çift yönlü yuva iletişimi ile yayımla ve abone ol senaryolarına olanak sağlamak için İnternet kapsamında olay dağıtımını destekler. Geçişli mesajlaşma düzeninde bir şirket içi hizmet, giden bağlantı noktası aracılığıyla geçiş hizmetine bağlanır ve belirli bir randevu adresine bağlanan iletişim için çift yönlü yuva oluşturur. Ardından istemci, randevu adresini hedef alan geçiş hizmetine iletiler göndererek şirket içi hizmet ile iletişim kurabilir. Böylece geçiş hizmeti, zaten kullanımda olan çift yönlü yuva aracılığıyla iletileri şirket içi hizmete "geçirir". İstemcinin şirket içi hizmete doğrudan bağlantısının olmasına veya hizmetin nerede bulunduğunu bilmesine gerek yoktur. Ayrıca şirket içi hizmet için güvenlik duvarında gelen bağlantı noktalarının açık olması gerekmez.

WCF "geçiş" bağlamalarının bir paketini kullanarak geçiş hizmeti ile şirket içi hizmetiniz arasındaki bağlantıyı başlatırsınız. Arka planda ise geçiş bağlamaları, bulutta Service Bus ile tümleşen WCF kanalı bileşenleri oluşturmak üzere tasarlanan aktarım bağlama öğeleriyle eşleşir.

WCF Geçişi birçok avantaj sunar, ancak iletileri almak ve göndermek için istemci ile sunucunun aynı anda çevrimiçi olmasını gerektirir. Bu durum HTTP stili iletişim için en uygun seçim değildir. HTTP stili iletişimde tarayıcılar, mobil uygulamalar vb. gibi sürekli bağlantı kurmayan istemciler için istekler genellikle uzun ömürlü değildir. Aracılı mesajlaşma, ayrılmış iletişimi destekler ve kendine özgü avantajları vardır. Örneğin, istemciler ve sunucular gerektiğinde bağlanır ve zaman uyumsuz olarak işlemlerini gerçekleştirir.

## <a name="brokered-messaging"></a>Aracılı mesajlaşma

Geçiş şemasının aksine [kuyruklar, konular ve aboneliklerle](service-bus-queues-topics-subscriptions.md) Service Bus mesajlaşması, zaman uyumsuz veya "zamansal olarak bağımsız" şeklinde düşünülebilir. Üreticiler (göndericiler) ve tüketicilerin (alıcılar) aynı anda çevrimiçi olması gerekmez. Mesajlaşma altyapısı, kullanıcı tarafı almaya hazır olana kadar iletileri bir "aracıda" (örneğin bir kuyruk) güvenli şekilde depolar. Bu özellik, dağıtılan uygulamanın bileşenlerinin isteğe bağlı olarak (örneğin, bakım için) veya bir bileşen çökmesinden dolayı sistemin tamamını etkilemeden bağlantısının kesilmesine olanak sağlar. Ayrıca, alıcı uygulamanın yalnızca günün belirli zamanlarında çevrimiçi olması gerekebilir. Örneğin, bir stok yönetim sisteminin sadece iş günü sonunda çalıştırılması gerekir.

Service Bus aracılı mesajlaşma altyapısının temel bileşenleri kuyruklar, konu başlıkları ve aboneliklerdir. Bu bileşenler arasındaki temel fark ise konu başlıklarının, birden çok alıcıya gönderim yapma dahil olmak üzere gelişmiş içerik tabanlı yönlendirme ve teslimat mantığı için kullanılabilen yayımla/abone ol işlevlerini desteklemesidir. Bu bileşenler zamana bağlı ayırma, yayımla/abone ol ve yük dengelemesi gibi yeni, zaman uyumsuz mesajlaşma senaryolarına olanak sağlar. Bu mesajlaşma varlıkları hakkında daha fazla bilgi edinmek için bkz. [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md).

WCF Geçişi altyapısı sayesinde, aracılı mesajlaşma işlevi WCF ve .NET Framework programcıları için (REST aracılığıyla da) sunulur.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hizmeti hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın.

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

