---
title: "Azure Service Bus özel durumlar Mesajlaşma | Microsoft Docs"
description: "Service Bus Mesajlaşma özel durumlar ve önerilen eylemler listesi."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/31/2018
ms.author: sethm
ms.openlocfilehash: efcfad2834c2d6775c6693f5c705a0531b2650d6
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="service-bus-messaging-exceptions"></a>Hizmet Veri Yolu mesajlaşma özel durumları
Bu makalede, Microsoft Azure hizmet API'leri ileti veri yolu tarafından oluşturulan bazı özel durumlar listelenir. Bu başvuru değiştirilebilir, bu nedenle geri Güncelleştirmeleri denetle.

## <a name="exception-categories"></a>Özel durum kategorileri
Mesajlaşma API'lerini aşağıdaki kategorilere bunları çözmek için uygulayabileceğiniz ilişkili eylem birlikte dönebilir özel durumları oluşturur. Bir özel durum nedenleri ve anlamı Mesajlaşma varlık türüne bağlı olarak değişebilir dikkat edin:

1. Kodlama hatası kullanıcı ([System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Genel eylem: devam etmeden önce kodu düzeltmeye çalışır.
2. Kurulum/yapılandırma hatası ([Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Genel eylem: yapılandırmanızı inceleyin ve gerekirse değiştirin.
3. Geçici özel durumları ([Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception), [ Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Genel eylem: işlemi yeniden deneyin veya kullanıcılara bildirin.
4. Diğer özel durumlar ([System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception)). Genel eylem: özgü özel durum türü; Lütfen aşağıdaki bölümündeki tabloya bakın. 

## <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tabloda, Mesajlaşma özel durum türleri ve neden olur ve notlar önerilen eylem yapabileceğiniz listeler.

| **Özel durum türü** | **Neden/açıklama/örnekleri** | **Önerilen eylem** | **Otomatik/hemen yeniden deneme sırasında unutmayın** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Sunucu istenen işlemi denetlenen belirtilen süre içinde yanıt vermedi [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout). Sunucu istenen işlemi tamamlanmamış olabilir. Bu, ağ veya diğer altyapı gecikmeler nedeniyle gerçekleşebilir. |Tutarlılık için sistem durumunu denetleyin ve gerekiyorsa yeniden deneyin. Bkz: [zaman aşımı özel durumları](#timeoutexception). |Yeniden deneme bazı durumlarda yardımcı olabilir; yeniden deneme mantığı kodu ekleyin. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |İstenen kullanıcı işlemi, sunucu veya hizmet içinde izin verilmiyor. Özel durum iletisi Ayrıntılar için bkz. Örneğin, [Complete()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) iletisi alındı bu özel durum oluşturur [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modu. |Kod ve belgelerine bakın. İstenen işlem geçerli olduğundan emin olun. |Yeniden deneme yardımcı olmayacaktır. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Zaten kapatılmış, durduruldu veya atılmış bir nesne üzerinde bir işlemi başlatmak için bir deneme yapılır. Nadir durumlarda ortam işlem zaten atıldı. |Kodu kontrol edip bırakılmış bir nesne üzerinde işlem çağrılmaz emin olun. |Yeniden deneme yardımcı olmayacaktır. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |[TokenProvider](/dotnet/api/microsoft.azure.servicebus.tokenprovider) nesne bir belirteç alınamadı, belirteci geçersiz veya işlemi gerçekleştirmek için gerekli talep belirteci içermiyor. |Belirteç sağlayıcı doğru değerlerle oluşturulduğundan emin olun. Erişim denetimi hizmeti yapılandırmasını denetleyin. |Yeniden deneme bazı durumlarda yardımcı olabilir; yeniden deneme mantığı kodu ekleyin. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Yönteme sağlanan bir veya daha fazla bağımsız değişken geçersiz.<br /> URI tarafından sağlanan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [oluşturma](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) yolu segment(s) içerir.<br /> URI şeması tarafından sağlanan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [oluşturma](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) geçersiz. <br />Özellik değeri, 32 KB'den büyük. |Çağrıyı yapan kod denetleyin ve bağımsız değişkenlerin doğru olduğundan emin olun. |Yeniden deneme yardımcı olmayacaktır. |
| [MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception) |İşlemle ilişkili varlık yok veya silinmiş olabilir. |Varlık var olduğundan emin olun. |Yeniden deneme yardımcı olmayacaktır. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Belirli sıra numarasına sahip bir ileti almaya çalışır. Bu ileti bulunamadı. |İleti zaten alınmış değil emin olun. Sahipsiz sıraya ileti deadlettered olup olmadığını görmek için kontrol edin. |Yeniden deneme yardımcı olmayacaktır. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |İstemci, hizmet veri yolu bağlantı kurmak mümkün değil. |Sağlanan ana makine adının doğru olduğundan ve konağa erişilebildiğinden emin olun. |Aralıklı bağlantısı sorunları varsa, yeniden deneme yardımcı olabilir. |
| [ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception) |Hizmet şu anda isteğinizi mümkün değil. |İstemci bir süre bekleyin. ardından işlemi yeniden deneyin. |İstemci, belirli bir zaman aralığından sonra yeniden deneyebilir. Farklı bir özel durumla bir yeniden deneme sonuçlanırsa, bu özel durum yeniden deneme davranışını denetleyin. |
| [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception) |İletiyle ilişkilendirilmiş kilit belirtecinin süresi doldu veya kilidi belirteci bulunamadı. |İletiyi siler. |Yeniden deneme yardımcı olmayacaktır. |
| [SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception) |Bu oturumla ilişkili kilit kaybolur. |Abort [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) nesnesi. |Yeniden deneme yardımcı olmayacaktır. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Aşağıdaki durumlarda oluşturulan özel durum ileti genel:<br /> Oluşturmak için bir girişimde bir [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) adı veya farklı varlık türü için (örneğin, bir konu) ait yolu kullanarak.<br />  256 KB daha büyük bir ileti göndermek için bir deneme yapılır. Sunucu veya hizmet isteği işleme sırasında bir hatayla karşılaştı. Özel durum iletisi ayrıntıları için lütfen bkz. Bu genellikle geçici bir istisnadır. |Kod denetleyin ve yalnızca serileştirilebilir nesneler ileti gövdesi için kullanıldığından emin olun (veya özel bir seri hale getirici kullanın). Özelliklerin desteklenen değer türleri ve yalnızca desteklenen kullanım türleri için belgelere bakın. Denetleme [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient) özelliği. Eğer öyleyse **doğru**, işlemi yeniden deneyin. |Yeniden deneme davranışı tanımlanmamış ve yardımcı. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Bu hizmet ad alanı içinde başka bir varlık tarafından kullanılan bir ada sahip bir varlık oluşturma girişimi. |Var olan bir varlığını silin veya oluşturulacak varlık için farklı bir ad seçin. |Yeniden deneme yardımcı olmayacaktır. |
| [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) |Mesajlaşma varlığı izin verilen boyut üst sınırına ulaştı veya bir ad alanına bağlantısı sayısı aşıldı. |Alan, varlık veya onun alt sıralar iletilerini alarak varlıkta oluşturun. Bkz: [QuotaExceededException](#quotaexceededexception). |İletileri zarfında kaldırılmışsa yeniden deneme yardımcı olabilir. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Geçersiz kural eylemi oluşturmayı denerseniz, Service Bus bu özel durumun döndürür. Bu ileti için kural eylemi işlenirken bir hata oluşursa, Service Bus bu özel bir deadlettered iletiye ekler. |Kural eylemi doğruluğunu denetleyin. |Yeniden deneme yardımcı olmayacaktır. |
| [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) |Hizmet veri yolu geçersiz bir filtre oluşturmayı denerseniz, bu özel durumun döndürür. Hizmet veri yolu İleti Filtresi işlenirken bir hata oluştu, bu özel bir deadlettered iletiye ekler. |Filtre doğruluğunu denetleyin. |Yeniden deneme yardımcı olmayacaktır. |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Belirli bir oturum kimliği ile bir oturumu kabul çalışıldı, ancak oturumu şu anda başka bir istemci tarafından kilitlenmiş. |Diğer istemciler tarafından oturumun kilidi emin olun. |Geçici oturum bırakılmışsa yeniden deneme yardımcı olabilir. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |Çok fazla operations işlem bir parçasıdır. |Bu işlemin bir parçası olan işlem sayısını azaltın. |Yeniden deneme yardımcı olmayacaktır. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.azure.servicebus.messagingentitydisabledexception) |Devre dışı bırakılmış bir varlığı üzerinde bir çalışma zamanı işlemi isteği. |Varlık etkinleştirin. |Varlık arada etkinleştirdiyseniz yeniden deneme yardımcı olabilir. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Önceden filtreleme etkin olduğundan ve filtreleri hiçbiri eşleşen bir konu başlığına ileti gönderme, Service Bus bu özel durumun döndürür. |En az bir filtre eşleştiğinden emin olun. |Yeniden deneme yardımcı olmayacaktır. |
| [MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |İleti yükünü 256 KB sınırını aşıyor. Sistem özellikleri ve tüm .NET ek yükü içerebilir toplam ileti boyutu 256 KB'lik sınırını olduğuna dikkat edin. |İleti yükü azaltın ve işlemi yeniden deneyin. |Yeniden deneme yardımcı olmayacaktır. |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |Ortam işlem (*Transaction.Current*) geçersiz. Tamamlanan veya iptal. İç özel duruma ek bilgi sağlayabilir. | |Yeniden deneme yardımcı olmayacaktır. |
| [Transactionındoubtexception](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |Bir işlem, şüpheli bir işlem denendi veya hareketi için bir girişimde ve şüpheli işlem olur. |İşlem zaten önem silinmiş gibi uygulamanız (bir özel durum olarak), bu özel durumun işlemelidir. |- |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) belirli bir varlık için bir kota aşıldı gösterir.

### <a name="queues-and-topics"></a>Kuyruklar ve konu başlıkları
Kuyruklar ve konular için bu genellikle sırasının boyutudur. Hata iletisi özelliği ayrıca aşağıdaki örnekteki gibi ayrıntıları içerir:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: The maximum entity size has been reached or exceeded for Topic: ‘xxx-xxx-xxx’. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

İleti durumları, konu, bu örnek 1 GB (varsayılan boyut sınırı) boyut sınırını aştı. 

### <a name="namespaces"></a>Ad Alanları

Ad alanları için [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) bir uygulama bir ad alanı için bağlantı sayısını aştı belirtebilirsiniz. Örneğin:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

#### <a name="common-causes"></a>Olası nedenler
Bu hatanın ortak nedenleri vardır: sahipsiz sırayı ve ileti alıcıları çalışmıyor.

1. **[Sahipsiz sıra](service-bus-dead-letter-queues.md)**  bir okuyucu iletileri tamamlamak başarısız olan ve kilitleme sona erdiğinde iletileri sıra/konu döndürülür. Okuyucu gelen arama engelleyen bir özel durum karşılaşırsa gerçekleşebilir [BrokeredMessage.Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.complete). Bir ileti 10 kez okuduktan sonra varsayılan olarak sahipsiz sıraya taşır. Bu davranış tarafından denetlenen [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) özelliği ve varsayılan değer 10 sahiptir. Sahipsiz sıraya ileti üst üste yığmak gibi bunlar yer kaplar.
   
    Sorunu çözmek için okuma ve diğer herhangi bir sıradan gibi sahipsiz sıraya alınan iletileri tamamlayın. Kullanabileceğiniz [FormatDeadLetterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formatdeadletterpath) sahipsiz sırayı yolunu biçimlendirin yardımcı yöntemi.
2. **Alıcı durduruldu** bir alıcı bir kuyruk veya abonelik iletileri alma durduruldu. Bu tanımlamak için bakmak için yoludur [QueueDescription.MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) iletileri tam dökümünü gösterir özelliği. Varsa [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount) özelliği yüksek veya artan sonra iletileri yazılmakta kadar hızlı okunan değil.

## <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) kullanıcı tarafından başlatılan bir işlem işlem zaman aşımından daha uzun sürüyor gösterir. 

Değerini denetlemelisiniz [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) bu sınırı basarsa olarak özelliği, ayrıca neden olabilecek bir [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

### <a name="queues-and-topics"></a>Kuyruklar ve konu başlıkları
Kuyruklar ve konu başlıkları için zaman aşımı belirtilen ya da [MessagingFactorySettings.OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout) parçası olarak, bağlantı dizesi veya aracılığıyla özelliği [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.azure.servicebus.servicebusconnectionstringbuilder). Hata iletisi farklılık gösterebilir, ancak her zaman geçerli işlem için belirtilen zaman aşımı değeri içeriyor. 



## <a name="next-steps"></a>Sonraki adımlar

Tam Service Bus .NET API başvuru için bkz: [Azure .NET API Başvurusu](/dotnet/api/overview/azure/service-bus).

Daha fazla bilgi edinmek için [Service Bus](https://azure.microsoft.com/services/service-bus/), aşağıdaki makalelere bakın:

* [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus mimarisi](service-bus-architecture.md)

