---
title: "Özel durumlar Mesajlaşma Azure Event Hubs | Microsoft Docs"
description: "Azure Event Hubs özel durumlar ve önerilen eylemler ileti listesi."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 2c6273de-0106-47e5-b45d-59040e51f2c5
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 964475ba8b42ac41707fa78468bfe551677c595f
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="event-hubs-messaging-exceptions"></a>Event Hubs mesajlaşma özel durumları

Bu makalede, olay hub'ları API'leri içeren Azure Service Bus Mesajlaşma API kitaplığı tarafından oluşturulan özel durumları bazıları listelenmektedir. Bu başvuru değiştirilebilir, bu nedenle geri Güncelleştirmeleri denetle.

## <a name="exception-categories"></a>Özel durum kategorileri

Olay hub'ları API'leri aşağıdaki kategorilere bunları çözmek için uygulayabileceğiniz ilişkili eylem birlikte dönebilir özel durumları oluşturur.

1. Kullanıcı kodlama hatası: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). Genel eylem: devam etmeden önce kodu düzeltmeye çalışır.
2. Kurulum/yapılandırma hatası: [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Genel eylem: yapılandırmanızı inceleyin ve gerekirse değiştirin.
3. Geçici özel durumlar: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](#serverbusyexception), [Microsoft.Azure.EventHubs.ServerBusyException](#serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). Genel eylem: işlemi yeniden deneyin veya kullanıcılara bildirin.
4. Diğer özel durumlar: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](#timeoutexception), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception). Genel eylem: özgü özel durum türü; Lütfen aşağıdaki bölümündeki tabloya bakın. 

## <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tabloda, Mesajlaşma özel durum türleri ve neden olur ve notlar önerilen eylem yapabileceğiniz listeler.

| **Özel durum türü** | **Neden/açıklama/örnekleri** | **Önerilen eylem** | **Otomatik/hemen yeniden deneme sırasında unutmayın** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Sunucu istenen işlemi tarafından denetlenen belirtilen süre içinde yanıt vermedi [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout). Sunucu istenen işlemi tamamlanmamış olabilir. Bu, ağ veya diğer altyapı gecikmeler nedeniyle gerçekleşebilir. |Tutarlılık için sistem durumunu denetleyin ve gerekiyorsa yeniden deneyin.<br /> Bkz: [TimeoutException](#timeoutexception). |Yeniden deneme bazı durumlarda yardımcı olabilir; yeniden deneme mantığı kodu ekleyin. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |İstenen kullanıcı işlemi, sunucu veya hizmet içinde izin verilmiyor. Özel durum iletisi Ayrıntılar için bkz. Örneğin, [tam](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) iletisi alındı bu özel durum oluşturur [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu. |Kod ve belgelerine bakın. İstenen işlem geçerli olduğundan emin olun. |Yeniden deneme yardımcı olmayacaktır. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Zaten kapatılmış, durduruldu veya atılmış bir nesne üzerinde bir işlemi başlatmak için bir deneme yapılır. Nadir durumlarda ortam işlem zaten atıldı. |Kodu kontrol edip bırakılmış bir nesne üzerinde işlem çağrılmaz emin olun. |Yeniden deneme yardımcı olmayacaktır. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |[TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) nesne bir belirteç alınamadı, belirteci geçersiz veya işlemi gerçekleştirmek için gerekli talep belirteci içermiyor. |Belirteç sağlayıcı doğru değerlerle oluşturulduğundan emin olun. Erişim denetimi hizmeti yapılandırmasını denetleyin. |Yeniden deneme bazı durumlarda yardımcı olabilir; yeniden deneme mantığı kodu ekleyin. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Yönteme sağlanan bir veya daha fazla bağımsız değişken geçersiz. URI tarafından sağlanan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [oluşturma](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) yolu segment(s) içerir. URI şeması tarafından sağlanan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [oluşturma](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) geçersiz. Özellik değeri, 32 KB'den büyük. |Çağrıyı yapan kod denetleyin ve bağımsız değişkenlerin doğru olduğundan emin olun. |Yeniden deneme yardımcı olmayacaktır. |
| [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /> [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) |İşlemle ilişkili varlık yok veya silinmiş olabilir. |Varlık var olduğundan emin olun. |Yeniden deneme yardımcı olmayacaktır. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |İstemci, olay Hub'ına bağlantı mümkün değil. |Sağlanan ana makine adının doğru olduğundan ve konağa erişilebildiğinden emin olun. |Aralıklı bağlantısı sorunları varsa, yeniden deneme yardımcı olabilir. |
| [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) |Hizmet şu anda isteğinizi mümkün değil. |İstemci bir süre bekleyin. ardından işlemi yeniden deneyin. <br /> Bkz: [ServerBusyException](#serverbusyexception). |İstemci, belirli bir zaman aralığından sonra yeniden deneyebilir. Farklı bir özel durumla bir yeniden deneme sonuçlanırsa, bu özel durum yeniden deneme davranışını denetleyin. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Aşağıdaki durumlarda oluşturulan özel durum ileti genel: oluşturmak için bir girişimde bir [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) adı veya farklı varlık türü için (örneğin, bir konu) ait yolu kullanarak. 256 KB daha büyük bir ileti göndermek için bir deneme yapılır. Sunucu veya hizmet isteği işleme sırasında bir hatayla karşılaştı. Özel durum iletisi ayrıntıları için lütfen bkz. Bu genellikle geçici bir istisnadır. |Kod denetleyin ve yalnızca serileştirilebilir nesneler ileti gövdesi için kullanıldığından emin olun (veya özel bir seri hale getirici kullanın). Özelliklerin desteklenen değer türleri ve yalnızca desteklenen kullanım türleri için belgelere bakın. Denetleme [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient) özelliği. Eğer öyleyse **doğru**, işlemi yeniden deneyin. |Yeniden deneme davranışı tanımlanmamış ve yardımcı. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Bu hizmet ad alanı içinde başka bir varlık tarafından kullanılan bir ada sahip bir varlık oluşturma girişimi. |Var olan bir varlığını silin veya oluşturulacak varlık için farklı bir ad seçin. |Yeniden deneme yardımcı olmayacaktır. |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |Mesajlaşma varlığı, izin verilen boyut üst sınırına ulaştı. (5'tir) maksimum sayısı alıcılar bir tüketici başına grubu düzeyinde açılan bu durum meydana gelebilir. |Alan, varlık veya onun alt sıralar iletilerini alarak varlıkta oluşturun. <br /> Bkz: [QuotaExceededException](#quotaexceededexception) |İletileri zarfında kaldırılmışsa yeniden deneme yardımcı olabilir. |
|  | | | |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) |Devre dışı bırakılmış bir varlığı üzerinde bir çalışma zamanı işlemi isteği. |Varlık etkinleştirin. |Varlık arada etkinleştirdiyseniz yeniden deneme yardımcı olabilir. |
| [Microsoft.ServiceBus.Messaging.MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /> [Microsoft.Azure.EventHubs.MessageSizeExceededException](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | İleti yükünü 256 K sınırını aşıyor. Sistem özellikleri ve tüm .NET ek yükü içerebilir toplam ileti boyutu 256 k sınırı olduğunu unutmayın. |İleti yükü azaltın ve işlemi yeniden deneyin. |Yeniden deneme yardımcı olmayacaktır. |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) belirli bir varlık için bir kota aşıldı gösterir.

Alıcıları (5) en fazla bir tüketici başına grubu düzeyinde açılan bu durum meydana gelebilir.

### <a name="event-hubs"></a>Event Hubs
Event Hubs olay hub'ı başına 20 tüketici grupları sınırı vardır. Daha fazla oluşturma girişiminde bulunduğunuzda aldığınız bir [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

## <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) kullanıcı tarafından başlatılan bir işlem işlem zaman aşımından daha uzun sürüyor gösterir. 

Olay hub'ları için zaman aşımı parçası olarak, bağlantı dizesi veya aracılığıyla belirtilen [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). Hata iletisi farklılık gösterebilir, ancak her zaman geçerli işlem için belirtilen zaman aşımı değeri içeriyor. 

### <a name="common-causes"></a>Olası nedenler
Bu hatanın ortak nedenleri vardır: yanlış yapılandırma ya da geçici hizmet hatası.

1. **Yanlış yapılandırma** işlem zaman aşımı işletimsel koşul için çok küçük olabilir. İstemci SDK işlemi zaman aşımı için varsayılan değer 60 saniyedir. Kodunuzu değer olup olmadığını görmek için çok küçük bir şeye ayarlayın denetleyin. Ağ ve CPU kullanımı koşulunu işlem zaman aşımı çok düşük bir değere ayarlanmamalıdır şekilde belirli bir işlemin tamamlanması için geçen süreyi etkileyebilir unutmayın.
2. **Geçici hizmet hatası** bazen Event Hubs hizmeti isteklerini işleme; Örneğin, yüksek trafiği dönemlerinde gecikme. İşlem başarılı olana kadar bu gibi durumlarda bir gecikmeden sonra işlemi yeniden deneyebilir. Aynı işlemi birden çok denemeden sonra yine başarısız olursa, lütfen ziyaret [Azure hizmet durumu site](https://azure.microsoft.com/status/) bilinen bir hizmet kesintileri olup olmadığını görmek için.

## <a name="serverbusyexception"></a>ServerBusyException

A [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) veya [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) bir sunucusu aşırı yüklendi gösterir. Bu özel durumun iki ilgili hata kodları vardır.

### <a name="error-code-50002"></a>Hata kodu 50002

Bu hata iki nedenlerden biriyle oluşabilir:

1. Yükü olay hub'ındaki tüm bölümler arasında eşit olarak dağıtılmaz ve yerel işleme birimi sınırlaması bir bölüm isabetler.
    
    Çözüm: bölüm Dağıtım stratejisi düzeltilmesi veya çalışırken [EventHubClient.Send(eventDataWithOutPartitionKey)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) yardımcı olabilir.

2. Olay hub'ları ad alanı yeterli üretilen iş birimleri yok (kontrol edebilirsiniz **ölçümleri** olay hub'ları ad alanı penceresinde ekran [Azure portal](https://portal.azure.com) onaylamak için). Portal toplanmış (1 dakika) bilgileri gösterir, ancak yalnızca bir tahmin olacak şekilde size gerçek zamanlı – iş hacmini ölçmek unutmayın.

    Çözüm: ad alanında üretilen iş birimleri artırma yardımcı olabilir. Portalında, bunu yapabilirsiniz **ölçek** olay hub'ları ad alanı ekranın penceresi.

### <a name="error-code-50001"></a>Hata kodu 50001

Bu hata nadiren olmalıdır. Ad alanınız için bir kod çalıştırmasını kapsayıcı CPU üzerinde düşük – olay hub'ları yük dengeleyici önce geçmeyen birkaç saniye başlar olduğunda ortaya çıkar.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
