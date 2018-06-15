---
title: Azure Service Bus hazırlık iletileri | Microsoft Docs
description: Azure Service Bus ileti prefetching tarafından performansı.
services: service-bus-messaging
documentationcenter: ''
author: clemensv
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: sethm
ms.openlocfilehash: 0a61918108a48f4a9fa3d1c07cc8d41525f1f2a0
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
ms.locfileid: "28928170"
---
# <a name="prefetch-azure-service-bus-messages"></a>Azure Service Bus ileti Hazırlık

Zaman *hazırlık* etkin herhangi resmi hizmet veri yolu istemcileri alıcı sessizce daha fazla ileti en fazla edinir [PrefetchCount](/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount#Microsoft_Azure_ServiceBus_QueueClient_PrefetchCount) ne uygulama başlangıçta için sorulan ötesinde sınırı.

Tek bir başlangıç [alma](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive) veya [ReceiveAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receiveasync) çağrısı, bu nedenle çabuk kullanılabilir olarak döndürülen hemen tüketimi için bir ileti alır. İstemci daha sonra hazırlık arabellek doldurmak için arka planda iletileri alır.

## <a name="enable-prefetch"></a>Önceden getirme etkinleştir

.NET ile önceden getirme özelliği ayarlanarak etkinleştirmeden [PrefetchCount](/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount#Microsoft_Azure_ServiceBus_QueueClient_PrefetchCount) özelliği bir **MessageReceiver**, **QueueClient**, veya **SubscriptionClient**  sıfırdan büyük bir sayı. Değeri, sıfır kapatır hazırlık için ayarlama.

Alış tarafı için bu ayar kolayca ekleyebilirsiniz [QueuesGettingStarted](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/QueuesGettingStarted) veya [ReceiveLoop](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/ReceiveLoop) bu bağlamlarında etkisini görmek için örnekleri ayarları.

İletileri önceden getirme arabelleğinde, sonraki kullanılabilir, ancak **alma**/**ReceiveAsync** çağrıları hemen arabelleğinden yerine getirildi ve arabellek içinde yenilenir arka plan alanı kullanılabilir olduğunda. Yoksa hiçbir ileti teslimi için alma işlemi arabellek ve ardından bekler veya blokları, beklendiği gibi boşaltır.

Önceden getirme de çalışır ile aynı şekilde [Onmessageoptions](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) ve [OnMessageAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessageasync) API'leri.

## <a name="if-it-is-faster-why-is-prefetch-not-the-default-option"></a>Daha hızlı olması durumunda neden hazırlık varsayılan seçeneği değil mi?

Önceden getirme ne zaman ve uygulama için bir tane ister önce yerel alımı için kullanıma hazır bir ileti sağlayarak ileti akışı hızlandırır. Bu işleme kazanç uygulama yazarı açıkça yapmalısınız bir denge sonucudur:

İle [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode.receiveanddelete) modu Al, hazırlık arabelleğe alınan tüm iletileri artık sırada kullanılabilir olmayan ve uygulamaya alınana kadar bellek içi önceden getirme arabelleğinde yalnızca bulunur aracılığıyla **alma**/**ReceiveAsync** veya **Onmessageoptions**/**OnMessageAsync** API'leri. Bu iletiler iletileri uygulamasına alınmadan önce uygulama sona ererse, modelinizden kaybolur.

İçinde [PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode.peeklock) modu Al, hazırlık arabelleğe alınan iletileri kilitli bir durumda arabelleğe alınan ve kilit şekilde ilerleyen zaman aşımı saati sahip. Önceden getirme arabellek büyük ve işlem çok uzun kilitleri önceden getirme arabelleğinde veya bile uygulama iletiyi işlerken bulunan hatayla sona bu iletiyi alır, bazı kafa karıştırıcı olayları işlemek üzere bir uygulama olabilir.

Uygulama, süresi dolmuş ya da imminently süresi dolan kilidi içeren bir ileti alma. Öyleyse, uygulama iletiyi işlemek, ancak sonra onu kilitleme sona ermesi nedeniyle tamamlayamıyor bulun. Uygulama denetleyebilirsiniz [LockedUntilUtc](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.lockeduntilutc#Microsoft_Azure_ServiceBus_Core_MessageReceiver_LockedUntilUtc) (saat aracısı arasında yerel makine saat eğriltme tabi olan) özelliği. İleti kilit süresi dolmuşsa, uygulama iletiyi yoksaymak gerekir; hiçbir API çağrısı, ya da ileti ile yapılmalıdır. İleti süresi sona erme kullanıcılarınıza varsa, kilit kullanılabilir yenilendi ve başka bir varsayılan kilit noktayla çağırarak Genişletilmiş [ileti. RenewLock()](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_RenewLockAsync_System_String_)

Kilit sessizce önceden getirme arabelleğinde süresi dolarsa, ileti terk olarak kabul edilir ve yeniden sırasından alma için kullanılabilir hale getirilir. Önceden getirme arabelleğe getirilmesi için neden olabilir; sonunda yerleştirilir. Önceden getirme arabellek genellikle aracılığıyla ileti sona erme sırasında çalışması olamaz, bu iletileri kullanılabilir (geçerli bir şekilde kilitli) durumda hiçbir zaman etkili bir şekilde teslim art arda prefetched olması neden olur ve sonunda sahipsiz sıraya kez taşınır Maksimum teslimat sayısı aşıldı.

İleti işleme için güvenilirlik yüksek derecede gerekir ve işleme önemli iş ve zaman alır, ölçülü veya hiç önceden getirme özelliğini kullanmanız önerilir.

Yüksek boyunca gerekir ve ileti işleme genellikle ucuz ise, hazırlık önemli verimlilik avantajları verir.

En fazla hazırlık sayısı ve sıra veya abonelik üzerinde yapılandırılmış kilit süresi sağlayacak şekilde kilit zaman aşımı en az bir ileti yanı sıra önceden getirme arabelleğin en büyük boyutu için işleme toplu beklenen bir ileti aşıyor dengeli gerekir. Aynı anda kilit zaman aşımı gelmelidir iletileri kendi maksimum aşabilir kadar uzun süre olmaması [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) yanlışlıkla bırakılan, böylece yeniden teslim önce süresi dolacak şekilde kendi kilit gerektiren.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
