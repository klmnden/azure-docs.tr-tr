---
title: Mesajlaşma özel durumları - Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs Mesajlaşma özel durumları ve Önerilen Eylemler listesini sağlar.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: a6ebfc86a2489910d23faa96550f34cc979c0435
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60203440"
---
# <a name="event-hubs-messaging-exceptions"></a>Event Hubs mesajlaşma özel durumları

Bu makalede .NET Framework olay hub'ları API'ları içeren Azure Service Bus Mesajlaşma API'si kitaplığı tarafından oluşturulan özel durumları bazıları listelenmektedir. Bu başvuru değişebilir, bu nedenle geri Güncelleştirmeleri denetle.

## <a name="exception-categories"></a>Özel durum kategorisi

Olay hub'ları API'leri, bunları çözmek için gerçekleştirebileceğiniz ilişkili bir eylem ile birlikte şu kategorilere giren özel durumlar oluşturun.

1. Kodlama hatası kullanıcı: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). Genel eylem: devam etmeden önce kod düzeltmeye çalışın.
2. Kurulumu/yapılandırma hatası: [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Genel eylem: yapılandırmanızı inceleyin ve gerekirse değiştirin.
3. Geçici özel durumlar: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](#serverbusyexception), [Microsoft.Azure.EventHubs.ServerBusyException](#serverbusyexception), [ Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). Genel eylem: işlemi yeniden deneyin veya kullanıcılara bildirin.
4. Diğer özel durumlar: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](#timeoutexception), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [ Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception). Genel eylem: özel durum türü; özel aşağıdaki bölümdeki tabloya bakın. 

## <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tabloda, Mesajlaşma özel durum türlerini ve nedenler ve notları önerilen eylem uygulayabileceğiniz listeler.

| Özel Durum Türü | Neden/açıklama/örnekleri | Önerilen eylem | Otomatik/hemen yeniden deneme sırasında dikkat edin. |
| -------------- | -------------------------- | ---------------- | --------------------------------- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Sunucu istenen işlemi tarafından denetlenen belirtilen süre içinde yanıt vermedi [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings). Sunucu istenen işlemi tamamlanmamış olabilir. Bu durum, ağ veya diğer altyapı gecikmeler nedeniyle oluşabilir. |Tutarlılık için sistem durumunu denetleyin ve gerekirse yeniden deneyin.<br /> Bkz: [TimeoutException](#timeoutexception). | Bazı durumlarda yeniden başlatma yardımcı; yeniden deneme mantığı için kod ekleyin. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |İstenen kullanıcı işlemi, sunucu veya hizmet içinde izin verilmiyor. Özel durum iletisi ayrıntıları için bkz. Örneğin, [tam](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) iletisi alındı bu özel durum oluşturur [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu. | Kod ve belgelere bakın. İstenen işlem geçerli olduğundan emin olun. | Yeniden deneme yardımcı olmaz. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) | Zaten kapalı, durduruldu veya atılmış bir nesne üzerinde bir işlem çağırmak için bir deneme yapılır. Nadiren de olsa, ortam işlem zaten atıldı. | Kodu kontrol edip atılan nesneye işlemleri çağırma kullanılamaz olduğundan emin olun. | Yeniden deneme yardımcı olmaz. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) | [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) nesne bir belirteci alınamadı, belirteci geçersiz veya işlemi gerçekleştirmek için gerekli talep belirteci içermiyor. | Belirteç sağlayıcısı, doğru değerlerle oluşturulduğundan emin olun. Erişim denetimi hizmetinin yapılandırmasını denetleyin. | Bazı durumlarda yeniden başlatma yardımcı; yeniden deneme mantığı için kod ekleyin. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[Üretiliyor](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) | Yöntemine sağlanan bir veya daha fazla bağımsız değişken geçersiz. Sağlanan URI [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Oluştur](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) yolu segment(s) içerir. Sağlanan URI düzeni [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Oluştur](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) geçersiz. Özellik değeri, 32 KB'den daha büyük. | Çağıran kod denetleyin ve bağımsız değişkenlerin doğru olduğundan emin olun. | Yeniden deneme yardımcı olmaz. |
| [Microsoft.ServiceBus.Messaging MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /><br/> [Microsoft.Azure.EventHubs MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) | İşlemle ilişkili varlık yok veya silinmiş olabilir. | Varlığın mevcut olduğundan emin olun. | Yeniden deneme yardımcı olmaz. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) | İstemci, olay hub'ına bağlantı kuramıyor. |Sağlanan ana bilgisayar adının doğru olduğundan ve konağın erişilebilir olduğundan emin olun. | Aralıklı bağlantı sorunu yoksa, yeniden deneme yardımcı olabilir. |
| [Microsoft.ServiceBus.Messaging ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> <br/>[Microsoft.Azure.EventHubs ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) | Hizmet şu anda isteğinizi mümkün değil. | İstemci bir süre bekleyin. ardından işlemi yeniden deneyin. <br /> Bkz: [ServerBusyException](#serverbusyexception). | İstemci, belirli bir süre sonra yeniden deneyebilir. Farklı bir özel durum yeniden oluşur, o özel durumu yeniden deneme davranışını kontrol edin. |
| [Istransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) | Genel, aşağıdaki durumlarda oluşturulan özel durum iletileri: Oluşturmak için bir girişimde bir [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) adınızı veya ait farklı bir varlık türü için (örneğin, bir konu) yolu. 1 MB'tan daha büyük bir ileti göndermek için bir deneme yapılır. Sunucu veya hizmet isteğinin işlenmesi sırasında bir hatayla karşılaştı. Özel durum iletisi ayrıntıları için bkz. Bu durum genellikle geçici bir istisnadır. | Kodunu kontrol edin ve yalnızca serileştirilebilir nesneler ileti gövdesi için kullanıldığından emin olun (veya özel bir serileştirici kullanın). Özelliklerin desteklenen değer türleri ve yalnızca desteklenen kullanım türleri için belgelere bakın. Denetleme [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) özelliği. Eğer öyleyse **true**, işlemi yeniden deneyin. | Yeniden deneme davranışı tanımsızdır ve yardımcı. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) | Bu hizmet ad alanındaki başka bir varlık tarafından zaten kullanılan bir ada sahip bir varlık oluşturmaya çalışır. | Var olan bir varlığa silin veya oluşturulacak varlığın farklı bir ad seçin. | Yeniden deneme yardımcı olmaz. |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) | Mesajlaşma varlığı, izin verilen boyut üst sınırına. Bu özel durumun (olan 5) sayısı alıcılar bir tüketici her grubu düzeyinde açılan oluşabilir. | Alanı varlık içinde varlık veya onun alt iletilerini alma oluşturun. <br /> Bkz: [QuotaExceededException](#quotaexceededexception) | İletiler sırada kaldırdıysanız, yeniden deneme yardımcı olabilir. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) | Bir çalışma zamanı işlemi devre dışı bırakılmış bir varlık için isteği. |Varlık etkinleştirin. | Bu arada varlık etkinleştirdiyseniz, yeniden deneme yardımcı olabilir. |
| [Microsoft.ServiceBus.Messaging MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /><br/> [Microsoft.Azure.EventHubs MessageSizeExceededException](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | İleti yükü 1 MB sınırını aşıyor. Sistem özellikleri ve herhangi bir .NET yükü içerebilen toplam ileti için bu 1 MB sınırdır. | İleti yükü azaltın, ardından işlemi yeniden deneyin. |Yeniden deneme yardımcı olmaz. |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) belirli bir varlığa kotasının aşıldığını gösterir.

Bu özel durum sayısı (5) alıcılar bir tüketici her grubu düzeyinde açılan oluşabilir.

### <a name="event-hubs"></a>Event Hubs
Event Hubs olay hub'ı başına 20 tüketici grubu sınırı vardır. Daha fazla oluşturmaya çalıştığınızda, aldığınız bir [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

## <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) kullanıcı tarafından başlatılan bir işlemin işlemi zaman aşımı süresinden daha uzun sürdüğünü gösterir. 

Event Hubs için zaman aşımı veya bağlantı dizesinin veya aracılığıyla bir parçası olarak belirtilen [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). Hata iletisi farklılık gösterebilir, ancak her zaman geçerli işlem için belirtilen zaman aşımı değeri içerir. 

### <a name="common-causes"></a>Olası nedenler
Bu hata sık karşılaşılan iki nedeni vardır: yanlış yapılandırma veya bir geçici hizmet hatası.

1. **Yanlış yapılandırma** işlem zaman aşımı işletimsel koşul için çok küçük olabilir. İstemci SDK'sı işlemi zaman aşımı'için varsayılan değer 60 saniyedir. Kodunuzu değer olup olmadığını görmek için çok küçük bir şeye ayarlayın denetleyin. Ağ ve CPU kullanımı koşulu işlem zaman aşımı için küçük bir değere ayarlanmamalıdır böylece uygulamanın belirli bir işlemin tamamlanması için geçen süreyi etkileyebilir unutmayın.
2. **Geçici bir hizmet hatası** bazen Event Hubs hizmeti istekleri işliyor; Örneğin, yüksek trafiği dönemlerinde gecikme. Bu gibi durumlarda, işlemi başarılı olana kadar bir gecikmeden sonra işlemi yeniden deneyebilirsiniz. Aynı işlemi birden çok denemeden sonra yine başarısız olursa ziyaret [Azure hizmet durumu sitesine](https://azure.microsoft.com/status/) herhangi bir bilinen hizmet kesintileri olup olmadığını görmek için.

## <a name="serverbusyexception"></a>ServerBusyException

A [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) veya [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) bir sunucu aşırı yüklenmiş olduğunu gösterir. Bu özel durumun iki ilgili hata kodları vardır.

### <a name="error-code-50002"></a>Hata kodu 50002

Bu hata iki nedenlerden birinden dolayı oluşabilir:

1. Yük olay hub'ındaki tüm bölümler arasında eşit olarak dağıtılmaz ve bir bölüm yerel aktarım hızı birimi sınırlama denk gelir.
    
    Çözüm: Bölüm Dağıtım stratejisi düzeltilmesi veya çalışırken [EventHubClient.Send(eventDataWithOutPartitionKey)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient) yardımcı olabilir.

2. Yeterli işleme birimleri Event Hubs ad alanı yok (denetleyebilirsiniz **ölçümleri** olay ad alanı penceresinde Hubs ekran [Azure portalında](https://portal.azure.com) onaylamak için). Portal toplanan (1 dakika) bilgileri gösterir, ancak yalnızca tahmin, bu nedenle size gerçek zamanlı işleme ölçün.

    Çözüm: Üretilen iş birimlerine ad alanına göre artan yardımcı olabilir. Portal, bu işlemi de yapabilirsiniz **ölçek** Event Hubs ad alanı ekranın penceresi. Veya, kullanabileceğiniz [otomatik şişme](event-hubs-auto-inflate.md).

### <a name="error-code-50001"></a>Hata kodu 50001

Bu hata nadiren gerçekleşmesi. Ad alanınız için kod kapsayıcıyı CPU üzerinde düşük – başlamadan önce olay hub'ları yük dengeleyici fazla birkaç saniye ortaya çıkar.


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
