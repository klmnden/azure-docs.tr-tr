---
title: Azure Service Bus Mesajlaşma özel durumları | Microsoft Docs
description: Service Bus Mesajlaşma özel durumları ve önerilen eylemler listesi.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: aschhab
ms.openlocfilehash: b90e87310bf6dec505176b7f4d4cb9e15ac57c20
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60307786"
---
# <a name="service-bus-messaging-exceptions"></a>Hizmet Veri Yolu mesajlaşma özel durumları
Bu makalede, Microsoft Azure hizmet API'leri ileti veri yolu tarafından oluşturulan bazı özel durumlar listelenir. Bu başvuru değişebilir, bu nedenle geri Güncelleştirmeleri denetle.

## <a name="exception-categories"></a>Özel durum kategorisi
Mesajlaşma API'lerini bunları çözmek için gerçekleştirebileceğiniz ilişkili bir eylem ile birlikte aşağıdaki kategorilere ayrılır, özel durum oluşturur. Bir özel durum nedenlerini ve anlamı Mesajlaşma varlığı türüne bağlı olarak değişebilir:

1. Kodlama hatası kullanıcı ([System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Genel eylem: devam etmeden önce kod düzeltmeye çalışın.
2. Kurulumu/yapılandırma hatası ([Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Genel eylem: yapılandırmanızı inceleyin ve gerekirse değiştirin.
3. Geçici özel durumlar ([Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception), [ Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Genel eylem: işlemi yeniden deneyin veya kullanıcılara bildirin. Unutmayın `RetryPolicy` sınıfında istemci SDK'sı, otomatik olarak yeniden deneme işlemlerini şekilde yapılandırılabilir. Bkz: [yeniden deneme Kılavuzu](/azure/architecture/best-practices/retry-service-specific#service-bus) daha fazla bilgi için.
4. Diğer özel durumlar ([System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception)). Genel eylem: özel durum türü; özel Lütfen aşağıdaki bölümdeki tabloya bakın: 

## <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tabloda, Mesajlaşma özel durum türlerini ve nedenler ve notları önerilen eylem uygulayabileceğiniz listeler.

| **Özel durum türü** | **Neden/açıklama/örnekleri** | **Önerilen eylem** | **Otomatik/hemen yeniden deneme sırasında dikkat edin.** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Sunucu istenen işlemi tarafından denetlenen belirtilen süre içinde yanıt vermedi [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings). Sunucu istenen işlemi tamamlanmamış olabilir. Bu, ağ veya diğer altyapı gecikmeler nedeniyle oluşabilir. |Tutarlılık için sistem durumunu denetleyin ve gerekirse yeniden deneyin. Bkz: [zaman aşımı özel durumları](#timeoutexception). |Bazı durumlarda yeniden başlatma yardımcı; yeniden deneme mantığı için kod ekleyin. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |İstenen kullanıcı işlemi, sunucu veya hizmet içinde izin verilmiyor. Özel durum iletisi ayrıntıları için bkz. Örneğin, [Complete()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) iletisi alındı bu özel durum oluşturur [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modu. |Kod ve belgelere bakın. İstenen işlem geçerli olduğundan emin olun. |Yeniden deneme yardımcı olmaz. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Zaten kapalı, durduruldu veya atılmış bir nesne üzerinde bir işlem çağırmak için bir deneme yapılır. Nadiren de olsa, ortam işlem zaten atıldı. |Kodu kontrol edip atılan nesneye işlemleri çağırma kullanılamaz olduğundan emin olun. |Yeniden deneme yardımcı olmaz. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |[TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) nesne bir belirteci alınamadı, belirteci geçersiz veya işlemi gerçekleştirmek için gerekli talep belirteci içermiyor. |Belirteç sağlayıcısı, doğru değerlerle oluşturulduğundan emin olun. Erişim denetimi hizmetinin yapılandırmasını denetleyin. |Bazı durumlarda yeniden başlatma yardımcı; yeniden deneme mantığı için kod ekleyin. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[Üretiliyor](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Yöntemine sağlanan bir veya daha fazla bağımsız değişken geçersiz.<br /> Sağlanan URI [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Oluştur](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) yolu segment(s) içerir.<br /> Sağlanan URI düzeni [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Oluştur](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) geçersiz. <br />Özellik değeri, 32 KB'den daha büyük. |Çağıran kod denetleyin ve bağımsız değişkenlerin doğru olduğundan emin olun. |Yeniden deneme yardımcı olmaz. |
| [MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception) |İşlemle ilişkili varlık yok veya silinmiş olabilir. |Varlığın mevcut olduğundan emin olun. |Yeniden deneme yardımcı olmaz. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Bir özel sıra numarası ile bir ileti almaya çalışır. Bu ileti bulunamadı. |İleti zaten alınmış değil emin olun. İleti deadlettered uygulanmış olup olmadığını görmek için teslim edilemeyen iletiler sırası denetleyin. |Yeniden deneme yardımcı olmaz. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |İstemci, Service Bus bağlantı kuramıyor. |Sağlanan ana bilgisayar adının doğru olduğundan ve konağın erişilebilir olduğundan emin olun. |Aralıklı bağlantı sorunu yoksa, yeniden deneme yardımcı olabilir. |
| [ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception) |Hizmet şu anda isteğinizi mümkün değil. |İstemci bir süre bekleyin. ardından işlemi yeniden deneyin. |İstemci, belirli bir süre sonra yeniden deneyebilir. Farklı bir özel durum yeniden oluşur, o özel durumu yeniden deneme davranışını kontrol edin. |
| [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception) |İletiyle ilişkili kilit belirtecinin süresi doldu veya kilit belirteci bulunamadı. |İletiyi siler. |Yeniden deneme yardımcı olmaz. |
| [SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception) |Bu oturumla ilişkili kilit kaybolur. |Abort [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) nesne. |Yeniden deneme yardımcı olmaz. |
| [Istransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Genel, aşağıdaki durumlarda oluşturulan özel durum iletileri:<br /> Oluşturmak için bir girişimde bir [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) adınızı veya ait farklı bir varlık türü için (örneğin, bir konu) yolu.<br />  256 KB'den daha büyük bir ileti göndermek için bir deneme yapılır. Sunucu veya hizmet isteğinin işlenmesi sırasında bir hatayla karşılaştı. Özel durum iletisi ayrıntıları için bkz. Genellikle geçici bir özel durum budur. |Kodunu kontrol edin ve yalnızca serileştirilebilir nesneler ileti gövdesi için kullanıldığından emin olun (veya özel bir serileştirici kullanın). Özelliklerin desteklenen değer türleri ve yalnızca desteklenen kullanım türleri için belgelere bakın. Denetleme [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) özelliği. Eğer öyleyse **true**, işlemi yeniden deneyin. |Yeniden deneme davranışı tanımsızdır ve yardımcı. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Bu hizmet ad alanındaki başka bir varlık tarafından zaten kullanılan bir ada sahip bir varlık oluşturmaya çalışır. |Var olan bir varlığa silin veya oluşturulacak varlığın farklı bir ad seçin. |Yeniden deneme yardımcı olmaz. |
| [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) |Mesajlaşma varlığı, izin verilen boyut üst sınırına veya bir ad alanı bağlantılarını sayısı aşıldı. |Alanı varlık içinde varlık veya onun alt iletilerini alma oluşturun. Bkz: [QuotaExceededException](#quotaexceededexception). |İletiler sırada kaldırdıysanız, yeniden deneme yardımcı olabilir. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Geçersiz kural eylemi oluşturmaya çalışırsanız, Service Bus bu özel durumun döndürür. Kural eylemi için bu iletiyi işlerken bir hata oluşursa, Service Bus bu özel durum deadlettered iletiye ekler. |Kural eylemi doğruluğunu denetleyin. |Yeniden deneme yardımcı olmaz. |
| [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) |Hizmet veri yolu, geçersiz bir filtre oluşturmaya çalışırsanız bu özel durumun döndürür. Bu ileti için filtre işlenirken bir hata oluştu, Service Bus bu özel durum deadlettered iletiye ekler. |Filtre doğruluğunu denetleyin. |Yeniden deneme yardımcı olmaz. |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Belirli bir oturum kimliği ile bir oturumu kabul etmek çalışır, ancak oturum başka bir istemci tarafından şu anda kilitli. |Diğer istemciler tarafından oturumun kilidi olduğundan emin olun. |Bu arada oturum bırakılmışsa yeniden deneme yardımcı olabilir. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |Çok sayıda işlem işlem bir parçasıdır. |Bu işlemin bir parçası olan işlemlerin sayısını azaltın. |Yeniden deneme yardımcı olmaz. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.azure.servicebus.messagingentitydisabledexception) |Bir çalışma zamanı işlemi devre dışı bırakılmış bir varlık için isteği. |Varlık etkinleştirin. |Bu arada varlık etkinleştirdiyseniz, yeniden deneme yardımcı olabilir. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Önceden filtreleme etkin olduğundan ve filtreleri hiçbiri eşleşen bir konu başlığına bir ileti gönderirseniz, Service Bus bu özel durumun döndürür. |En az bir filtre ile eşleştiğinden emin olun. |Yeniden deneme yardımcı olmaz. |
| [MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |İleti yükü 256 KB'lik sınırını aşıyor. 256 KB'lik sınırını Sistem özellikleri ve herhangi bir .NET yükü içerebilir toplam ileti boyutudur. |İleti yükü azaltın, ardından işlemi yeniden deneyin. |Yeniden deneme yardımcı olmaz. |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |Ortam işlem (*Transaction.Current*) geçersiz. Tamamlanan veya iptal. İç özel duruma ek bilgi sağlayabilir. | |Yeniden deneme yardımcı olmaz. |
| [Transactionındoubtexception](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |Bir işlem, şüpheli bir işlem üzerinde denenen veya hareketi tamamlamak için bir girişimde ve şüpheli işlem olur. |Uygulamanız, işlemin zaten teslim edilmiş olarak (bir özel durum olarak), bu özel durum işlemesi gerekir. |- |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception), belirli bir varlık için belirlenen kotanın aşıldığını gösterir.

### <a name="queues-and-topics"></a>Kuyruklar ve konular
Kuyruklar ve konular için genellikle kuyruk boyutu budur. Hata iletisi özelliği, daha fazla ayrıntı, aşağıdaki örnekte olduğu gibi içerir:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: The maximum entity size has been reached or exceeded for Topic: ‘xxx-xxx-xxx’. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

İletinin konu, bu durum 1 GB (varsayılan boyut sınırını) boyut sınırını aştı belirtir. 

### <a name="namespaces"></a>Ad Alanları

Ad alanları için [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) uygulamanın en fazla bir ad alanı için bağlantı sayısını aştı belirtebilirsiniz. Örneğin:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

#### <a name="common-causes"></a>Olası nedenler
Bu hata sık karşılaşılan iki nedeni vardır: sahipsiz sırayı ve ileti alıcılar işlevsiz.

1. **[Eski ileti sırası](service-bus-dead-letter-queues.md)**  iletileri tamamlamak bir okuyucu başarısız oluyor ve kilit süresi dolduğunda queue/topic için iletileri döndürülür. Okuyucuyu arama dan engelleyen bir özel durumla karşılaşırsa oluşabilir [BrokeredMessage.Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.complete). 10 kez bir iletinin okunup okunmadığını sonra varsayılan olarak teslim edilemeyen iletiler kuyruğuna taşır. Bu davranış tarafından denetlenir [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) özelliği ve 10 varsayılan değerine sahiptir. Sahipsiz sıraya ileti üst üste yığmak gibi bunlar yer kaplar.
   
    Sorunu çözmek için okuma ve diğer bir kuyruktan gibi eski ileti sırası gelen iletileri tamamlayın. Kullanabileceğiniz [FormatDeadLetterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formatdeadletterpath) geçerliliğini yitirmiş kuyruk yolu biçimlendirme yardımcı olmak için yöntemi.
2. **Alıcı durduruldu**. Bir alıcı bir kuyruk veya abonelikten ileti alma durduruldu. Bunu belirlemek için bakmak için yoludur [QueueDescription.MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) özelliği iletilerin tam dökümünü gösterir. Varsa [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount) özelliği yüksek veya giderek büyüyen sonra iletileri yazılmakta olan kadar hızlı okunan değil.

## <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) kullanıcı tarafından başlatılan bir işlemin işlemi zaman aşımı süresinden daha uzun sürdüğünü gösterir. 

Değerini denetlemeniz gerekir [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) bu limitini aştıktan olarak özelliği de neden olabilir bir [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

### <a name="queues-and-topics"></a>Kuyruklar ve konular
Kuyruklar ve konular için zaman aşımı belirtildi ya da [MessagingFactorySettings.OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings) özelliği aracılığıyla veya bağlantı dizesinin parçası olarak [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.azure.servicebus.servicebusconnectionstringbuilder). Hata iletisi farklılık gösterebilir, ancak her zaman geçerli işlem için belirtilen zaman aşımı değeri içerir. 



## <a name="next-steps"></a>Sonraki adımlar

Tam hizmet veri yolu .NET API Başvurusu için bkz. [Azure .NET API Başvurusu](/dotnet/api/overview/azure/service-bus).

Hakkında daha fazla bilgi edinmek için [Service Bus](https://azure.microsoft.com/services/service-bus/), aşağıdaki makalelere bakın:

* [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
* [Service Bus mimarisi](service-bus-architecture.md)

