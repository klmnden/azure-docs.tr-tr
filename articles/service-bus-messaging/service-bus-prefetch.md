---
title: Azure Service Bus önceden getirme iletileri | Microsoft Docs
description: Azure Service Bus iletileri önceden getiriliyor tarafından performansını.
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2018
ms.author: aschhab
ms.openlocfilehash: c63e6bf66e4832a1a5b0b5e6fc3dfbbf02d1e490
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125857"
---
# <a name="prefetch-azure-service-bus-messages"></a>Azure Service Bus iletileri önceden getirme

Zaman *önceden getirme* etkin resmi hizmet veri yolu istemcileri hiçbirinde alıcı sessizce daha fazla ileti kadar edinme [PrefetchCount](/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount#Microsoft_Azure_ServiceBus_QueueClient_PrefetchCount) ne uygulama başlangıçta için sorulan ötesinde sınırı.

Tek bir başlangıç [alma](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive) veya [ReceiveAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receiveasync) çağrı, bu nedenle çabuk kullanılabilir olarak döndürülen hemen kullanılmaya bir ileti alır. İstemci daha sonra arka arabellek önceden getirme dolduracak şekilde iletileri alır.

## <a name="enable-prefetch"></a>Önceden getirmeyi etkinleştir

.NET ile önceden getirme özelliği ayarlayarak etkinleştirmeniz [PrefetchCount](/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount#Microsoft_Azure_ServiceBus_QueueClient_PrefetchCount) özelliği bir **MessageReceiver**, **QueueClient**, veya **SubscriptionClient**  sıfırdan büyük bir sayı. Önceden getirme sıfır kapatır değeri ayarlanamadı.

Bu ayar alış tarafı kolayca ekleyebilirsiniz [QueuesGettingStarted](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/QueuesGettingStarted) veya [ReceiveLoop](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/ReceiveLoop) bu bağlamlarda etkisini görmek için örnekleri ayarları.

İletileri önceden getirme arabelleğinde, herhangi bir sonraki kullanılabilir ancak **alma**/**ReceiveAsync** çağrılar hemen arabellekteki yerine getirildiğini ve arabellek olarak yenilenir arka plan alanı kullanılabilir olduğunda. Teslimat için kullanılabilir hiçbir ileti yoksa alma işlemi arabellek ve ardından bekler ve blokları, beklendiği gibi boşaltır.

Önceden getirme ile aynı şekilde de çalışır [Onmessageoptions](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) ve [OnMessageAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessageasync) API'leri.

## <a name="if-it-is-faster-why-is-prefetch-not-the-default-option"></a>Neden daha hızlı ise, önceden getirme varsayılan seçeneği değil mi?

Önceden getirme ileti akışı olduğunda ve bir uygulama ister önce yerel alma işlemi için hazır bir ileti sağlayarak hızlandırır. Bu aktarım hızı kazanç uygulama yazarı açıkça yapmalısınız bir denge sonucudur:

İle [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu Al, önceden getirme arabelleğe alınan tüm iletiler kuyrukta artık kullanılamaz ve uygulamasına alınana kadar yalnızca bellek içi önceden getirme arabelleğinde bulunur aracılığıyla **alma**/**ReceiveAsync** veya **Onmessageoptions**/**OnMessageAsync** API'leri. İletileri uygulamasına alınmadan önce uygulama sona ererse, bu iletileri geri çevrilemez biçimde kaybolur.

İçinde [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode#Microsoft_ServiceBus_Messaging_ReceiveMode_PeekLock) modu Al, iletileri önceden getirme arabelleğe alınan kilitli bir durumda arabelleğine alınan ve kilit şekilde ilerleyen zaman aşımı saati sahip. Önceden getirme arabellek büyükse ve işleme kadar önceden getirme arabelleğinde veya bile uygulamanın iletiyi işlerken bulunan sırasında kilit süresi sona bu iletiyi alır, uygulamanın işlemek kafa karıştırıcı bazı olaylar olabilir.

Uygulama, süresi dolmuş veya imminently sona eren bir kilit içeren bir ileti almanız. Bu durumda, uygulama iletiyi işleyen, ancak sonra bunu bir kilit zaman aşımı nedeniyle tamamlayamıyor bulun. Uygulama denetleyebilirsiniz [LockedUntilUtc](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.lockeduntilutc) (saat aracısı ve yerel makine saat arasında eğriltme tabidir) özelliği. İletinin kilit süresi dolmuşsa, uygulamanın ileti yoksayması gereken; herhangi bir API çağrısı, ya da ileti ile yapılmalıdır. İleti süresinin dolmadığını, ancak Süre sonu yaklaşan kilidi yenilenmesi ve başka bir varsayılan kilit nokta çağırarak Genişletilmiş [ileti. RenewLock()](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_RenewLockAsync_System_String_)

Kilit sessizce önceden getirme arabelleğinde dolarsa iletisi bırakılmış olarak kabul edilir ve yeniden kuyruğa alma için kullanılabilir hale getirileceğini. Önceden getirme belleğe getirilmesi için neden olabilir; sonunda yerleştirilir. Önceden getirme arabellek genellikle üzerinden ileti süre çalışması olamaz, bu iletileri (geçerli kilitli) kullanılabilir durumda hiçbir zaman etkili bir şekilde teslim sürekli olarak önceden getirilmiş olması neden olur ve sonunda eski ileti sırası için bir kez taşınır en fazla teslim sayısı aşıldı.

İleti işleme için güvenilirliği yüksek derecede gerekir ve işleme önemli bir çalışma ve zaman alan, ölçülü veya hiç önceden getirme özelliğini kullanmanız önerilir.

Önceden getirme, yüksek aktarım hızı gerekir ve ileti işleme genellikle ucuz ise, önemli aktarım hızı avantajları verir.

En fazla hazırlık sayısı ve kuyruk veya abonelik yapılandırılmış kilit süresi kilit zaman aşımı en az bir ileti yanı sıra toplu beklenen ileti önceden getirme arabellek boyutu en fazla bir süredir işleme aşıyor, dengeli gerekir. Aynı zamanda, kilit zaman aşımı, iletileri kendi maksimum aşabilir kadar uzun olmaması gereken [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) yanlışlıkla bırakılır, bu nedenle yeniden teslim önce süresi dolacak şekilde kendi kilit gerek.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
